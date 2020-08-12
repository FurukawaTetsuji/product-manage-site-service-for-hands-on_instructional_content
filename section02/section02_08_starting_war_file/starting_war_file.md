# WAR ファイルの起動

## WARファイル格納場所の作成
C:\udemy\angular の下に「api」フォルダを作成 ⇒ C:\udemy\angular\api

## 商品イメージ格納場所の作成
Cドライブ直下に「tmp」フォルダを作成 ⇒ C:\tmp

## APIのWARファイルをおいたレポジトリ（ここからWARファイルなど取得）
https://github.com/FurukawaTetsuji/product-manage-site-service-for-hands-on_war
ZIPでダウンロードもしくはgitクローンする

## 「webapp」フォルダについて
商品の写真を格納するフォルダ。
Cドライブ直下に「tmp」フォルダを作成しその中に「webapp」フォルダごと移動する ⇒ C:\tmp\webapp\product-images

## WARファイルなど
WARファイル：ProductManageService-1.0.0.war
設定ファイル：application.properties
起動用バッチファイル（Windowsのみ）：StartService.bat

上記3ファイルを下記に移動する
C:\udemy\angular\api

## WARファイルの起動方法
「StartService.bat」をダブルクリックする
または下記のコマンドを直接実行する
java -Xmx1024m -jar ProductManageService-1.0.0.war

## WARファイルの終了方法
WARファイルが動いているコマンド画面にて「Ctrl」＋「c」キーを押す

-------------------------------------------------------------------------------
※Macユーザのかた
-------------------------------------------------------------------------------
## WARファイル格納場所の作成
 ⇒ WARファイル格納場所はどこにしても構いません

## 商品イメージ格納場所の作成
 ⇒ /usr/local/tmp/を作成してください。
 ちがう場所にしたい場合は、合わせてapplication.propertiesの設定も変更してください。
