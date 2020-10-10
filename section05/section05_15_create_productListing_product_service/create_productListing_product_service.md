# ① 商品一覧画面の作成

## ① - 1 商品検索サービスの追加

### a. 「BaseSearchListResponseDto」を追加

この dto は一覧検索 dto のベースクラス

#### Cli コマンド

作成コマンド

```
ng g interface pages/models/dtos/responses/base-search-list-response-dto
```

#### 「base-search-list-response-dto.ts」の編集

```
export interface BaseSearchListResponseDto {
  pageIndex: number;
  resultsLength: number;
}
```

### b. 「ProductSearchListResponseDto」を追加

#### Cli コマンド

作成コマンド

```
ng g interface pages/models/dtos/responses/product-search-list-response-dto
```

#### 「product-search-list-response-dto.ts」の編集

```
export interface ProductSearchListResponseDto extends BaseSearchListResponseDto {
  productSearchResponseDtos: ProductSearchResponseDto[];
}
```

### c. 「product.service」の作成

#### Cli コマンド

作成コマンド

```
ng g service pages/services/product
```

#### 「product.service.ts」の編集

コンストラクタ追加

```
constructor(
  private http: HttpClient,
  private errorMessageService: ErrorMessagingService
) {}
```

メソッドを追加

```
/**
 * Gets product list
 * @param httpParams search params
 * @returns product search response
 */
getProductList(httpParams: HttpParams): Observable<ProductSearchListResponseDto> {
  const webApiUrl = ApiConst.PATH_API_ROOT + ApiConst.PATH_PRODUCT_SEARCH;
  this.clearMessageProperty();

  return this.http
    .get<ProductSearchListResponseDto>(webApiUrl, { params: httpParams })
    .pipe(
      catchError((error) => {
        this.errorMessageService.setupPageErrorMessageFromResponse(error);
        return of(null as ProductSearchListResponseDto);
      })
    );
}

/**
 * Gets genres
 * @returns genres
 */
getGenres(): Observable<string[]> {
  const webApiUrl = ApiConst.PATH_API_ROOT + ApiConst.PATH_GENRE;
  this.clearMessageProperty();

  return this.http.get<string[]>(webApiUrl).pipe(
    catchError((error) => {
      this.errorMessageService.setupPageErrorMessageFromResponse(error);
      return of([] as string[]);
    })
  );
}
// --------------------------------------------------------------------------------
// private methods
// --------------------------------------------------------------------------------
private clearMessageProperty() {
  this.errorMessageService.clearMessageProperty();
}
```

## ① - 2 商品検索機能の組込み

### a. 「BaseSearchParamsDto」を追加

検索条件まとめた dto のベースクラス

#### Cli コマンド

作成コマンド

```
ng g interface pages/models/dtos/requests/base-search-params-dto
```

#### 「base-search-params-dto.ts」の編集

```
export interface BaseSearchParamsDto {
  pageSize: number;
  pageIndex: number;
}
```

### b. 「ProductListingSearchParamsDto」を追加

商品一覧画面の検索条件をまとめた dto

#### Cli コマンド

作成コマンド

```
ng g interface pages/models/dtos/requests/product-listing-search-params-dto
```

#### 「product-listing-search-params-dto.ts」の編集

```
export interface ProductListingSearchParamsDto extends BaseSearchParamsDto {
  productName: string;
  productCode: string;
  productGenre: string;
  endOfSale: boolean;
}
```

### c. 「product-listing-page.component.ts」の編集

#### コンストラクタに下記を追加

```
private productService: ProductService,
```

#### ngOnInit

#ngOnInit に loadData を追加

```
/**
  * on init
  */
ngOnInit(): void {
  this.loadData();
  this.setupLanguage();
}
```

#### clickClearButton

clickClearButton の中身を追加

```
/**
 * Clicks clear button
 */
clickClearButton(): void {
  this.clearSearchConditions();
  this.clearSearchResultList();
}
```

#### clickSearchButton

#clickSearchButton の中身を追加

```
/**
  * Clicks search button
  */
clickSearchButton(): void {
  merge(this.paginator.page)
    .pipe(
      startWith({}),
      switchMap(() => {
        this.loadingService.startLoading();
        const productListingSearchParamsDto: ProductListingSearchParamsDto = this.createSearchParamsDto();

        return this.productService.getProductList(this.createHttpParams(productListingSearchParamsDto));
      }),
      map((data) => {
        this.loadingService.stopLoading();
        this.resultsLength = data.resultsLength;
        if (this.paginator.pageIndex !== data.pageIndex) {
          this.paginator.pageIndex = data.pageIndex;
        }

        return data.productSearchResponseDtos;
      })
    )
    .subscribe((data) => (this.productSearchResponseDtos = data));
}
```

#### clickListRow

#clickListRow の中身を追加

```
/**
 * Clicks list row
 * @param productSearchResponseDto Product search response dto
 */
clickListRow(productSearchResponseDto: ProductSearchResponseDto): void {
  this.routingService.router.navigate([UrlConst.PATH_PRODUCT_REGISTERING, productSearchResponseDto.productCode]);
}
```

#### unselectProductGenre

#unselectProductGenre の中身を追加

```
/**
 * Unselects product genre
 */
unselectProductGenre(): void {
  this.productGenre.setValue('');
}
```

#### プライベートメソッド

プライベートメソッドを追加

```
private loadData(): void {
  this.productService.getGenres().subscribe((data) => (this.genres = data));
}
```

```
private createSearchParamsDto(): ProductListingSearchParamsDto {
  const productListingSearchParamsDto: ProductListingSearchParamsDto = {
    productName: this.productName.value,
    productCode: this.productCode.value,
    productGenre: this.productGenre.value,
    endOfSale: this.endOfSale.value,
    pageSize: this.paginator.pageSize,
    pageIndex: this.paginator.pageIndex
  };
  return productListingSearchParamsDto;
}

private createHttpParams(productListingSearchParamsDto: ProductListingSearchParamsDto): HttpParams {
  const paramsOptions = { fromObject: productListingSearchParamsDto } as any;
  const params = new HttpParams(paramsOptions);
  return params;
}

private clearSearchConditions(): void {
  this.productName.setValue('');
  this.productCode.setValue('');
  this.productGenre.setValue('');
  this.endOfSale.setValue(false);
  this.paginator.pageSize = this.initialPageSize;
  this.paginator.pageIndex = 0;
}

private clearSearchResultList(): void {
  this.productSearchResponseDtos = null;
  this.resultsLength = 0;
}
```

その他、クイックフィックスできなかった場合

```
import { ProductService } from '../../services/product.service';
import {
    ProductListingSearchParamsDto
} from '../../models/dtos/requests/product-listing-search-params-dto';

```
