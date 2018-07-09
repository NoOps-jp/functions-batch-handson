# Module03: Azure Storageのキュー操作

## 1. Azure Storage Explorer を使う

デスクトップアプリケーションである **Azure Storage Explorer** を使って、Azure Storageのキューを操作することができます。Windows、Linux、および MAC バージョンがサポートされています。

### インストール

[Azure Storage Explorer](https://azure.microsoft.com/ja-jp/features/storage-explorer/)のダウンロードページから各プラットフォーム用のインストーラをダウンロードしてインストールします。

### アカウント設定

Azure Storage Explorerを起動します。
[Manage Accounts] アイコン（人型アイコン）をクリックし、[Add an account..] をクリックして、Azure資格情報を入力します。

### キューを確認する

Azure Storage Explorerを使って、ストレージアカウントを展開し、[module01](module01.md)で作成したキューがあるかどうかを確認します（例: basic-queue-items ）。

### ストレージエミュレーターを起動する

* Windowsの場合

  1. [スタート] を選択するか、Windows キーを押し、「Azure Storage Emulator」と入力します。

  1. 表示されているアプリケーションの一覧からエミュレーターを選択します。

  > タスクトレイのWindowsアイコンをクリックしてエミュレーターが起動していることを確認できます

* Macの場合

  (TBD)

### ローカル実行用のキューを作成する

1. Azure Storage Explorerで、 **(Local and Attached) > Storage Accounts > (Development)** を展開します。

1. **Queues** を右クリック > **Create Queue** を実行します。

1. テキストボックスに[module01](module01.md)で作成したキューと同一名を入力してキューを作成します（例: basic-queue-items ）。

## 3. キュートリガーFunctionの動作確認

### Functionのローカル起動

Visual Studioで `funnctions-batchapps` ソリューションを開き、 `BasicQueueTriggerApp` をスタートアッププロジェクトに設定します。

デバッグで起動します。 `BasicQueueTrigger` クラスの `Run` メソッド内に適宜ブレイクポイントをいれておくとキュートリガー起動時の動きを確認できます。

> Azure Function Toolsのインストールまたはアップデートを求められた場合は、そのまま進めて下さい。

### キューにメッセージを追加

Azure Storage Explorerを使って、キューにメッセージを追加します。

1. **(Local and Attached) > Storage Accounts > (Development)** > [Queues] > [作成したキュー名] を選択し、ダブルクリックします。

1. [作成したキュー名]タブの [+ Add Message] ボタンをクリックします。

1. Add Messageダイアログに、任意の文字列を入力します。（例: "test"など）

    * [Encode message body in Base64] はチェックしたままにします

### Functionの動作を確認

ローカルで起動しているFunction App ```AajpFunctions``` のコンソールに、キューで登録したメッセージが到達したログが以下のように表示されていることを確認します。

```log
[2018/07/11 0:00:00] C# Queue trigger function processed: test
```

---
[Back](module02.md) | [Next](module04.md)