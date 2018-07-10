# Module02: キュートリガーFunctionの作成

## 1. 開発環境の確認

このハンズオンに必要な環境は以下の通りです。

* Visual Studio 2017の場合

    * Visual Studio 2017 Community Edition (Proffesional, Enterpriseも可): [ダウンロード](https://www.visualstudio.com/ja/free-developer-offers/)
    * Version 15.5.0以上を推奨
    * コンポーネントとして 「Microsoft Azure Webjob ツール」がインストールされていること

* Visual Studio for Macの場合

    * Visual Studio for Mac: [ダウンロード](https://www.visualstudio.com/ja/free-developer-offers/)
    * Version 7.2以上を推奨

* Azure Functions Core Toolsのインストール（Macは必須）

    CLIベースでFunctionsのローカルデバッグやデプロイなどができるようになります。

    Macでのインストールは以下の通りです（Homebrewを使ってインストールします）。
    ```bash
    brew tap azure/functions
    brew install azure-functions-core-tools
    ```

    * CLIのインストールにはNode.jsが必要です。macにインストールされていない場合は [Node.js](https://nodejs.org/ja/) 公式ページからインストーラをダウンロードして導入して下さい。

## 2. ハンズオン用ソースコードの入手

* Gitで入手する

    プロジェクト保存用ディレクトリに移動して以下を実行して下さい。

    ```bash
    git clone https://github.com/zenarchitects/functions-batchapps.git
    ```

* zipをダウンロードする（Gitを使わない場合）

    * リポジトリ: https://github.com/zenarchitects/functions-batchapps

    Githubリポジトリの **clone or download**ボタンから **Download ZIP**を実行し、適当なディレクトリに解凍して下さい。

---
[Back](module01.md) | [Next](module03.md)