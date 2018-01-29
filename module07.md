# Module07: Cosmos DB 出力バインディングFunctionを追加デプロイする

このステップでは、既存のFunction App（[module04](module04.md)でデプロイしたFunction App）に関数を追加します。

## 1. Visual Studio 2017を使ったデプロイ

1. Visual Studioでデプロイするソリューションを開きます。

1. デプロイするプロジェクト ```AajpFunctions``` を右クリックし > **[発行]** をクリックします。

1. デプロイが完了するまで数分待ちます。

    出力コンソールに ```Publish completed``` と表示されれば完了です。

## 2. Visual Studio for Macを使ったデプロイ

Visual Studio for MacではGUIを使ったAzureへのFunctionデプロイがサポートされていないため、Function AppのCLIツールである[Azure Functions Core Tools]を使います。（ [module02](module02.md)でインストール済み ）

1. Visual Studio for Macでデプロイするソリューションを開きます。

1. Function Appプロジェクト ```AajpFunctions``` を再度ビルドします

1. ターミナルで、 AajpFunctions / bin / Debug(Release) / net461 に移動して以下を実行します

```bash
$ func azure functionapp publish functions-batchapps
```

## 3. ポータルでのデプロイ確認

1. Azureポータルの全体メニューで **Fanction App** をクリックし、デプロイしたFunction Appを選択します。

1. Function App画面で、該当のFunction App以下にある[関数]を展開して、追加したFunction（ ```CosmosBinding``` など）が表示されていればデプロイは完了しています。

## 4. Cosmos DB 接続文字列の追加

Function AppからCosmos DBに接続するための情報を設定します。

1. Function App画面で、該当のFunction App名のルートをクリックします。

1. 構成済みの機能欄にある [アプリケーション設定] リンクをクリックします。

1. アプリケーション設定タブの **[アプリケーション設定]** で以下のようにキーと値を追加して下さい。

    * キー: CosmosDbConnectionString
    * 値: AccountEndpoint=https://xxxxxxxx.documents.azure.com:443/;AccountKey=b8PM2D........oLzm254IA==;

    > 値には、 [module05](module05.md) で記録しておいた Cosmos DB 接続文字列を入れて下さい。

1. アプリケーション設定タブ上部の **保存** アイコンをクリックします。

## 5. キューを使ってFunctionを起動する

[module06](module06.md)で実施したように、Azure Storage Explorerを使って、キューに再度メッセージを追加します。

> ローカルでFunctionがデバッグ実行されていないことを確認して下さい。

1. [対象のストレージアカウント] - [Queues] - [キュー名] を選択し、ダブルクリックします。

    * キューは、module06で作成したキュー名( ```stock-queue-items``` など)を選択します

1. [キュー名]タブの [+ Add Message] ボタンをクリックします。

1. Add Messageダイアログに、以下のJSON文字列を入力します。

    ```json
    {
        "ticker": "0003",
        "stockName": "サンプル3",
        "price": 300,
        "volume": 1000,
        "stockValue": 300000
    }
    ```

    - Expireは**1 Minutes**などに変更します
    - [Encode message body in Base64] はチェックしたままにします。

## 6. デプロイしたFunctionの動作確認

1. Function App画面で、該当のFunction App以下にある[関数] - [追加したFunction名（ ```CosmosBinding``` など）]を展開します。

1. [モニター] をクリックして、Functionの実行結果を確認します。

    ![m07-1](images/m07-1.png)

    > 反映するまで数分時間をおく必要があります。定期的に [Refresh] をクリックして下さい。

## 7. Cosmos DBにデータが格納されていることを確認

1. Azureポータルの全体メニューで **Azure Cosmos DB** をクリックし、対象のデータベースアカウント名を選択します。

1. **Azure Cosmos DB アカウント** ブレードにて、 **データエクスプローラー** をクリックします。

1. [Stocks] データベースの [Price] コレクションをクリックします。

1. 画面上部の [New SQL Query] をクリックして以下のSQLを発行します。

    ```sql
    SELECT * FROM c WHERE c.ticker = '0003'
    ```

1. JSONデータが表示されれば問題ありません。

---
[Back](module06.md) | [Next](module08.md)