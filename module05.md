# Module05: Azure Cosmos DB の構築

## 1. Azure Cosmos DBアカウントの作成

1. Azureポータルにて、 **+リソースの作成** > **Databases** > **Azure Cosmos DB** をクリックします。

1. **New account** 画面では以下を参考に設定して下さい。

    * ID: 任意の文字列（半角英数小文字とハイフンのみ）
    * API: **SQL** を選択
    * サブスクリプション: ハンズオン用に用意したサブスクリプション
    * リソースグループ: [module0](module0.md)で作成したリソースグループを選択（新規作成しない）
    * 場所: Function Appをデプロイした場所と同一
    * **Enable geo-redundancy** のチェックはここでは外しておく
    * Virtual Networks は **Disabled** を選択

    **作成** をクリックします。

1. デプロイが完了するまで数分待ちます。

    画面右上の通知領域でデプロイ状況がわかりますので、データベースアカウントが作成できたことを確認します。

## 2. コレクションの作成

1. 全体メニューで **Azure Cosmos DB** をクリックし、作成したデータベースアカウントを選択します。

    全体メニューに表示されない場合は、メニュー左上「すべてのサービス」から探し、スターを付けておきます。

1. **Azure Cosmos DB account** ブレードにて、 **Data Explorer** > **New Collection** をクリックします。

1. Add Collection画面では以下のように設定して下さい。

    * Database id: **Stocks** (Provision database throughputのチェックはつけない)
    * Collection id: **Price**
    * Storage capacity: **Fixed (10GB)** を選択
    * Throughput: **1000**
    * Unique keys: 設定しない

    > Database名やCollection名に大文字、小文字の使い分けは任意ですが、クライアントアプリケーションの設定値等に影響しますので、注意して下さい。

    ![Collection作成画面](./images/m05-1.png)

1. **OK** をクリックします。

## 4. サンプルデータの追加

1. Data ExplorerのCOLLECTIONペインにて、 **Stocks** データベース内の **Price** コレクションを展開し、 **Documents** をクリックします。

1. **Documents** タブで、 **New Document** をクリックします。

1. 以下を参考にJSONドキュメントを編集します。

    ```JSON
    {
        "ticker": "0001",
        "stockName": "サンプル1",
        "price": 500,
        "volume": 1000,
        "stockValue": 500000
    }
    ```

    > `id`は記述を省略すると自動で採番されます。

1. **Documents** タブで、**Save**アイコンをクリックします。

## 5. データの確認

1. 追加されたデータは、**Documents** タブのリストからidを選択して確認することができます。

    > ```_rid```などアンダースコアから始まる項目は、Cosmos DBが内部で利用する管理データです。

1. そのままJSONを編集し、 **Save** アイコンをクリックすると、データを変更できます。

    > 管理データは編集や削除をしないようにして下さい。

### (参考)　Azure Storage Explorer を使う

デスクトップアプリケーションである **Azure Storage Explorer** を使って、Azure Cosmos DBのデータを操作することができます。

* 利用手順: [Azure Cosmos DB を Azure Storage Explorer で管理する](https://docs.microsoft.com/ja-jp/azure/cosmos-db/storage-explorer)

## 6. Cosmos DBの接続文字列を記録しておく

後続の作業で使うため、Cosmos DBの接続文字列を手元にメモしておきます。

1. **Azure Cosmos DB account** ブレードにて、 **Keys** を選択します。

1. **PRIMARY CONNECTION STRING**をコピーします。

1. 手元のテキストファイル等に保存しておきます

---
[Back](module04.md) | [Next](module06.md)