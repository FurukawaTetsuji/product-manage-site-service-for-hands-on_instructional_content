# 前回までのおさらい

これまでに下記の作業は終わっています。

- material module 作成済
- ngx-translata module 作成済
- assets（フォルダー）作成済み

# ① module 作成

今回作る module は 4 つ

1. core
1. shared
1. pages
1. dummy-purchasing-page

# ② core module の作業

## ② - 1 モジュール作成

### ● コマンド

```
ng g module core --module app.module
```

## ② - 2 component 作成

まず core module では下の 2 つのコンポーネントを作ります

1. loading
1. error-messaging

### ● コマンド

```
ng g component core/components/loading

ng g component core/components/error-messaging
```

## ② - 3 servie 作成

1. loading.service
2. error-messaging.service

### ● コマンド

```
ng g service core/services/loading

ng g service core/services/error-messaging
```
