# ① API との通信

## ① - 2 Dto を追加する

API のリクエスト用とレスポンス用に 2 つ作成する

### a. SignInRequestDto を追加

コマンド

```
ng g interface pages/models/dtos/requests/sign-in-request-dto
```

内容

```
export interface SignInRequestDto {
  Username: string;
  Password: string;
}
```

### b. signInResponseDto を編集

コマンド

```
ng g interface pages/models/dtos/responses/sign-in-response-dto
```

内容

```
export interface SignInResponseDto {
  userAccount: string;
  userName: string;
  userLocale: string;
  userLanguage: string;
  userTimezone: string;
  userTimezoneOffset: string;
  userCurrency: string;
}
```

## ① - 3 Account サービスを編集

コンストラクタに HttpClient を追加する

```
constructor(private http: HttpClient) {}
```

```
/**
  * Signs in
  * @param signInRequestDto sign in request
  * @returns sign in response
  */
signIn(signInRequestDto: SignInRequestDto): Observable<SignInResponseDto> {
  const webApiUrl = ApiConst.PATH_API_ROOT + ApiConst.PATH_SIGN_IN;
  const headers = new HttpHeaders({
    authorization: 'Basic ' + btoa(signInRequestDto.Username + ':' + signInRequestDto.Password)
  });

  return this.http
    .post<SignInResponseDto>(webApiUrl, signInRequestDto, { headers })
    .pipe(
      catchError((error) => {
        return of(null as SignInResponseDto);
      })
    );
}
```

catchError の import 文はこちら

```
import { catchError } from 'rxjs/operators';
```

## ① - 4 Account サービスを呼び出し

### a. sign-in-page.component.ts の編集

「sign-in-page.component.ts」を編集する

コンストラクタに下記を追加する

```
private accountService: AccountService,
private routingService: RoutingService,
```

サインインボタンをクリックした時に呼び出されるメソッドを追加する

```
/**
  * Clicks sign in button
  */
clickSignInButton() {
  // Creates request dto.
  const signInRequestDto = this.createSignInRequestDto();

  // Signs in using dto.
  this.signIn(signInRequestDto);
}
```

private メソッド 2 つを追加する

1. サインインが成功したら商品一覧画面に移動するメソッド
1. SignInRequestDto のエンティティを作るメソッド

```
private signIn(signInRequestDto: SignInRequestDto) {
  // Signs in and gets response dto.
  const signInResponseDto: Observable<SignInResponseDto> = this.accountService.signIn(signInRequestDto);
  signInResponseDto.subscribe(responseDto => {
    if (responseDto != null) {
      // Moves to the Product listing page.
      this.routingService.navigate(UrlConst.PATH_PRODUCT_LISTING);
    }
  });
}

private createSignInRequestDto(): SignInRequestDto {
  // Creates Request.
  return {
    Username: this.signInUserAccount.value,
    Password: this.signInUserPassword.value
  };
}
```

### b. sign-in-page.component.html の編集

button タグに下記のクリックイベントを追加する

```
(click)="clickSignInButton()"
```

### c. ポート転送

Angular のフォルダー直下に「proxy.conf.json」ファイル を vscode で作成

```
{
  "/api": {
    "target": "http://localhost:8080"
  }
}
```

## ① - 5 動作確認

コマンド

```
ng serve -o --proxy-config proxy.conf.json
```

「package.json」を編集

```
"scripts": {
    "start": "ng serve -o --proxy-config proxy.conf.json"
}
```
