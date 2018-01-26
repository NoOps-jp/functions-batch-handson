# Module03: Azure Storage ExplorerでのQueue Storage操作

## 1. Azure Storage Explorer を使う

デスクトップアプリケーションである **Azure Storage Explorer** を使って、Azure Storageのキューを操作することができます。Windows、Linux、および MAC バージョンがサポートされています。

### インストール

TBD

### アカウント設定

TBD

## 2. キューを作成する

Functionをトリガーするためのキューをストレージアカウントに作成します。

TBD

## 3. キュートリガーFunctionの動作確認

### Functionのローカル起動

VSの```funnctions-batchapps```ソリューションにある```BasicQueueTriggerApp```をスタートアッププロジェクトに設定します。

デバッグで起動します。```BasicFunction```クラスの```Run```メソッド内に適宜ブレイクポイントをいれておくとキュートリガー起動時の動きを確認できます。

### キューにメッセージを追加

Azure Storage Explorerを使って、2で作成したキューにメッセージを追加します。

TBD