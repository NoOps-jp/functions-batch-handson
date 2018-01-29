# Module06: Function Appの出力先としてCosmosDBを設定する

* 公式ドキュメント: [Azure Functions の Azure Cosmos DB バインド](https://docs.microsoft.com/ja-jp/azure/azure-functions/functions-bindings-cosmosdb#output)

## 1. プロジェクトでCosmos DB バインドを使えるようにする

1. Visual Studioでソリューションファイル```funnctions-batchapps.sln```を開きます。

1. ```AajpFunctions```プロジェクトに、NuGet パッケージ ```Microsoft.Azure.WebJobs.Extensions.DocumentDB```を追加します。

    > パッケージのバージョンを必ず1.0.0にして下さい。最新の1.1.0を導入すると現時点ではエラーとなります。

## 2. Function用のクラスを追加する

1. ```AajpFunctions```プロジェクトに空のクラスを追加します。クラス名は```CosmosBinding```等にしておきます（自由）。

1. 追加したクラスに以下のようなコードを追加します（namespaceやclass名はプロジェクトに合わせて下さい)。

    ```cs
    using System.Threading;
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Host;
    using Microsoft.Extensions.Logging;

    namespace AajpFunctions
    {
        // Function実行クラス
        public static class CosmosBinding
        {
            [FunctionName("CosmosBinding")]
            public static void Run(
                [QueueTrigger("stock-queue-items")] Stock myQueueItem,
                [DocumentDB("Stocks", "Price", Id = "id", ConnectionStringSetting = "CosmosDbConnectionString")] out dynamic document,
                ILogger logger)
            {
                logger.LogInformation($"Request for item with Ticker={myQueueItem.Ticker}.");

                // Cosmos DB出力バインディングを使ってデータを保存
                document = new
                {
                    ticker = myQueueItem.Ticker,
                    stockName = myQueueItem.StockName,
                    price = decimal.Parse(myQueueItem.Price),
                    volume = decimal.Parse(myQueueItem.Volume),
                    stockValue = decimal.Parse(myQueueItem.Price) * decimal.Parse(myQueueItem.Volume)
                };

                Thread.Sleep(3000); // 一定の処理負荷をシミュレートするため、擬似的に待機を入れる
            }
        }

        // Cosmos DBに格納するJSONのデータ型
        public class Stock
        {
            public string Ticker { get; set; }
            public string StockName { get; set; }
            public string Price { get; set; }
            public string Volume { get; set; }
        }
    }
    ```

## 3. 設定ファイルにCosmos DBへの接続情報を追加する

1. ```AajpFunctions```プロジェクト内の```local.settings.json```を開き、[module05](module05.md)でメモしておいたCosmos DBの接続文字列を```"AzureWebJobsStorage"```に入力して保存します。

    ```json
    {
        "IsEncrypted": false,
        "Values": {
            "AzureWebJobsStorage": "DefaultEndpointsProtocol=https;AccountName=xxxxxx;AccountKey=xxxxxxx4bVMg==;EndpointSuffix=core.windows.net",
            ""AzureWebJobsDashboard": DefaultEndpointsProtocol=https;AccountName=xxxxxx;AccountKey=xxxxxxx4bVMg==;EndpointSuffix=core.windows.net",
            "CosmosDbConnectionString"; "AccountEndpoint=https://xxxxxxxx.documents.azure.com:443/;AccountKey=b8PM2D........oLzm254IA==;"
        }
    }
    ```

## 4. Functionの動作確認を行う

### キューを追加する

1. Azure Storage Explorerを使って、Functionに対応するストレージアカウントを展開します。

1. [Queues] を右クリックし、 [Create Queue]を選択します。

1. Queueの名前を入力します（```CosmosBinding```クラスのQueueTrigger属性で定義しているキュー名）

    * 上記コード例の場合は```stock-queue-items```

### Functionのローカル起動

1. Visual Studioの```funnctions-batchapps```ソリューションにある```AajpFunctions```をスタートアッププロジェクトに設定します。

1. デバッグで起動します。
    
    ```CosmosBinding```クラスの```Run```メソッド内に適宜ブレイクポイントをいれておくとキュートリガー起動時の動きを確認できます。

### キューにメッセージを追加してFunctionを呼ぶ

Azure Storage Explorerを使って、キューにメッセージを追加します。

1. [対象のストレージアカウント] - [Queues] - [作成したキュー名] を選択し、ダブルクリックします。

1. [作成したキュー名]タブの [+ Add Message] ボタンをクリックします。

1. Add Messageダイアログに、以下のJSON文字列を入力します。

    ```json
    {
        "ticker": "0002",
        "stockName": "サンプル2",
        "price": 200,
        "volume": 1000,
        "stockValue": 200000
    }
    ```
    - Expireは**1 Minutes**などに変更します
    - [Encode message body in Base64] はチェックしたままにします。

### Functionの動作を確認

ローカルで起動しているFunction App ```BasicQueueTriggerApp``` にメッセージが到達していることを確認します。