# ① メニュー関連 API の説明

## ① - 1 API の内容を確認する

API ドキュメント  
http://localhost:8080/swagger-ui.html

コントローラ名  
menu-rest-controller

### a. menu api を確認する

localhost:8080/api/menu/v1

（「GET」メソッド）

※初めにサインインしておく必要あり  
localhost:8080/api/sign-in/v1  
（「POST」メソッド）  
「Authorization」タブを開いて、Basic 認証の「Username」と「Password」を使用する

レスポンス

```
[
    {
        "menuCode": "product",
        "subMenuCodeList": [
            "product-listing"
        ]
    },
    {
        "menuCode": "purchase",
        "subMenuCodeList": [
            "purchase-history-listing"
        ]
    },
    {
        "menuCode": "stock",
        "subMenuCodeList": [
            "stock-registering"
        ]
    }
]
```

### b. available-pages api を確認する

localhost:8080/api/available-pages/v1

（「GET」メソッド）

レスポンス

```
[
    "purchase-history-listing",
    "product-listing",
    "product-registering",
    "stock-registering"
]
```

### c. sign-out api を確認する

localhost:8080/api/sign-out/v1

（「GET」メソッド）
