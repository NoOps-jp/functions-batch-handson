# Module02: キュートリガーのFunction App作成(C#)

## 1. 開発環境の確認

このハンズオンに必要な環境は以下の通りです。

* Visual Studio 2017の場合

    * Visual Studio 2017 Community Edition (Proffesional, Enterpriseも可): [ダウンロード](https://www.visualstudio.com/ja/free-developer-offers/)
    * Version 15.5.0以上を推奨
    * ASP.NETとAzure開発用の主要コンポーネントがインストールされていること
    * .NET Core 2系のSDK: [SDKのダウンロード](https://www.microsoft.com/net/download/windows)

* Visual Studio for Macの場合

    * Visual Studio for Mac: [ダウンロード](https://www.visualstudio.com/ja/free-developer-offers/)
    * Version 7.2以上を推奨
    * .NET Core 2系のSDK: [SDKのダウンロード](https://www.microsoft.com/net/download/macos)

## 2. ハンズオン用ソースコードの入手

* Gitで入手する

    プロジェクト保存用ディレクトリに移動して以下を実行して下さい。

    ```bash
    $ git clone https://github.com/zenarc/aajp1218-todo.git
    ```

* zipをダウンロードする

    * リポジトリ: https://github.com/zenarc/aajp1218-todo

    Githubリポジトリの **clone or download**ボタンから **Download ZIP**を実行し、適当なディレクトリに解凍して下さい。

## 3. ストレージアカウントへの接続情報をセットする

1. Visual Studioからソースコードのディレクトリにあるソリューションファイル```aajp1218.sln```を開きます。

1. ```appsettings.Development.json```を開き、以下のように接続用の各種文字列を変更して保存します。

    ```json
    "AppConfiguration":  {
        "Endpoint": "Azureポータルから取得したURI",
        "Key": "Azureポータルから取得したプライマリキー",
        "DatabaseId": "コレクション作成時に指定したDatabase id",
        "CollectionId": "コレクション作成時に指定したCollection id",
        "Region": "Cosmos DBアカウント作成時に指定したリージョン名(※1)"
    }
    ```
    ※1) リージョン名は、 指定された文字列を入れる必要があります。
    * 東日本: ```Japan East```
    * 西日本: ```Japan West```
    * その他: [Azure regions](https://azure.microsoft.com/en-us/regions/)のRegion文字列を使って下さい。

## 4. 動作確認を行う

1. Visual Studioにてデバッグを実行し、ブラウザに以下のような画面が表示されれば問題ありません。

    ![デバッグ実行結果](./images/module4-1.png)

## Op. アプリケーションを複数リージョンに対応させる

複数リージョンにデプロイしたCosmos DBの場合、読み取りリージョンをアプリケーションに最も近いリージョンに向けることが可能です。

.NET Coreのアプリケーションでは```DocumentClient```生成時に以下のように```PreferredLocations```プロパティを追加し、リージョン名を設定するようにして下さい。

```c#
 // 接続ポリシーの作成
ConnectionPolicy cp = new ConnectionPolicy();

// このAppのリージョンを追加（西日本リージョンを指定する例）
cp.PreferredLocations.Add("Japan West");

// CosmosDB接続クライアント作成
_client = new DocumentClient({ENDPOINT}, {KEY}, cp);
```

ハンズオン用のソースコードでは、設定ファイル```appsettings.Development.json```からリージョン名を取得するようにしています。そうすることで、App Serviceのアプリケーション設定にて設定値をオーバーライドすることが可能となります。

```PreferredLocations```を設定しなかった場合は、Cosmos DBを複数の地域にグローバル分散させても、アプリケーションからは書き込みリージョンにしかアクセスされません。