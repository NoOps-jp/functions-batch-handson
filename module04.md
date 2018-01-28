# Function App をデプロイする

## Visual Studio 2017を使ったデプロイ

1. Visual Studioでデプロイするソリューションを開きます。

1. デプロイするプロジェクト ```AajpFunctions``` を右クリックし > **[発行]** をクリックします。

1. 発行画面で **Azure Function App（Azure関数）**を選択し、 **[既存のものを選択]** を選んで、 **[発行]** をクリックします。

1. **[アカウントの追加]** をクリックし、Azure サブスクリプションにサインインします。 既にサインインしている場合は、目的のサブスクリプションを含んだアカウントをドロップダウンから選択します。

1. **[サブスクリプション]** を選択すると、先ほど作成したFunction Appを選択できるので、デプロイしたいWeb App名を選択（ **ここでプライマリとセカンダリのどちらかを選ぶ** ）した状態にして、 **[OK]** をクリックします。

1. デプロイが完了するまで数分待ちます。

## Visual Studio for Macを使ったデプロイ

Visual Studio for MacではGUIを使ったAzureへのFunctionデプロイがサポートされていないため、Function AppのCLIツールである[Azure Functions Core Tools]を使います。（ [module02](module02.md)でインストール済み ）

1. Visual Studio for Macでデプロイするソリューションを開きます。

1. Function Appプロジェクト ```AajpFunctions``` をビルドします

1. ターミナルで、 AajpFunctions / bin / Debug(Release) / net461 に移動して以下を実行します

```bash
$ func azure login
# コンソールの指示に従ってサインイン処理を行う

$ func azure account set d234eb8b-e2a2-4567-a0f6-579f01631743
$ func azure functionapp publish functions-batchapps
```

## ポータルでのデプロイ確認

1. Azureポータルの全体メニューで **Fanction App** をクリックし、[module01](module01.md)で作成したFunction Appを選択します。

    全体メニューに表示されない場合は、メニュー一番下「その他サービス」から探し、スターを付けておきます。

1. Function App画面で、該当のFunction App以下にある[関数]を展開して、[BasicQueueTrigger]というFunctionが表示されていればデプロイは完了しています。

    ![m04-1](images/m04-1.png)
