# bedrock-line-chat

BedrockとチャットができるLINEのボットを作成します。


## デプロイ方法

1. AWS Serverless Application Repositoryを使用しデプロイする
1. GitHubからソースを取得し、SAMでデプロイする


### デプロイ方法１：AWS Serverless Application Repositoryを使用しデプロイする

1. LINE Messaging APIの設定
    1. [LINE Developers](https://developers.line.biz/)アカウントを作成します。
    1. [こちら](https://developers.line.biz/ja/docs/messaging-api/getting-started/)を参考にチャンネルを作成します。
    1. チャンネルシークレットと[長期のチャネルアクセストークン](https://developers.line.biz/ja/docs/basics/channel-access-token/#long-lived-channel-access-token)を取得します。

2. アプリケーションのデプロイ
    1. [Lambda管理コンソールのアプリケーション](https://us-east-1.console.aws.amazon.com/lambda/home#/applications)にアクセスします。
    1. `アプリケーションの作成`をクリックします。
    1. `サーバーレスアプリケーション`タブの`パブリックアプリケーション`を表示しアプリケーションを検索します。
        * `カスタム IAM ロールまたはリソースポリシーを作成するアプリを表示する`にチェックを入れる
        * 検索欄に「bedrock-line-chat」と入力
    1. アプリケーションの設定に入力します。

        | アプリケーションの設定 | 設定値 |
        | --- | --- |
        | FoundationModel | 使用する基盤モデル（anthropic.claude-v2、anthropic.claude-v1、anthropic.claude-instant-v1） |
        | LineChannelAccessToken | 長期のチャネルアクセストークン |
        | LineChannelSecret | チャンネルシークレット | 
        | NumOfHistory | チャット履歴の件数（デフォルト：10） |

    1. `このアプリがカスタム IAM ロールを作成することを承認します。`にチェックを入れ、`デプロイ`ボタンをクリックします。
    1. `デプロイ`タブに遷移し、SAM テンプレートセクションにある`CloudFormation スタック`のリンクをクリックします。
    1. `出力`タブからLambdaの関数URLのURLを取得します。

1. Lambdaの関数URLのURLをWebhook URLとしてLINE Messaging APIに設定



### デプロイ方法２：GitHubからソースを取得し、SAMでデプロイする

1. LINE Messaging APIの設定
    1. [LINE Developers](https://developers.line.biz/)アカウントを作成します。
    1. [こちら](https://developers.line.biz/ja/docs/messaging-api/getting-started/)を参考にチャンネルを作成します。
    1. チャンネルシークレットと[長期のチャネルアクセストークン](https://developers.line.biz/ja/docs/basics/channel-access-token/#long-lived-channel-access-token)を取得します。


1. SAMのデプロイ

    1. ソースコードの取得

        ```shell
        git clone https://github.com/moritalous/bedrock-line-chat.git
        ```

    1. ビルド

        ```
        sam build
        ```

    1. デプロイ

        ```
        sam deploy --guided
        ```

        パラメーターは以下の値をセットします。

        | パラメーター | 設定値 |
        | --- | --- |
        | LineChannelAccessToken | 長期のチャネルアクセストークン |
        | LineChannelSecret | チャンネルシークレット | 
        | NumOfHistory | チャット履歴の件数（デフォルト：10） |
        | FoundationModel | 使用する基盤モデル（anthropic.claude-v2、anthropic.claude-v1、anthropic.claude-instant-v1） |

        Webhookのリクエストを認証なしの関数URLで受信する設計にしているため、ウィザードの途中で`LineBotFunction Function Url has no authentication. Is this okay? [y/N]:`と聞かれますので、`Y`で回答する必要があります。

1. Lambdaの関数URLのURLをWebhook URLとしてLINE Messaging APIに設定
