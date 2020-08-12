# ① メニュー関連 API の追加

## ① - 1 menu api

menu api のレスポンスを受け取るインターフェースを作る

```
ng g interface pages/models/dtos/responses/menu-list-response-dto
```

中にメンバー変数を 2 つ追加する。

```
export interface MenuListResponseDto {
  menuCode: string;
  subMenuCodeList: string[];
}
```

account service 「account.service.ts」に下記メニュー関連のメソッドを追加

```
/**
* Gets menu
* @returns menu response
*/
getMenu(): Observable<MenuListResponseDto[]> {
const webApiUrl = ApiConst.PATH_API_ROOT + ApiConst.PATH_MENU;

return this.http.get<MenuListResponseDto[]>(webApiUrl).pipe(
    catchError((error) => {
    this.errorMessageService.setupPageErrorMessageFromResponse(error);
    return of(null as MenuListResponseDto[]);
    })
);
}

/**
* Gets available pages
* @returns available pages response
*/
getAvailablePages(): Observable<string[]> {
const webApiUrl = ApiConst.PATH_API_ROOT + ApiConst.PATH_AVAILABLE_PAGES;

return this.http.get<string[]>(webApiUrl).pipe(
    catchError((error) => {
    this.errorMessageService.setupPageErrorMessageFromResponse(error);
    return of(null as string[]);
    })
);
}

/**
* Signs out
* @returns nothing
*/
signOut(): Observable<void> {
const webApiUrl = ApiConst.PATH_API_ROOT + ApiConst.PATH_SIGN_OUT;
return this.http.post(webApiUrl, {}).pipe(
    map((res) => {
    this.removeUser();
    return;
    })
);
}
```
