## WebAppsを作る

[http://portal.azure.com](http://portal.azure.com)にアクセスし、左上の**新規**ボタンを押します。

![21](img/21.png)

**Web+モバイル**から**Web App**を選択します。

![22](img/22.png)

**アプリ名**はWebAppsのURLとなります。好きなIDを入力してください。

**リソースグループ**はAzureのリソースをグループ化するものです。適当な名前をつけてください。

![23](img/23.png)

**App Servivceプラン/場所**をクリックします。

**新規作成**を押し、**AppServiceプラン**に適当な名前を入れます。**場所**はJapanであればどこでも大丈夫です。価格レベルは**F1 Free**です。

ここまでできたら下にある**OK**を押します。

![24](img/24.png)

**ダッシュボードにピン留めする**にチェックを入れ、**作成**ボタンを押します。

![25](img/25.png)

Web Appsができました。ポータルの左のリストにある**すべてのリソース**から作成したWebAppsの管理画面を表示することができます。

管理画面に書いてある**URL**をメモ帳にコピーしておきます。このURLはかなり後で使うことになります。

![35](img/35.png)

## プログラムの配置をする

**デプロイ資格情報**を押し、好きな名前とパスワードを設定し、**保存**を押します。

![30](img/30.png)

続いて**デプロイオプション**を押します。

![26](img/26.png)

**ソースの選択**から**ローカルGitリポジトリ**を押します。

![27](img/27.png)

**OK**を押します。

再び**概要**に戻り、**GitクローンURL**に書いてあるURLをコピーします。

![28](img/28.png)

シェルを立ち上げます。Macの方は**ターミナル**アプリを、Windowsの方は**PowerShell**を立ち上げます。

![29](img/29.png)

好きなディレクトリで先ほどコピーしたWebAppsのGitクローンURLをクローンします。

Windowsの方は右クリックでクリップボードの内容を貼り付けることができます。

```sh
git clone {WebAppsに表示されているGitクローンURL}
```

途中で認証情報を聞かれるので**デプロイ資格情報**で設定したユーザー名とパスワードを入れます。

![31](img/31.png)

Windowsのエクスプローラーでフォルダを見ると、cloneした場所に新しくWebAppsと同じ名前のフォルダができています。

![32](img/32.png)

このcloneしたフォルダの中に、ダウンロードしたサンプルコードの**line/code/**の中に入っている**package.json**と**server.js**ファイルをコピーします。

![33](img/33.png)

シェルに戻り、cdコマンドでcloneしたフォルダに移動します。

```
cd {cloneしたフォルダ}
```

lsコマンドを使うと以下のように、package.jsonとserver.jsが表示されればOKです。

![34](img/34.png)

カレントディレクトリをcloneしたフォルダに移せたら以下のコマンドを入力します。

WindowsのPowerShellの人
```sh
echo "node_modules/"|Out-File .gitignore -Encoding utf8
```
Macのターミナルアプリの人
```sh
echo "node_modules/">.gitignore
```

最後に、npm installコマンドを入力します。

```sh
npm install
```


エラーがおこらなければOKです。


## LINEデベロッパーポータルにログインする
この記事ではすでにLINEアカウントを持っているものとして進めます。
もしLINEアカウントを持っていない人はスタッフに声をかけてください。

[https://business.line.me/ja/](https://business.line.me/ja/)にアクセスします。

右上の**ログイン**を押します。

![1](img/1.png)

メールアドレスをパスワードを入力して**ログイン**を押します。

![2](img/2.png)

認証が入った場合は認証を通すために表示されたパスワードをLINEアプリを開いて入力します。

ログインできたら、右上にある**会社/事業者未選択**というところを押します。

![3](img/3.png)

緑色の**会社/事業者を追加する**を押します。

各種項目を入力し、**確認する**ボタンを押します。その後、情報を確認できたら**完了する**ボタンを押します。

右上に事業者名と名前が表示されたらOKです。

![5](img/5.png)

## Messaging APIを利用する

次に少し下にスクロールし、一番右の**Messaging API**を押します。

![6](img/6.png)

左下の**Messaging APIを始める**を押します。

![7](img/7.png)

適当な名前を入れます。アイコンはつけても付けなくても大丈夫です。

業種も適当なものを選択し、**確認する**ボタンを押します。

![8](img/8.png)

**申し込む**ボタンを押します。

![9](img/9.png)

**LINE@MANAGERへ**ボタンを押します。

![10](img/10.png)

## Botの設定をする

Bot設定の画面にいくと思いますが、Bot設定画面に行かなかった場合、左のリストから**アカウント設定**>**Bot設定**を押してください。

Bot設定の画面から**APIを利用する**ボタンを押します。

![11](img/11.png)

**確認**ボタンを押します。

![12](img/12.png)

リクエスト設定から**Webhook送信**を**利用する**にします。

詳細設定から**Botのグループトーク参加**を**利用する**にします。

![13](img/13.png)

できたら**保存**ボタンを押します。

![14](img/14.png)

続いて左のリストから**メッセージ**>**自動応答メッセージ**を選択します。

**delete**を押して現在有効になっている自動応答メッセージを削除します。

![43](img/43.png)

## BotをAzureとつなげる

**アカウント設定**>**Bot設定**画面で、ステータスの下にある**LINE Developersで設定する**を押します。

![15](img/15.png)

下の方にスクロールすると**Webhook URL**と**Channel Access Token**と書いてある項目があります。

**EDIT**ボタンを押します。

![16](img/16.png)

Webhook URLの項目に**自分のAzure WebAppsのURLをhttpsにしたものを入れます**(メモ帳にコピーしておいたものです)

**http://**{自分のWebAppsのID}.azurewebsites.net

となっているURLを

**https://**{自分のWebAppsのID}.azurewebsites.net

にするということです。

入力できたら**SAVE**ボタンを押します。

![17](img/17.png)

再び画面が戻ったら、次は**Channel Access Token**の横にある**ISSUE**ボタンを押します。

Channel Access Tokenを発行できたら、**メモ帳にコピーしておきます**

![18](img/18.png)

## プログラムを編集する

プログラムを編集するために**VisualStudio Code**を立ち上げます。

![36](img/36.png)

VisualStudio Codeから**ファイル**>**フォルダーを開く**を押します。

![37](img/37.png)

クローンしておいたWebAppsの名前と同じ名前の、package.jsonとserver.jsを置いたフォルダを選択します。

![38](img/38.png)

左のリストに**server.js**が表示されるのでクリックして開き、**{yout channel access token here}**と書いてある部分に先ほどメモ帳にコピーしておいた channel access tokenを貼り付けます。

![39](img/39.png)

![40](img/40.png)

## プログラムをWebAppsへ配置する

今回作成したWebAppsでは、Git PushをするとWebApps上にプログラムを配置することができます。

先ほどのnpm installをしたシェルをもう一度表示し、以下のコマンドを入力します。

{commit message}のところは好きなメッセージにしてください。

```sh
git add .
git commit -m "{commit message}"
git push
```

Deployment successfulと表示されればOKです。

![41](img/41.png)

## Lineからメッセージを送ってみる

スマートフォンのLINEアプリを起動します。

**友達追加**>**QRコード**を押し、開いていたLINE Developersのサイトに表示されているQRコードを読み込みます。

![42](img/42.png)

読み込んだら**追加**を押し、**同意する**を押します。

Botと話してみましょう。Botから応答が帰ってきたらOKです。

![44](img/44.png)