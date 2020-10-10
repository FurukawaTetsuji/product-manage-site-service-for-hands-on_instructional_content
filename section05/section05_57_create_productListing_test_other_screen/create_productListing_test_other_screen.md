# ① テストコードの作成

## ① - 1 ヘッダコンポーネント

### a. 「header.component.spec.ts」 を編集する

let matDialogSpy の下に下記を追加

```
let searchParamsServiceSpy: { removeProductListingSearchParamsDto: jasmine.Spy };
```

beforeEach(async(() => { の matDialogSpy = jasmine.createSpyObj('MatDialog', ['open']); 下に下記を追加

```
searchParamsServiceSpy = jasmine.createSpyObj('SearchParamsService', ['removeProductListingSearchParamsDto']);
```

TestBed.configureTestingModule({　の providers: [ 下に下記を追加

```
{ provide: SearchParamsService, useValue: searchParamsServiceSpy }
```

既存の declarations に FormattedCurrencyPipe, FormattedNumberPipe を追加

```
declarations: [ProductListingPageComponent, FormattedCurrencyPipe, FormattedNumberPipe]

```

beforeEach(async(() => {}); は、まとめると下記になる

```
beforeEach(async(() => {
  accountServiceSpy = jasmine.createSpyObj('AccountService', ['getMenu', 'signOut']);
  matDialogSpy = jasmine.createSpyObj('MatDialog', ['open']);
  searchParamsServiceSpy = jasmine.createSpyObj('SearchParamsService', ['removeProductListingSearchParamsDto']);

  TestBed.configureTestingModule({
    schemas: [NO_ERRORS_SCHEMA],
    imports: [
      RouterTestingModule,
      TranslateTestingModule.withTranslations({ ja: require('src/assets/i18n/ja.json') })
    ],
    providers: [
      { provide: MatDialog, useValue: matDialogSpy },
      { provide: AccountService, useValue: accountServiceSpy },
      { provide: SearchParamsService, useValue: searchParamsServiceSpy }
    ],
    declarations: [HeaderComponent]
  }).compileComponents();
  router = TestBed.inject(Router);
}));
```

テストメソッドを新たに clickSidenav の下に追加する

```
describe('#clickSubmenu', () => {
  it('should remove search param', () => {
    component.clickSubmenu();
    expect(searchParamsServiceSpy.removeProductListingSearchParamsDto.calls.count()).toBe(1);
  });
});
```

## ① - 2 サイドナビコンポーネント

### a. 「sidenav.component.spec.ts」 を編集する

let accountServiceSpy の下に下記を追加

```
let searchParamsServiceSpy: { removeProductListingSearchParamsDto: jasmine.Spy };
```

beforeEach(async(() => { の accountServiceSpy = jasmine.createSpyObj('AccountService', ['getMenu']); 下に下記を追加

```
searchParamsServiceSpy = jasmine.createSpyObj('SearchParamsService', ['removeProductListingSearchParamsDto']);
```

TestBed.configureTestingModule({　の providers: [ 下に下記を追加

```
{ provide: SearchParamsService, useValue: searchParamsServiceSpy }
```

既存の declarations に FormattedCurrencyPipe, FormattedNumberPipe を追加

```
declarations: [ProductListingPageComponent, FormattedCurrencyPipe, FormattedNumberPipe]

```

beforeEach(async(() => {}); は、まとめると下記になる

```
beforeEach(async(() => {
  accountServiceSpy = jasmine.createSpyObj('AccountService', ['getMenu']);
  searchParamsServiceSpy = jasmine.createSpyObj('SearchParamsService', ['removeProductListingSearchParamsDto']);

  TestBed.configureTestingModule({
    schemas: [NO_ERRORS_SCHEMA],
    imports: [
      RouterTestingModule,
      TranslateTestingModule.withTranslations({ ja: require('src/assets/i18n/ja.json') })
    ],
    providers: [
      { provide: AccountService, useValue: accountServiceSpy },
      { provide: SearchParamsService, useValue: searchParamsServiceSpy }
    ],
    declarations: [SidenavComponent]
  }).compileComponents();
  router = TestBed.inject(Router);
}));
```

clickSubmenu のテストに expect(searchParamsServiceSpy…を追加する

```
describe('#clickSubmenu', () => {
  it('should remove search param', () => {
    spyOn(component.sidenavClose, 'emit').and.callThrough();
    component.clickSubmenu();
    expect(searchParamsServiceSpy.removeProductListingSearchParamsDto.calls.count()).toBe(1);
    expect(component.sidenavClose.emit).toHaveBeenCalled();
  });
});
```
