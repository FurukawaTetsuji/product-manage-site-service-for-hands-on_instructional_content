# ① テストコードの作成

## ① - 1 その他のコンポーネント

### c. 「auth.guard.spec.ts」 を編集する

下記の guard を authGuard にリネーム

```
let guard: AuthGuard;
```

もともと書かれていたソースを describe でかこむ

```
describe('#constractor', () => {
  it('should create', () => {
    expect(authGuard).toBeTruthy();
  });
});
```

describe のすぐ下に変数を追加

```
let authGuard: AuthGuard;
let accountServiceSpy: { getAvailablePages: jasmine.Spy };
let routerStateSnapshotSpy: RouterStateSnapshot;
let router: Router;
```

beforeEach(() => { の下に spy を追加

```
accountServiceSpy = jasmine.createSpyObj('AccountService', ['getAvailablePages']);
routerStateSnapshotSpy = jasmine.createSpyObj('RouterStateSnapshot', ['toString']);
```

TestBed.configureTestingModule({ の下に追加

```
imports: [RouterTestingModule],
providers: [{ provide: AccountService, useValue: accountServiceSpy }]
```

TestBed.configureTestingModule({}); の下に追加

```
authGuard = TestBed.inject(AuthGuard);
router = TestBed.inject(Router);
```

beforeEach(async(() => {})); は、まとめると下記になる

```
beforeEach(() => {
  accountServiceSpy = jasmine.createSpyObj('AccountService', ['getAvailablePages']);
  routerStateSnapshotSpy = jasmine.createSpyObj('RouterStateSnapshot', ['toString']);

  TestBed.configureTestingModule({
    imports: [RouterTestingModule],
    providers: [{ provide: AccountService, useValue: accountServiceSpy }]
  });
  authGuard = TestBed.inject(AuthGuard);
  router = TestBed.inject(Router);
});
```

テストメソッドを新たに追加する

```
describe('#canActivate', () => {
  describe('no page is available to the user', () => {
    it('should return false if no page is available to the user', () => {
      accountServiceSpy.getAvailablePages.and.returnValue(of(null));
      spyOn(router, 'navigate');
      (authGuard.canActivate(null, null) as Observable<boolean>).subscribe((res) => {
        expect(router.navigate).toHaveBeenCalledWith([UrlConst.SLASH + UrlConst.PATH_SIGN_IN]);
        expect(res).toBeFalsy();
      });
    });
  });

  describe('else', () => {
    it('should return true if the current url is on a page available to the user', () => {
      routerStateSnapshotSpy.url = UrlConst.SLASH + UrlConst.PATH_PRODUCT_LISTING;
      accountServiceSpy.getAvailablePages.and.returnValue(of(Array(UrlConst.PATH_PRODUCT_LISTING)));
      (authGuard.canActivate(null, routerStateSnapshotSpy) as Observable<boolean>).subscribe((res) => {
        expect(res).toBeTruthy();
      });
    });
    it('should return true if the current url + /:productCode is on a page available to the user', () => {
      routerStateSnapshotSpy.url =
        UrlConst.SLASH + UrlConst.PATH_PRODUCT_REGISTERING + UrlConst.SLASH + ':productCode';
      accountServiceSpy.getAvailablePages.and.returnValue(of(Array(UrlConst.PATH_PRODUCT_REGISTERING)));
      (authGuard.canActivate(null, routerStateSnapshotSpy) as Observable<boolean>).subscribe((res) => {
        expect(res).toBeTruthy();
      });
    });
    it('should return false if the current url is not on a page available to the user', () => {
      routerStateSnapshotSpy.url = UrlConst.SLASH + UrlConst.PATH_PRODUCT_LISTING;
      accountServiceSpy.getAvailablePages.and.returnValue(of(Array(UrlConst.PATH_DUMMY_PURCHASING)));
      spyOn(router, 'navigate');
      (authGuard.canActivate(null, routerStateSnapshotSpy) as Observable<boolean>).subscribe((res) => {
        expect(router.navigate).toHaveBeenCalledWith([UrlConst.SLASH + UrlConst.PATH_SIGN_IN]);
        expect(res).toBeFalsy();
      });
    });
  });
});
```
