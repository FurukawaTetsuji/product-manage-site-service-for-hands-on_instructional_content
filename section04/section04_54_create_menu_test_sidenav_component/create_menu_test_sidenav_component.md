# ① テストコードの作成

## ① - 1 サイドナビコンポーネント

### a. 「sidenav.component.spec.ts」 を編集する

xdescribe( を describe( に戻す

beforeEach(());の中の fixture.detectChanges();をコメントアウト

```
beforeEach(() => {
  fixture = TestBed.createComponent(SidenavComponent);
  component = fixture.componentInstance;
  // Commented out the following because an error occurred in the 'should create' test.
  // fixture.detectChanges();
});
```

もともと書かれていたソースを describe でかこむ

```
describe('#constractor', () => {
  it('should create', () => {
    expect(component).toBeTruthy();
  });
});
```

let の下に変数を追加

```
let accountServiceSpy: { getMenu: jasmine.Spy };
let router: Router;
```

beforeEach(async(() => { の下に spy を追加

```
accountServiceSpy = jasmine.createSpyObj('AccountService', ['getMenu']);
```

TestBed.configureTestingModule({ の下に追加

```
schemas: [NO_ERRORS_SCHEMA],
imports: [
  RouterTestingModule,
  TranslateTestingModule.withTranslations({ ja: require('src/assets/i18n/ja.json') })
],
providers: [
  { provide: AccountService, useValue: accountServiceSpy }
],
```

compileComponents();の下に下記を追加

```
router = TestBed.inject(Router);
```

beforeEach(async(() => {})); は、まとめると下記になる

```
beforeEach(async(() => {
  accountServiceSpy = jasmine.createSpyObj('AccountService', ['getMenu']);

  TestBed.configureTestingModule({
    schemas: [NO_ERRORS_SCHEMA],
    imports: [
      RouterTestingModule,
      TranslateTestingModule.withTranslations({ ja: require('src/assets/i18n/ja.json') })
    ],
    providers: [
      { provide: AccountService, useValue: accountServiceSpy }
    ],
    declarations: [SidenavComponent]
  }).compileComponents();
  router = TestBed.inject(Router);
}));
```

テストメソッドを新たに追加する

```
describe('#ngOnInit', () => {
  it('should init', () => {
    const expetedMenuListResponseDtos: MenuListResponseDto[] = createExpectedMenu();
    accountServiceSpy.getMenu.and.returnValue(of(expetedMenuListResponseDtos));

    component.ngOnInit();
    expect(component.menuListResponseDto).toEqual(expetedMenuListResponseDtos);
    expect(accountServiceSpy.getMenu.calls.count()).toBe(1);
  });
});

describe('#clickSubmenu', () => {
  it('should remove search param', () => {
    spyOn(component.sidenavClose, 'emit').and.callThrough();
    component.clickSubmenu();
    expect(component.sidenavClose.emit).toHaveBeenCalled();
  });
});

describe('#clickHome', () => {
  it('should close side nav', () => {
    spyOn(component.sidenavClose, 'emit').and.callThrough();
    component.clickHome();
    expect(component.sidenavClose.emit).toHaveBeenCalled();
  });
});
```

クラスの外側に追加

```
function createExpectedMenu() {
  const menuListResponseDto1: MenuListResponseDto = {
    menuCode: 'product',
    subMenuCodeList: Array('product-listing')
  };

  const menuListResponseDto2: MenuListResponseDto = {
    menuCode: 'purchase',
    subMenuCodeList: Array('purchase-history-listing', 'dummy-purchasing')
  };

  const expetedMenuListResponseDtos: MenuListResponseDto[] = Array(menuListResponseDto1, menuListResponseDto2);
  return expetedMenuListResponseDtos;
}
```

### b. 「app.component.spec.ts」 を編集する

TestBed.configureTestingModule({});の中に下記を追加する
（sidenav などを追加した影響でユニットテストでエラーが出るようになった為）

```
schemas: [NO_ERRORS_SCHEMA],
imports: [RouterTestingModule, MaterialModule, BrowserAnimationsModule],
```
