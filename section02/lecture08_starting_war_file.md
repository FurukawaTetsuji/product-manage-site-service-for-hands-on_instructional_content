# WAR ファイルの起動

## WAR ファイル格納場所の作成

C:\udemy\angular の下に「api」フォルダを作成 ⇒ C:\udemy\angular\api

## 商品イメージ格納場所の作成

C ドライブ直下に「tmp」フォルダを作成 ⇒ C:\tmp

## API の WAR ファイルをおいたレポジトリ（ここから WAR ファイルなど取得）

https://github.com/FurukawaTetsuji/product-manage-site-service-for-hands-on_war

ZIP でダウンロードもしくは git クローンする

## 「webapp」フォルダについて

商品の写真を格納するフォルダ。  
C ドライブ直下に「tmp」フォルダを作成しその中に「webapp」フォルダごと移動する ⇒ C:\tmp\webapp\product-images

## WAR ファイルなど

WAR ファイル：ProductManageService-1.0.0.war  
設定ファイル：application.properties  
起動用バッチファイル（Windows のみ）：StartService.bat

上記 3 ファイルを下記に移動する
C:\udemy\angular\api

## WAR ファイルの起動方法

「StartService.bat」をダブルクリックする  
または下記のコマンドを直接実行する  
java -Xmx1024m -jar ProductManageService-1.0.0.war

## WAR ファイルの終了方法

WAR ファイルが動いているコマンド画面にて「Ctrl」＋「c」キーを押す

---

## ※Mac ユーザのかた

## WAR ファイル格納場所の作成

⇒ WAR ファイル格納場所はどこにしても構いません

## 商品イメージ格納場所の作成

⇒ /usr/local/tmp/を作成してください。  
 ちがう場所にしたい場合は、合わせて application.properties の設定も変更してください。
