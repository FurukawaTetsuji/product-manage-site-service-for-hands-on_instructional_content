# ③ shared module の作業

## ③ - 1 モジュール作成

### ● コマンド

ng g module shared --module app.module

## ③ - 2 component 作成

下記の 3 コンポーネントを作ります

1. sidenav
1. header
1. footer

### ● コマンド

ng g component shared/components/sidenav

ng g component shared/components/header

ng g component shared/components/footer

## ③ - 3 servie 作成

sidenav、header のサービスは pages module に作るのでここでは作りません。  
フッター用のサービスそもそも必要ありません。  
ブラウザのタイトル用のサービスのみ追加します。

1. title-i18.service

### ● コマンド

ng g service shared/services/title-i18
