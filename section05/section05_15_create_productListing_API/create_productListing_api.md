# ① API との通信

## ① - 1 API を確認

商品ジャンルの API  
商品一覧検索の API

### a. WAR を起動する

コマンド

```
java -Xmx1024m -jar ProductManageService-1.0.0.war
```

### b. サインイン

localhost:8080/api/sign-in/v1
「Authorization」タブを開いて、Basic 認証の「Username」と「Password」を使用する

① ユーザ９９  
Request:  
Username:user99  
Password:demo

### b. 商品ジャンルのテスト

localhost:8080/api/product-genre/v1

Response:

```
[
    "1",
    "2",
    "3"
]
```

### b. 商品一覧検索のテスト

localhost:8080/api/product-search/v1

Request  
Params タブの「Query Params」に下記をセットする。

```
{
    "productName": "",
    "productCode": "",
    "productGenre": "",
    "endOfSale": false,
    "pageSize": 50,
    "pageIndex": 0
}
```

Response:

```
{
    "pageIndex": 0,
    "resultsLength": 5,
    "productSearchResponseDtos": [
        {
            "no": 1,
            "productName": "花柄のスニーカー",
            "productCode": "ABCD123",
            "productGenre": "1",
            "productImageUrl": "http://localhost:8080/product-images/ABCD123.png",
            "productSizeStandard": "24,25,26,27cm",
            "productColor": "花柄",
            "productUnitPrice": 2500.000,
            "productStockQuantity": 500,
            "endOfSale": false
        },
        {
            "no": 2,
            "productName": "男性革靴",
            "productCode": "ABCD456",
            "productGenre": "1",
            "productImageUrl": "http://localhost:8080/product-images/ABCD456.png",
            "productSizeStandard": "25,26,27,28cm",
            "productColor": "茶",
            "productUnitPrice": 12800.000,
            "productStockQuantity": 100,
            "endOfSale": false
        },
        {
            "no": 3,
            "productName": "カバン",
            "productCode": "XYZ0123",
            "productGenre": "3",
            "productImageUrl": "http://localhost:8080/product-images/XYZ0123.png",
            "productSizeStandard": "フリー",
            "productColor": "茶",
            "productUnitPrice": 8800.000,
            "productStockQuantity": 200,
            "endOfSale": false
        },
        {
            "no": 4,
            "productName": "セーター",
            "productCode": "ABCD654",
            "productGenre": "2",
            "productImageUrl": "http://localhost:8080/product-images/ABCD654.png",
            "productSizeStandard": "S/M/L",
            "productColor": "グレー",
            "productUnitPrice": 7800.000,
            "productStockQuantity": 300,
            "endOfSale": false
        },
        {
            "no": 5,
            "productName": "ビジネスバッグ",
            "productCode": "XYZ3210",
            "productGenre": "3",
            "productImageUrl": "http://localhost:8080/product-images/XYZ3210.png",
            "productSizeStandard": "30×70cm",
            "productColor": "黒/茶",
            "productUnitPrice": 24000.000,
            "productStockQuantity": 400,
            "endOfSale": false
        }
    ]
}
```
