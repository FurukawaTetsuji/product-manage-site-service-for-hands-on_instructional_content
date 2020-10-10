# ① テストコードの作成

## ① - 1 商品一覧ページコンポーネント

### a. 「product-listing-page.component.spec.ts」 を編集する

xdescribe( を describe( に戻す

もともと書かれていたソースを describe でかこむ

```
describe('#constractor', () => {
  it('should create', () => {
    expect(component).toBeTruthy();
  });
});
```

let fixture の下に下記を追加

```
let accountServiceSpy: { getUser: jasmine.Spy };
let productServiceSpy: { getGenres: jasmine.Spy; getProductList: jasmine.Spy };
let titleI18ServiceSpy: { setTitle: jasmine.Spy };
let searchParamsServiceSpy: {
  getProductListingSearchParamsDto: jasmine.Spy;
  removeProductListingSearchParamsDto: jasmine.Spy;
  setProductListingSearchParamsDto: jasmine.Spy;
};
let router: Router;

/** Initial values */
const VALUE_PRODUCT_CODE = 'productCode';
const VALUE_PRODUCT_NAME = 'productName';
const VALUE_PRODUCT_GENRE = '1';
const VALUE_END_OF_SALE_TRUE = true;
const INITIAL_VALUE_PAGE_INDEX = 0;
const INITIAL_VALUE_PAGE_SIZE = 50;

/** Expected values */
const expectedUser = createExpectedUser();
const expectedGenres = createExpectedGenres();

/** Expected values of search params */
const expectedProductListingSearchParamsDto = createExpectedProductListingSearchParamsDto();
const expectedSearchParamsDtoPartlyUndefined = createExpectedSearchParamsDtoPartlyUndefined();

/** Expected values of search results */
const expectedProductSearchListResponseDto = createExpectedProductSearchListResponseDto();
```

beforeEach(async(() => { の下に下記を追加

```
accountServiceSpy = jasmine.createSpyObj('AccountService', ['getUser']);
productServiceSpy = jasmine.createSpyObj('ProductService', ['getGenres', 'getProductList']);
titleI18ServiceSpy = jasmine.createSpyObj('TitleI18Service', ['setTitle']);
searchParamsServiceSpy = jasmine.createSpyObj('SearchParamsService', [
  'getProductListingSearchParamsDto',
  'removeProductListingSearchParamsDto',
  'setProductListingSearchParamsDto'
]);
```

TestBed.configureTestingModule({　の下に下記を追加

```
schemas: [NO_ERRORS_SCHEMA],
imports: [
  NgxUpperCaseDirectiveModule,
  HttpClientTestingModule,
  RouterTestingModule,
  TranslateTestingModule.withTranslations({ ja: require('src/assets/i18n/ja.json') }),
  MaterialModule,
  BrowserAnimationsModule,
  ReactiveFormsModule
],
providers: [
  FormBuilder,
  FormattedCurrencyPipe,
  FormattedNumberPipe,
  CurrencyPipe,
  { provide: AccountService, useValue: accountServiceSpy },
  { provide: ProductService, useValue: productServiceSpy },
  { provide: TitleI18Service, useValue: titleI18ServiceSpy },
  { provide: SearchParamsService, useValue: searchParamsServiceSpy }
],
```

既存の declarations に FormattedCurrencyPipe, FormattedNumberPipe を追加

```
declarations: [ProductListingPageComponent, FormattedCurrencyPipe, FormattedNumberPipe]
```

}).compileComponents(); の下に下記を追加

```
router = TestBed.inject(Router);
```

beforeEach(async(() => {}); は、まとめると下記になる

```
beforeEach(async(() => {
  accountServiceSpy = jasmine.createSpyObj('AccountService', ['getUser']);
  productServiceSpy = jasmine.createSpyObj('ProductService', ['getGenres', 'getProductList']);
  titleI18ServiceSpy = jasmine.createSpyObj('TitleI18Service', ['setTitle']);
  searchParamsServiceSpy = jasmine.createSpyObj('SearchParamsService', [
    'getProductListingSearchParamsDto',
    'removeProductListingSearchParamsDto',
    'setProductListingSearchParamsDto'
  ]);

  TestBed.configureTestingModule({
    schemas: [NO_ERRORS_SCHEMA],
    imports: [
      NgxUpperCaseDirectiveModule,
      HttpClientTestingModule,
      RouterTestingModule,
      TranslateTestingModule.withTranslations({ ja: require('src/assets/i18n/ja.json') }),
      MaterialModule,
      BrowserAnimationsModule,
      ReactiveFormsModule
    ],
    providers: [
      FormBuilder,
      FormattedCurrencyPipe,
      FormattedNumberPipe,
      CurrencyPipe,
      { provide: AccountService, useValue: accountServiceSpy },
      { provide: ProductService, useValue: productServiceSpy },
      { provide: TitleI18Service, useValue: titleI18ServiceSpy },
      { provide: SearchParamsService, useValue: searchParamsServiceSpy }
    ],
    declarations: [ProductListingPageComponent, FormattedCurrencyPipe, FormattedNumberPipe]
  }).compileComponents();
  router = TestBed.inject(Router);
}));
```

beforeEach(() => { の下に下記を追加

```
accountServiceSpy.getUser.and.returnValue(expectedUser);
productServiceSpy.getGenres.and.returnValue(of(expectedGenres));
searchParamsServiceSpy.getProductListingSearchParamsDto.and.returnValue(null);
```

テストメソッドを新たに追加する

```
describe('#ngOnInit', () => {
  it('should get genre on init', () => {
    component.ngOnInit();
    expect(component.genres).toEqual(expectedGenres);
  });
  it('should set user lang to default of translate service on init', () => {
    component.ngOnInit();
    expect(component.translateService.getDefaultLang()).toEqual(expectedUser.userLanguage);
  });
});

describe('#ngAfterViewInit', () => {
  describe('When SearchParamsDto is not registered in SearchParamsService', () => {
    it('should not init search criteria', () => {
      searchParamsServiceSpy.getProductListingSearchParamsDto.and.returnValue(null);
      spyOn(component, 'clickSearchButton').and.callThrough();
      component.ngAfterViewInit();

      expect(component.productName.value).toEqual('');
      expect(component.productCode.value).toEqual('');
      expect(component.productGenre.value).toEqual('');
      expect(component.endOfSale.value).toBeFalsy();
      expect(component.paginator.pageIndex).toEqual(INITIAL_VALUE_PAGE_INDEX);
      expect(component.paginator.pageSize).toEqual(INITIAL_VALUE_PAGE_SIZE);
    });
    it('should not search', () => {
      searchParamsServiceSpy.getProductListingSearchParamsDto.and.returnValue(null);
      spyOn(component, 'clickSearchButton').and.callThrough();
      component.ngAfterViewInit();

      expect(component.clickSearchButton).toHaveBeenCalledTimes(0);
    });
  });

  describe('When SearchParamsDto is registered in SearchParamsService', () => {
    it('should init search criteria', () => {
      searchParamsServiceSpy.getProductListingSearchParamsDto.and.returnValue(expectedProductListingSearchParamsDto);
      productServiceSpy.getProductList.and.returnValue(of(expectedProductSearchListResponseDto));
      component.ngAfterViewInit();

      expect(component.productName.value).toEqual(expectedProductListingSearchParamsDto.productName);
      expect(component.productCode.value).toEqual(expectedProductListingSearchParamsDto.productCode);
      expect(component.productGenre.value).toEqual(expectedProductListingSearchParamsDto.productGenre);
      expect(component.endOfSale.value).toEqual(expectedProductListingSearchParamsDto.endOfSale);
      expect(component.paginator.pageIndex).toEqual(expectedProductListingSearchParamsDto.pageIndex);
      expect(component.paginator.pageSize).toEqual(expectedProductListingSearchParamsDto.pageSize);
    });

    it('should set blank to search criteria when search param is undefined', () => {
      searchParamsServiceSpy.getProductListingSearchParamsDto.and.returnValue(expectedSearchParamsDtoPartlyUndefined);
      productServiceSpy.getProductList.and.returnValue(of(expectedProductSearchListResponseDto));
      component.ngAfterViewInit();

      expect(component.productName.value).toEqual('');
      expect(component.productCode.value).toEqual('');
      expect(component.productGenre.value).toEqual('');
    });

    it('should search', () => {
      searchParamsServiceSpy.getProductListingSearchParamsDto.and.returnValue(expectedProductListingSearchParamsDto);
      productServiceSpy.getProductList.and.returnValue(of(expectedProductSearchListResponseDto));
      spyOn(component, 'clickSearchButton').and.callThrough();
      component.ngAfterViewInit();

      expect(component.clickSearchButton).toHaveBeenCalledTimes(1);
    });
  });
});

describe('#ngAfterViewChecked', () => {
  it('should set title', () => {
    searchParamsServiceSpy.getProductListingSearchParamsDto.and.returnValue(null);
    component.ngAfterViewChecked();
    expect(titleI18ServiceSpy.setTitle.calls.count()).toBeGreaterThan(1);
  });
});

describe('#clickNewButton', () => {
  it('should move to new page', () => {
    searchParamsServiceSpy.getProductListingSearchParamsDto.and.returnValue(null);
    spyOn(router, 'navigate').and.returnValue(null);
    component.clickNewButton();
    expect(searchParamsServiceSpy.removeProductListingSearchParamsDto.calls.count()).toEqual(1);
    expect(router.navigate).toHaveBeenCalledWith([UrlConst.SLASH + UrlConst.PATH_PRODUCT_REGISTERING_NEW]);
  });
});

describe('#clickClearButton', () => {
  it('should clear', () => {
    component.productName.setValue(VALUE_PRODUCT_NAME);
    component.productCode.setValue(VALUE_PRODUCT_CODE);
    component.productGenre.setValue(VALUE_PRODUCT_GENRE);
    component.endOfSale.setValue(VALUE_END_OF_SALE_TRUE);

    component.clickClearButton();

    expect(searchParamsServiceSpy.removeProductListingSearchParamsDto.calls.count()).toEqual(1);
    expect(component.productName.value).toEqual('');
    expect(component.productCode.value).toEqual('');
    expect(component.productGenre.value).toEqual('');
    expect(component.endOfSale.value).toBeFalsy();
  });
});

describe('#clickSearchButton', () => {
  describe('If the page index does not change after searching, the paginator pageindex does not change', () => {
    it('should search', () => {
      productServiceSpy.getProductList.and.returnValue(of(expectedProductSearchListResponseDto));
      component.clickSearchButton();

      expect(productServiceSpy.getProductList.calls.count()).toEqual(1);
      expect(component.productSearchResponseDtos).toEqual(
        expectedProductSearchListResponseDto.productSearchResponseDtos
      );
    });
  });
  describe('If the page index change after searching, paginator pageindex will be overwritten', () => {
    it('should search', () => {
      component.paginator.pageIndex = 1;
      productServiceSpy.getProductList.and.returnValue(of(expectedProductSearchListResponseDto));
      component.clickSearchButton();

      expect(component.paginator.pageIndex).toEqual(expectedProductSearchListResponseDto.pageIndex);
    });
  });
});

describe('#clickListRow', () => {
  it('should move to new page', () => {
    const expectedProductSearchResponseDto: ProductSearchResponseDto =
      expectedProductSearchListResponseDto.productSearchResponseDtos[0];
    spyOn(router, 'navigate').and.returnValue(null);
    component.clickListRow(expectedProductSearchResponseDto);

    expect(router.navigate).toHaveBeenCalledTimes(1);
  });
});

describe('#unselectProductGenre', () => {
  it('should unselect genre', () => {
    component.productGenre.setValue(VALUE_PRODUCT_GENRE);
    component.unselectProductGenre();

    expect(component.productGenre.value).toEqual('');
  });
});

// --------------------------------------------------------------------------------
// DOM test cases
// --------------------------------------------------------------------------------
describe('DOM placeholder', () => {
  it('title', () => {
    const htmlInputElement: HTMLInputElement = fixture.debugElement.query(By.css('#title')).nativeElement;
    expect(htmlInputElement.innerText).toContain('商品一覧');
  });

  it('product name', () => {
    const htmlInputElement: HTMLInputElement = fixture.debugElement.query(By.css('#product-name')).nativeElement;
    expect(htmlInputElement.dataset.placeholder).toContain('商品名');
  });
  it('product code', () => {
    const htmlInputElement: HTMLInputElement = fixture.debugElement.query(By.css('#product-code')).nativeElement;
    expect(htmlInputElement.dataset.placeholder).toContain('商品コード');
  });
  it('product genre', () => {
    const hTMLLabelElement: HTMLLabelElement = fixture.debugElement.query(By.css('#product-genre-label'))
      .nativeElement;
    expect(hTMLLabelElement.innerText).toContain('ジャンル');
  });
  it('end of sale', () => {
    const htmlInputElement: HTMLInputElement = fixture.debugElement.query(By.css('#end-of-sale')).nativeElement;
    expect(htmlInputElement.innerText).toContain('販売終了');
  });

  it('new button', () => {
    const htmlInputElement: HTMLInputElement = fixture.debugElement.query(By.css('#new-button')).nativeElement;
    expect(htmlInputElement.innerText).toContain('新規');
  });
  it('clear button', () => {
    const htmlInputElement: HTMLInputElement = fixture.debugElement.query(By.css('#clear-button')).nativeElement;
    expect(htmlInputElement.innerText).toContain('クリア');
  });
  it('search button', () => {
    const htmlInputElement: HTMLInputElement = fixture.debugElement.query(By.css('#search-button')).nativeElement;
    expect(htmlInputElement.innerText).toContain('検索');
  });
});

describe('DOM input test', () => {
  it('product name', () => {
    const expectedValue = VALUE_PRODUCT_NAME;
    HtmlElementUtility.setValueToHTMLInputElement<typeof component>(fixture, '#product-name', expectedValue);
    expect(component.productName.value).toEqual(expectedValue);
  });
  it('product code', () => {
    const expectedValue = VALUE_PRODUCT_CODE;
    HtmlElementUtility.setValueToHTMLInputElement<typeof component>(fixture, '#product-code', expectedValue);
    expect(component.productCode.value).toEqual(expectedValue.toUpperCase());
  });
  it('product genre', () => {
    const expectedValue = VALUE_PRODUCT_GENRE;
    HtmlElementUtility.setValueToHtmlSelectElement<typeof component>(
      fixture,
      '#product-genre',
      '.product-genre-option',
      Number(expectedValue)
    );
    expect(component.productGenre.value).toEqual(expectedValue);
  });
  it('end of sale', () => {
    // Clicks checkbox's label
    HtmlElementUtility.clickHtmlElement<typeof component>(fixture, '#end-of-sale label');
    expect(component.endOfSale.value).toEqual(VALUE_END_OF_SALE_TRUE);
  });
});

describe('DOM input test', () => {
  describe('#createSearchParamsDto', () => {
    it('Should create product listing search params dto correctly', () => {
      HtmlElementUtility.setValueToHTMLInputElement<typeof component>(fixture, '#product-name', VALUE_PRODUCT_NAME);
      HtmlElementUtility.setValueToHTMLInputElement<typeof component>(fixture, '#product-code', VALUE_PRODUCT_CODE);
      HtmlElementUtility.setValueToHtmlSelectElement<typeof component>(
        fixture,
        '#product-genre',
        '.product-genre-option',
        Number(VALUE_PRODUCT_GENRE)
      );
      HtmlElementUtility.clickHtmlElement<typeof component>(fixture, '#end-of-sale label');

      fixture.detectChanges();

      const privateMethodName = 'createSearchParamsDto';
      const resultProductListingSearchParamsDto: ProductListingSearchParamsDto = component[privateMethodName]();
      expect(resultProductListingSearchParamsDto).toEqual(expectedProductListingSearchParamsDto);
    });
  });
});
```

クラス外に下記を追加する

```
function createExpectedGenres() {
  return Array('1', '2', '3');
}

function createExpectedUser(): User {
  const user: User = new User();
  user.userAccount = 'userAccount';
  user.userCurrency = 'JPY';
  user.userLanguage = 'ja';
  user.userLocale = 'ja-JP';
  user.userName = 'userName';
  user.userTimezone = 'UTC';
  return user;
}

function createExpectedProductListingSearchParamsDto() {
  const productListingSearchParamsDto: ProductListingSearchParamsDto = {
    productCode: 'PRODUCTCODE',
    productName: 'productName',
    productGenre: '1',
    endOfSale: true,
    pageIndex: 0,
    pageSize: 50
  };
  return productListingSearchParamsDto;
}

function createExpectedSearchParamsDtoPartlyUndefined() {
  const productListingSearchParamsDto: ProductListingSearchParamsDto = {
    productCode: undefined,
    productName: undefined,
    productGenre: undefined,
    endOfSale: true,
    pageIndex: 0,
    pageSize: 50
  };
  return productListingSearchParamsDto;
}

function createExpectedProductSearchListResponseDto(): ProductSearchListResponseDto {
  const productSearchResponseDto: ProductSearchResponseDto[] = [
    {
      no: 1,
      productName: 'productName',
      productCode: 'PRODUCTCODE',
      productGenre: '1',
      productImageUrl: 'productImageUrl',
      productSizeStandard: 'productSizeStandard',
      productColor: 'productColor',
      productUnitPrice: 123,
      productStockQuantity: 456,
      endOfSale: false
    }
  ];
  const productSearchListResponseDto: ProductSearchListResponseDto = {
    productSearchResponseDtos: productSearchResponseDto,
    pageIndex: 0,
    resultsLength: 1
  };
  return productSearchListResponseDto;
}
```
