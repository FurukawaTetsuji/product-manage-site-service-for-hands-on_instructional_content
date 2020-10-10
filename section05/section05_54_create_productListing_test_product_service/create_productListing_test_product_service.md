# ① テストコードの作成

## ① - 1 Product サービス

### a. 「product.service.spec.ts」 を編集する

もともと書かれていたソースを describe でかこむ

```
describe('#constractor', () => {
  it('should be created', () => {
    expect(service).toBeTruthy();
  });
});
```

describe('ProductService', () => { の下に下記を追加

```
const expectedProductSearchResponseDto: ProductSearchResponseDto = createProductSearchResponseDto();
const expectedProductSearchListResponseDto: ProductSearchListResponseDto = {
  productSearchResponseDtos: Array(expectedProductSearchResponseDto),
  pageIndex: 0,
  resultsLength: 1
};
const expectedGenresArrayResponse: string[] = createGenresArray();
```

let の下に下記を追加

```
let httpTestingController: HttpTestingController;
let errorMessagingServiceSpy: { clearMessageProperty: jasmine.Spy; setupPageErrorMessageFromResponse: jasmine.Spy };
```

beforeEach(() => { の下に下記を追加

```
errorMessagingServiceSpy = jasmine.createSpyObj('ErrorMessagingService', [
  'clearMessageProperty',
  'setupPageErrorMessageFromResponse'
]);
```

TestBed.configureTestingModule({　の下に下記を追加

```
schemas: [NO_ERRORS_SCHEMA],
imports: [HttpClientTestingModule],
providers: [
  { provide: ErrorMessagingService, useValue: errorMessagingServiceSpy }
]
```

service = TestBed.inject(ProductService); の下に下記を追加

```
httpTestingController = TestBed.inject(HttpTestingController);
```

beforeEach(() => {}); は、まとめると下記になる

```
beforeEach(() => {
  successMessagingServiceSpy = jasmine.createSpyObj('SuccessMessagingService', [
    'clearMessageProperty',
    'setMessageProperty'
  ]);
  errorMessagingServiceSpy = jasmine.createSpyObj('ErrorMessagingService', [
    'clearMessageProperty',
    'setupPageErrorMessageFromResponse'
  ]);
  TestBed.configureTestingModule({
    schemas: [NO_ERRORS_SCHEMA],
    imports: [HttpClientTestingModule],
    providers: [
      { provide: SuccessMessagingService, useValue: successMessagingServiceSpy },
      { provide: ErrorMessagingService, useValue: errorMessagingServiceSpy }
    ]
  });
  service = TestBed.inject(ProductService);
  httpTestingController = TestBed.inject(HttpTestingController);
});
```

beforeEach(() => {}); の下に afterEach メソッドを追加

```
afterEach(() => {
  // After every test, assert that there are no more pending requests.
  httpTestingController.verify();
});
```

テストメソッドを新たに追加する

```
describe('#getProductList', () => {
  const webApiUrl = ApiConst.PATH_API_ROOT + ApiConst.PATH_PRODUCT_SEARCH;

  it('should return expected response', () => {
    service.getProductList(new HttpParams()).subscribe((response) => {
      expect(response).toEqual(expectedProductSearchListResponseDto);
      expect(errorMessagingServiceSpy.setupPageErrorMessageFromResponse.calls.count()).toBe(0);
    }, fail);

    const req = httpTestingController.expectOne(webApiUrl);
    expect(req.request.method).toEqual('GET');
    expect(successMessagingServiceSpy.clearMessageProperty.calls.count()).toBe(1);
    expect(errorMessagingServiceSpy.clearMessageProperty.calls.count()).toBe(1);

    // Respond with the mock
    req.flush(expectedProductSearchListResponseDto);
  });

  it('should return null 404 Not Found', () => {
    const msg = '404 Not Found';
    service.getProductList(new HttpParams()).subscribe((response) => {
      expect(response).toBeNull();
      expect(errorMessagingServiceSpy.setupPageErrorMessageFromResponse.calls.count()).toBe(1);
    }, fail);

    const req = httpTestingController.expectOne(webApiUrl);
    expect(req.request.method).toEqual('GET');
    expect(successMessagingServiceSpy.clearMessageProperty.calls.count()).toBe(1);
    expect(errorMessagingServiceSpy.clearMessageProperty.calls.count()).toBe(1);

    // Respond with the mock
    req.flush(msg, { status: 404, statusText: '404 Not Found' });
  });
});

describe('#getGenres', () => {
  const webApiUrl = ApiConst.PATH_API_ROOT + ApiConst.PATH_GENRE;

  it('should return expected response', () => {
    service.getGenres().subscribe((response) => {
      expect(response).toEqual(expectedGenresArrayResponse);
      expect(errorMessagingServiceSpy.setupPageErrorMessageFromResponse.calls.count()).toBe(0);
    }, fail);

    const req = httpTestingController.expectOne(webApiUrl);
    expect(req.request.method).toEqual('GET');
    expect(successMessagingServiceSpy.clearMessageProperty.calls.count()).toBe(1);
    expect(errorMessagingServiceSpy.clearMessageProperty.calls.count()).toBe(1);

    // Respond with the mock
    req.flush(expectedGenresArrayResponse);
  });

  it('should return [] 404 Not Found', () => {
    const msg = '404 Not Found';
    service.getGenres().subscribe((response) => {
      expect(response).toEqual([]);
      expect(errorMessagingServiceSpy.setupPageErrorMessageFromResponse.calls.count()).toBe(1);
    }, fail);

    const req = httpTestingController.expectOne(webApiUrl);
    expect(req.request.method).toEqual('GET');
    expect(successMessagingServiceSpy.clearMessageProperty.calls.count()).toBe(1);
    expect(errorMessagingServiceSpy.clearMessageProperty.calls.count()).toBe(1);

    // Respond with the mock
    req.flush(msg, { status: 404, statusText: '404 Not Found' });
  });
});
```

クラスの外側に追加

```
function createProductSearchResponseDto(): ProductSearchResponseDto {
  return {
    endOfSale: false,
    no: 1,
    productCode: 'productCode',
    productColor: 'productColor',
    productGenre: '1',
    productImageUrl: 'productImageUrl',
    productName: 'productName',
    productSizeStandard: 'productSizeStandard',
    productStockQuantity: 1,
    productUnitPrice: 1
  };
}

function createGenresArray(): string[] {
  return Array('1', '2', '3');
}
```
