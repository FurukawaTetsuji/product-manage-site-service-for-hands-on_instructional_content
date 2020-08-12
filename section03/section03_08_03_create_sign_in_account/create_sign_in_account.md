# ① API との通信

## ① - 1 API を確認

### a. WAR を起動する

コマンド

```
java -Xmx1024m -jar ProductManageService-1.0.0.war
```

### b. API ドキュメントで URI を確認する

API ドキュメント  
http://localhost:8080/swagger-ui.html

コントローラ名  
sign-in-rest-controller

URI  
localhost:8080/api/sign-in/v1

### c. API POSTMAN で確認する

localhost:8080/api/sign-in/v1
「Authorization」タブを開いて、Basic 認証の「Username」と「Password」を使用する

① ユーザ１  
Request:  
Username:user01  
Password:demo

Response:

```
{
    "userAccount": "user01",
    "userName": "テストユーザ０１",
    "userLocale": "ja-JP",
    "userLanguage": "ja",
    "userTimezone": "Asia/Tokyo",
    "userTimezoneOffset": "+0900",
    "userCurrency": "JPY"
}
```

② ユーザ２  
Request:  
Username:user01  
Password:demo

Response:

```
{
    "userAccount": "user02",
    "userName": "TEST_USER02",
    "userLocale": "en-US",
    "userLanguage": "en",
    "userTimezone": "America/Los_Angeles",
    "userTimezoneOffset": "-0800",
    "userCurrency": "USD"
}
```

③ ユーザ９９  
Request:  
Username:user99  
Password:demo

Response:

```
{
    "userAccount": "user99",
    "userName": "管理者ユーザ",
    "userLocale": "ja-JP",
    "userLanguage": "ja",
    "userTimezone": "Asia/Tokyo",
    "userTimezoneOffset": "+0900",
    "userCurrency": "JPY"
}
```

# 参考資料

## Spring と Angular の認証について

Spring のチュートリアル  
https://spring.io/guides/tutorials/spring-security-and-angular-js/#using-curl

日本語版  
https://spring.pleiades.io/guides/tutorials/spring-security-and-angular-js/
