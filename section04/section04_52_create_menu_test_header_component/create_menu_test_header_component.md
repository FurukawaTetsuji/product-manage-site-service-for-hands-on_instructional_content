# ① テストコードの作成

## ① - 1 ヘッダーコンポーネント

### a. 「header.component.spec.ts」 を編集する

xdescribe( を describe( に戻す

beforeEach(());の中の fixture.detectChanges();をコメントアウト

```
beforeEach(() => {
  fixture = TestBed.createComponent(HeaderComponent);
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
let accountServiceSpy: { getMenu: jasmine.Spy; signOut: jasmine.Spy };
let matDialogSpy: { open: jasmine.Spy };
let router: Router;
```

beforeEach(async(() => { の下に spy を追加

```
accountServiceSpy = jasmine.createSpyObj('AccountService', ['getMenu', 'signOut']);
matDialogSpy = jasmine.createSpyObj('MatDialog', ['open']);
```

TestBed.configureTestingModule({ の下に追加

```
schemas: [NO_ERRORS_SCHEMA],
imports: [
  RouterTestingModule,
  TranslateTestingModule.withTranslations({ ja: require('src/assets/i18n/ja.json') })
],
providers: [
  { provide: MatDialog, useValue: matDialogSpy },
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
  accountServiceSpy = jasmine.createSpyObj('AccountService', ['getMenu', 'signOut']);
  matDialogSpy = jasmine.createSpyObj('MatDialog', ['open']);

  TestBed.configureTestingModule({
    schemas: [NO_ERRORS_SCHEMA],
    imports: [
      RouterTestingModule,
      TranslateTestingModule.withTranslations({ ja: require('src/assets/i18n/ja.json') })
    ],
    providers: [
      { provide: MatDialog, useValue: matDialogSpy },
      { provide: AccountService, useValue: accountServiceSpy }
    ],
    declarations: [HeaderComponent]
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

describe('#clickSidenav', () => {
  it('should show side navi', () => {
    spyOn(component.sidenavToggle, 'emit').and.callThrough();
    component.clickSidenav();
    expect(component.sidenavToggle.emit).toHaveBeenCalled();
  });
});

describe('#clickSignOut', () => {
  it('should sign out', async(() => {
    matDialogSpy.open.and.returnValue({ afterClosed: () => of(true) });
    accountServiceSpy.signOut.and.returnValue(of(null));
    spyOn(router, 'navigate');
    component.clickSignOut();
    expect(matDialogSpy.open.calls.count()).toBe(1);
    expect(accountServiceSpy.signOut.calls.count()).toBe(1);
    expect(router.navigate).toHaveBeenCalledWith([UrlConst.SLASH + UrlConst.PATH_SIGN_IN]);
  }));

  it('should not sign out', () => {
    matDialogSpy.open.and.returnValue({ afterClosed: () => of(false) });
    component.clickSignOut();
    expect(matDialogSpy.open.calls.count()).toBe(1);
    expect(accountServiceSpy.signOut.calls.count()).toBe(0);
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
