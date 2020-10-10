# ① テストコードの作成

## ① - 1 SearchParams サービス

### a. 「search-params.service.spec.ts」 を編集する

もともと書かれていたソースを describe でかこむ

```
describe('#constractor', () => {
  it('should be created', () => {
    expect(service).toBeTruthy();
  });
});
```

describe('SearchParamsService', () => { の下に const を追加

```
const expectedProductListingSearchParamsDto: ProductListingSearchParamsDto = createProductListingSearchParamsDto();

```

beforeEach(() => {}); は、まとめると下記になる

```
beforeEach(() => {
  TestBed.configureTestingModule({});
  service = TestBed.inject(SearchParamsService);
});
```

テストメソッドを新たに追加する

```
describe('#setProductListingSearchParamsDto,#getProductListingSearchParamsDto', () => {
  it('should set value', () => {
    service.setProductListingSearchParamsDto(expectedProductListingSearchParamsDto);
    const res: ProductListingSearchParamsDto = service.getProductListingSearchParamsDto();
    expect(res).toEqual(expectedProductListingSearchParamsDto);
  });
});

describe('#removeProductListingSearchParamsDto', () => {
  it('should remove value', () => {
    service.setProductListingSearchParamsDto(expectedProductListingSearchParamsDto);
    service.removeProductListingSearchParamsDto();
    expect(service.getProductListingSearchParamsDto()).toBeNull();
  });
});
```

クラスの外側に追加

```
function createProductListingSearchParamsDto() {
  const expectedProductListingSearchParamsDto: ProductListingSearchParamsDto = {
    productCode: 'productCode',
    productName: 'productName',
    productGenre: '1',
    endOfSale: true,
    pageIndex: 0,
    pageSize: 10
  };
  return expectedProductListingSearchParamsDto;
}
```
