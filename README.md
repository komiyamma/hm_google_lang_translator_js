# HmGoogleLangTranslatorJS

![GitHub release (latest by date)](https://img.shields.io/github/v/release/komiyamma/hm_google_lang_translator_js?style=flat&label=Release)
[![MIT](https://img.shields.io/badge/license-MIT-blue.svg?style=flat)](License.txt)
![Hidemaru 9.35](https://img.shields.io/badge/Hidemaru-v9.35-6479ff.svg)

## 概要

Google Apps Script (GAS) を利用して自分専用の翻訳APIを作成し、秀丸エディタからそのAPIを経由してテキストの言語間翻訳を実現するマクロです。

Googleアカウントさえあれば、無料で利用できるGoogleの翻訳機能を、秀丸エディタから手軽に呼び出すことができます。

## 主な機能

*   秀丸エディタで選択しているテキスト、またはファイル全体のテキストを翻訳します。
*   Google翻訳の機能を利用しているため、多くの言語に対応しています。
*   翻訳元と翻訳先の言語を指定できます。（例: 日本語→英語、英語→日本語）

## 動作環境

*   秀丸エディタ Ver.9.35 以上
*   Googleアカウント

## インストール手順

本マクロを利用するには、**①Google Apps Scriptの準備**と**②秀丸マクロの準備**の２つのステップが必要です。

### 1. Google Apps Script (GAS) の準備

まず、Googleのサーバー上で翻訳処理を行うためのAPIを準備します。

1.  Googleアカウントにログインした状態で、[Google Apps Script](https://script.google.com) にアクセスし、「新しいプロジェクト」を作成します。
2.  エディタ画面が開いたら、既存のコードをすべて削除し、以下のコードを貼り付けます。

    ```javascript
    // Getメソッド用
    function doGet(e) {
        var p = e.parameter;
        var translatedText = LanguageApp.translate(p.text, p.source, p.target);
        return ContentService.createTextOutput(translatedText);
    }

    // Postメソッド用
    function doPost(e) {
        var p = JSON.parse(e.postData.getDataAsString());
        var translatedText = LanguageApp.translate(p.text, p.source, p.target);

        var output = ContentService.createTextOutput();
        output.setMimeType(ContentService.MimeType.JSON);
        output.setContent(JSON.stringify({ result: translatedText }));

        return output;
    }
    ```

3.  画面右上の「デプロイ」ボタンを押し、「新しいデプロイ」を選択します。
4.  「種類の選択」で「ウェブアプリ」を選択します。
5.  「アクセスできるユーザー」を「全員」または「自分のみ」に設定し、「デプロイ」ボタンを押します。（「自分のみ」の場合は、スクリプトの実行時にGoogleアカウントへのログインが必要です）
6.  承認を求められた場合は、許可してください。
7.  表示された**ウェブアプリのURL**をコピーして、安全な場所に控えておきます。このURLは次のステップで利用します。

より詳細な手順は、作者様のウェブサイトもご参照ください。
[https://秀丸マクロ.net/?page=nobu_tool_hm_google_app_script_lang_trans](https://秀丸マクロ.net/?page=nobu_tool_hm_google_app_script_lang_trans)

### 2. 秀丸マクロの準備

次に、秀丸エディタ側の設定を行います。

1.  このリポジトリから `.mac` ファイル一式 (`HmGoogleLangTranslatorJS.mac`, `HmGoogleLangTranslator2En.mac`, `HmGoogleLangTranslator2Ja.mac`) をダウンロードし、秀丸エディタのマクロ用フォルダ（通常は `C:\Users\YourName\Documents\Hidemaru\Macro` など）に配置します。
2.  前のステップで取得した**ウェブアプリのURL**をマクロに設定します。設定方法は2通りあります。

    **方法A: 環境変数を設定する（推奨）**
    -   Windowsの環境変数に、変数名 `GOOGLE_SCRIPT_TRANSLATION_API`、変数値にコピーしたURLを設定します。
    -   この方法の利点は、マクロファイルを更新しても設定が消えないことです。

    **方法B: マクロファイルを直接編集する**
    -   `HmGoogleLangTranslatorJS.mac` ファイルをテキストエディタで開きます。
    -   以下の部分を見つけ、`let endpoint_url = "..."` の行のコメントアウト (`//`) を解除し、`***` の部分を自分のURLに書き換えます。

        ```javascript
        // 環境変数から、Google Apps Script の翻訳APIへのURLアドレスを取得。
        let endpoint_url = getenv("GOOGLE_SCRIPT_TRANSLATION_API");

        // 環境変数ではなく、直接ファイルに書いてしまうなら以下を書き換えて、コメントアウトも外す
        // let endpoint_url = "https://script.google.com/macros/s/********************************************/exec";
        ```

## 使い方

マクロの実行は、`HmGoogleLangTranslator2En.mac` や `HmGoogleLangTranslator2Ja.mac` を直接実行します。

*   **英語に翻訳する場合**:
    1.  秀丸エディタで翻訳したいテキストを選択します。（選択しない場合はファイル全体が対象）
    2.  `HmGoogleLangTranslator2En.mac` を実行します。
    3.  翻訳結果が、元のテキストの後ろに挿入されます。

*   **日本語に翻訳する場合**:
    1.  同様にテキストを選択します。
    2.  `HmGoogleLangTranslator2Ja.mac` を実行します。

これらのマクロを秀丸エディタのメニューやキーボードに割り当てると、より便利に利用できます。

`HmGoogleLangTranslatorJS.mac` は、他のマクロから呼び出されるための基盤マクロですので、直接実行しないでください。

## ライセンス

[MIT License](License.txt)

## 作者

[komiyamma](https://github.com/komiyamma)
