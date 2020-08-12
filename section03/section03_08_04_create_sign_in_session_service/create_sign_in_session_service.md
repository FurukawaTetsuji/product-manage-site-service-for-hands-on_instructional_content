# ① サインインで取得したユーザ情報をセッションストレージに保存する

アカウントサービスにメソッドを追加して、サインイン API のレスポンス情報（ユーザ情報）を  
セッションストレージに保存する（ or 取得する）

## ① - 1 セッションストレージサービスを追加する

コマンド

```
ng g service core/services/session-storage
```

### a. コードを編集

上記で追加した「session-storage.service.ts」に下記の public で static なメソッドを追加する
（Dependency Injection なしで使える）

```
/**
  * Sets item
  * @template T type T as generics
  * @param key key name of variables to save session storage
  * @param t type T as generics
  */
static setItem<T>(key: string, t: T): void {
  sessionStorage.setItem(key, JSON.stringify(t));
}

/**
  * Gets item
  * @template T type T as generics
  * @param key key name of variables to save session storage
  * @param t type T as generics
  * @returns variables saved in session storage
  */
static getItem<T>(key: string, t: T): T {
  return JSON.parse(sessionStorage.getItem(key)) as T;
}

/**
  * Removes item
  * @param key key name of variables to save session storage
  */
static removeItem(key: string) {
  sessionStorage.removeItem(key);
}
```

## ① - 2 ユーザエンティティを作成

コマンド

```
ng g class pages/models/user
```

### a. コードの編集

```
export class User {
  userAccount: string;
  userName: string;
  userLocale: string;
  userLanguage: string;
  userTimezone: string;
  userTimezoneOffset: string;
  userCurrency: string;
}
```

## ① - 3 アカウントサービスにメソッド追加

#getUser()、#setUser()、#removeUser()の 3 メソッド
（セッションストレージからユーザ情報を取得する / 保存する / 削除する）

### a. メソッドを追加

「account.service.ts」に下記の public メソッドを追加する。  
（コンストラクタに SessionStorageService は追加しなくてよい）

```
/**
  * Gets user
  * @returns user informations from session storage
  */
getUser(): User {
  return SessionStorageService.getItem(AppConst.STORAGE_KEY_USER, new User());
}

/**
  * Sets user
  * @param user infomatios to save session storage
  */
setUser(user: User): void {
  SessionStorageService.setItem(AppConst.STORAGE_KEY_USER, user);
}

/**
  * Removes user
  */
removeUser(): void {
  SessionStorageService.removeItem(AppConst.STORAGE_KEY_USER);
}
```

## ① - 4 サインインページでユーザ情報を保存する

### a. サインインページコンポーネントの ts を編集

「sign-in-page.component.ts」に#setUpUserAccount()メソッドを追加する  
サインインのレスポンスで取得した SignInResponseDto をもとにして  
セッションストレージにユーザ情報を保存するメソッド

```
private setUpUserAccount(responseDto: SignInResponseDto) {
  const user: User = new User();
  user.userAccount = responseDto.userAccount;
  user.userName = responseDto.userName;
  user.userLocale = responseDto.userLocale;
  user.userLanguage = responseDto.userLanguage;
  user.userTimezone = responseDto.userTimezone;
  user.userTimezoneOffset = responseDto.userTimezoneOffset;
  user.userCurrency = responseDto.userCurrency;
  this.accountService.setUser(user);

  console.log('SignInPageComponent #setUpUserAccount() user.userAccount:' + user.userAccount);
  console.log('SignInPageComponent #setUpUserAccount() user.userName:' + user.userName);
  console.log('SignInPageComponent #setUpUserAccount() user.userLocale:' + user.userLocale);
  console.log('SignInPageComponent #setUpUserAccount() user.userLanguage:' + user.userLanguage);
  console.log('SignInPageComponent #setUpUserAccount() user.userTimezone:' + user.userTimezone);
  console.log('SignInPageComponent #setUpUserAccount() user.userTimezoneOffset:' + user.userTimezoneOffset);
  console.log('SignInPageComponent #setUpUserAccount() user.userCurrency:' + user.userCurrency);
}
```

#signIn()メソッドの中で追加した上記メソッドを呼び出す

```
private signIn(signInRequestDto: SignInRequestDto) {
  // Signs in and gets response dto.
  const signInResponseDto: Observable<SignInResponseDto> = this.accountService.signIn(signInRequestDto);
  signInResponseDto.subscribe(responseDto => {
    if (responseDto != null) {
      // Sets account information.
      this.setUpUserAccount(responseDto);
      // Moves to the Product listing page.
      this.routingService.navigate(UrlConst.PATH_PRODUCT_LISTING);
    }
  });
}
```

## ① - 3 起動して確認

コマンド

```
ng serve -o --proxy-config proxy.conf.json
```
