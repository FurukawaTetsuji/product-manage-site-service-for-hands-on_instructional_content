# ① 検索条件の保存

## ① - 1 検索条件の保存機能の追加

### a. AppConst に定数を追加

#### 「app-const.ts」の編集

下記を追加する

```
static readonly STORAGE_KEY_SEARCH_PARAMS_PRODUCT_LIST = 'SEARCH_PARAMS_PRODUCT_LIST';
```

### b. 「search-params.service」の作成

#### Cli コマンド

作成コマンド

```
ng g service pages/services/search-params
```

#### 「search-params.service.ts」の編集

メソッドを追加

```
/**
 * Sets product listing search params
 * @param productListingSearchParamsDto product listing search params
 */
setProductListingSearchParamsDto(productListingSearchParamsDto: ProductListingSearchParamsDto): void {
  SessionStorageService.setItem(AppConst.STORAGE_KEY_SEARCH_PARAMS_PRODUCT_LIST, productListingSearchParamsDto);
}

/**
 * Gets product listing search params
 * @returns product listing search params
 */
getProductListingSearchParamsDto(): ProductListingSearchParamsDto {
  return SessionStorageService.getItem(AppConst.STORAGE_KEY_SEARCH_PARAMS_PRODUCT_LIST, {
    productName: '',
    productCode: '',
    productGenre: '',
    endOfSale: false,
    pageSize: 0,
    pageIndex: 0
  });
}

/**
 * Removes product listing search params
 */
removeProductListingSearchParamsDto(): void {
  SessionStorageService.removeItem(AppConst.STORAGE_KEY_SEARCH_PARAMS_PRODUCT_LIST);
}
```

### c. 「product-listing-page.component.ts」の編集

#### コンストラクタ

コンストラクタに下記を追加

```
private searchParamsService: SearchParamsService,
```

#### ngAfterViewInit

クラスの`implements` に`AfterViewInit`を追加

```
export class ProductListingPageComponent implements OnInit, AfterViewChecked, AfterViewInit {
```

#### ngAfterViewInit

ngAfterViewInit を追加

```
/**
 * after view init
 */
ngAfterViewInit(): void {
  this.setupSearchConditions();
}
```

#### clickNewButton

clickNewButton に searchParamsService.removeProductListingSearchParamsDto を追加

```
clickNewButton(): void {
  this.searchParamsService.removeProductListingSearchParamsDto();
  this.routingService.navigate(UrlConst.PATH_PRODUCT_REGISTERING_NEW);
}
```

#### clickClearButton

#clickClearButton に searchParamsService.removeProductListingSearchParamsDto を追加

```
clickClearButton(): void {
  this.searchParamsService.removeProductListingSearchParamsDto();
  this.clearSearchConditions();
  this.clearSearchResultList();
}
```

#### clickSearchButton

#clickSearchButton に searchParamsService.setProductListingSearchParamsDto を追加

```
clickSearchButton(): void {
  merge(this.paginator.page)
    .pipe(
      startWith({}),
      switchMap(() => {
        this.loadingService.startLoading();
        const productListingSearchParamsDto: ProductListingSearchParamsDto = this.createSearchParamsDto();
        this.searchParamsService.setProductListingSearchParamsDto(productListingSearchParamsDto);

        return this.productService.getProductList(this.createHttpParams(productListingSearchParamsDto));
      }),
      map((data) => {
        this.loadingService.stopLoading();
        this.resultsLength = data.resultsLength;
        this.paginator.pageIndex = data.pageIndex;

        return data.productSearchResponseDtos;
      })
    )
    .subscribe((data) => (this.productSearchResponseDtos = data));
}
```

#### setupSearchConditions

プライベートメソッドを追加する
検索条件が保存されている場合それをもとに検索するメソッド

```
private setupSearchConditions(): void {
  // Gets past search conditions from searchParamsService.
  const productListingSearchParamsDto = this.searchParamsService.getProductListingSearchParamsDto();
  // If the past search conditions can be acquired, the value is set on the screen.
  if (productListingSearchParamsDto) {
    if (productListingSearchParamsDto.productName) {
      this.productName.setValue(productListingSearchParamsDto.productName);
    }
    if (productListingSearchParamsDto.productCode) {
      this.productCode.setValue(productListingSearchParamsDto.productCode);
    }
    if (productListingSearchParamsDto.productGenre) {
      this.productGenre.setValue(productListingSearchParamsDto.productGenre);
    }
    this.paginator.pageSize = productListingSearchParamsDto.pageSize;
    this.paginator.pageIndex = productListingSearchParamsDto.pageIndex;
    this.endOfSale.setValue(productListingSearchParamsDto.endOfSale);
    // Displays in the searched state.
    this.clickSearchButton();
  }
}
```

クイックフィックスが効かない場合

```
import { SearchParamsService } from '../../services/search-params.service';
```
