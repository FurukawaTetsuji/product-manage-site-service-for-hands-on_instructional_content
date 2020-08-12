# ① テストコードの作成

## ① - 1 app のコンポーネント

### a. 「app.component.spec.ts」 を編集する

xdescribe( を describe( に戻す

もともと書かれていたソースは下記で書き換える
it('should render title', () => {});は削除する

```
describe('#constractor', () => {
  it('should create the app', () => {
    const app = fixture.debugElement.componentInstance;
    expect(app).toBeTruthy();
  });

  it('should have as title _ product-manage-site-for-hands-on', () => {
    const app = fixture.debugElement.componentInstance;
    expect(app.title).toEqual('product-manage-site-for-hands-on');
  });
});
```

describe の下に変数を追加

```
let component: AppComponent;
let fixture: ComponentFixture<AppComponent>;
let router: Router;
```

.compileComponents(); の下に下記を追加

```
router = TestBed.inject(Router);
```

beforeEach(async(() => {});をぬけた下に下記を追加

```
beforeEach(() => {
  fixture = TestBed.createComponent(AppComponent);
  component = fixture.componentInstance;
  fixture.detectChanges();
});
```

テストメソッドを新たに追加する

```
describe('#isSignInPage', () => {
  it('should be true _ slash', () => {
    spyOnProperty(router, 'url').and.returnValue(UrlConst.SLASH);
    expect(component.isSignInPage()).toBeTruthy();
  });

  it('should be true _ sign in', () => {
    spyOnProperty(router, 'url').and.returnValue(UrlConst.SLASH + UrlConst.PATH_SIGN_IN);
    expect(component.isSignInPage()).toBeTruthy();
  });

  it('should be false', () => {
    spyOnProperty(router, 'url').and.returnValue(UrlConst.SLASH + UrlConst.PATH_PRODUCT_LISTING);
    expect(component.isSignInPage()).toBeFalsy();
  });
});
```
