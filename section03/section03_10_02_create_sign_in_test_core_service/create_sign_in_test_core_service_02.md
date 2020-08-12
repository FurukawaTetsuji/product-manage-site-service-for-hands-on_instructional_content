# ① テストコードの作成

## ① - 1 Core のサービス

### a. 「routing.service.spec.ts」 を編集する

xdescribe( を describe( に戻す

Router クラスの変数を beforeEach の上に追加
RouterTestingModule をインポートする
beforeEach の中で router を inject する

```
let service: RoutingService;
let router: Router;

beforeEach(() => {
  TestBed.configureTestingModule({
    imports: [RouterTestingModule]
  });
  service = TestBed.inject(RoutingService);
  router = TestBed.inject(Router);
});
```

もともと書かれていたソースを describe でかこむ

```
describe('#constractor', () => {
  it('should be created', () => {
    expect(service).toBeTruthy();
  });
});
```

テストメソッドを追加する

```
describe('#navigate', () => {
  it('should be navigated to sign in page', () => {
    spyOn(router, 'navigate');
    service.navigate(UrlConst.PATH_SIGN_IN);
    expect(router.navigate).toHaveBeenCalledWith([UrlConst.SLASH + UrlConst.PATH_SIGN_IN]);
  });
});
```

### b. 「session-storage.service.spec.ts」 を編集する

xdescribe( を describe( に戻す

User クラスの変数を beforeEach の上に追加
beforeEach の中で expectedUser = createUser();

```
let service: SessionStorageService;
let expectedUser: User;

beforeEach(() => {
  TestBed.configureTestingModule({});
  service = TestBed.inject(SessionStorageService);
  expectedUser = createUser();
});
```

もともと書かれていたソースを describe でかこむ

```
describe('#constractor', () => {
  it('should be created', () => {
    expect(service).toBeTruthy();
  });
});
```

テストメソッドを追加する

```
describe('#setItem, #getItem', () => {
  it('should set and get item', () => {
    SessionStorageService.setItem(AppConst.STORAGE_KEY_USER, expectedUser);
    const resultUser: User = SessionStorageService.getItem(AppConst.STORAGE_KEY_USER, new User());
    expect(resultUser.userAccount).toEqual(expectedUser.userAccount);
    expect(resultUser.userName).toEqual(expectedUser.userName);
    expect(resultUser.userLocale).toEqual(expectedUser.userLocale);
    expect(resultUser.userLanguage).toEqual(expectedUser.userLanguage);
    expect(resultUser.userTimezone).toEqual(expectedUser.userTimezone);
    expect(resultUser.userTimezoneOffset).toEqual(expectedUser.userTimezoneOffset);
    expect(resultUser.userCurrency).toEqual(expectedUser.userCurrency);
  });
});

describe('#removeItem', () => {
  it('should remove item', () => {
    SessionStorageService.setItem(AppConst.STORAGE_KEY_USER, expectedUser);
    SessionStorageService.removeItem(AppConst.STORAGE_KEY_USER);
    expect(SessionStorageService.getItem(AppConst.STORAGE_KEY_USER, new User())).toBeNull();
  });
});
```

テストコードの外に
下のメソッドを追加する

```
function createUser(): User {
  const user: User = new User();
  user.userAccount = 'userAccount';
  user.userName = 'userName';
  user.userLocale = 'ja-JP';
  user.userLanguage = 'ja';
  user.userTimezone = 'Asia/Tokyo';
  user.userTimezoneOffset = '+0900';
  user.userCurrency = 'JPY';
  return user;
}
```
