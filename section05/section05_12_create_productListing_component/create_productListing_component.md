# ① 商品一覧画面の作成

## ① - 1 準備

### a. 「product-listing-page.component.html」の準備

ひな形のレポジトリから「ProductListingPage.html」の中身をコピーする  
https://github.com/FurukawaTetsuji/product-manage-site-service-for-hands-on_html-example

```
<section class="listing-page-wrapper">
  <div class="listing-title-wrapper">
    <h1>商品一覧</h1>
  </div>
  <div class="search-criteria-wrapper">
    <form>
      <div class="search-criteria">
        <div class="search-condition-wrapper">
          <input id="product-name" type="text" placeholder="商品名">
        </div>
        <div class="search-condition-wrapper">
          <input id="product-code" type="text" placeholder="商品コード">
        </div>
        <div class="search-condition-wrapper">
          <select id="product-genre-label" value="">
            <option value='' disabled selected style='display:none;'>ジャンル</option>
            <option value="1">靴・スニーカー</option>
            <option value="2">トップス</option>
            <option value="3">バッグ</option>
          </select>
        </div>
        <div class="search-condition-wrapper">
            <input id="end-of-sale" type="checkbox" value="1">販売終了
        </div>
      </div>
    </form>
  </div>
  <div class="search-functions-wrapper">
    <div class="search-functions-paginator">
      -- Paginator area --
    </div>
    <div class="search-functions-buttons">
      <a href="#" id="new-button" class="button">新規</a>
      <a href="#" id="clear-button" class="button">クリア</a>
      <a href="#" id="search-button" class="button active">検索</a>
    </div>
  </div>
  <div class="search-results-wrapper">
    <table class="search-results">
      <thead>
        <tr>
          <th class="width-no">No.</th>
          <th class="width-product-name">商品名</th>
          <th class="width-product-code">商品コード</th>
          <th class="width-product-genre">ジャンル</th>
          <th class="width-product-image">イメージ</th>
          <th class="width-product-size">サイズ・規格</th>
          <th class="width-product-color">色</th>
          <th class="width-product-unit-price">単価</th>
          <th class="width-product-stock">在庫数</th>
          <th class="width-product-end">販売終了</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td class="width-no">1</td>
          <td class="width-product-name product-name">花柄のスニーカー</td>
          <td class="width-product-code"><a href="ProductRegisteringPage.html">ABCD123</a></td>
          <td class="width-product-genre">靴・スニーカー</td>
          <td class="width-product-image"><img src="img/ABCD123.png" /></td>
          <td class="width-product-size product-size">24,25,26,27cm</td>
          <td class="width-product-color">花柄</td>
          <td class="width-product-unit-price product-unit-price">2,500</td>
          <td class="width-product-stock product-stock">500</td>
          <td class="width-product-end">-</td>
        </tr>
        <tr>
          <td class="width-no">2</td>
          <td class="width-product-name product-name">男性革靴</td>
          <td class="width-product-code"><a href="ProductRegisteringPage.html">ABCD456</a></td>
          <td class="width-product-genre">靴・スニーカー</td>
          <td class="width-product-image"><img src="img/ABCD456.png" /></td>
          <td class="width-product-size product-size">25,26,27,28cm</td>
          <td class="width-product-color">茶</td>
          <td class="width-product-unit-price product-unit-price">12,800</td>
          <td class="width-product-stock product-stock">100</td>
          <td class="width-product-end">-</td>
        </tr>
        <tr>
          <td class="width-no">3</td>
          <td class="width-product-name product-name">カバン</td>
          <td class="width-product-code"><a href="ProductRegisteringPage.html">XYZ0123</a></td>
          <td class="width-product-genre">バッグ</td>
          <td class="width-product-image"><img src="img/XYZ0123.png" /></td>
          <td class="width-product-size product-size">フリー</td>
          <td class="width-product-color">茶</td>
          <td class="width-product-unit-price product-unit-price">8,800</td>
          <td class="width-product-stock product-stock">200</td>
          <td class="width-product-end">-</td>
        </tr>
        <tr>
          <td class="width-no">4</td>
          <td class="width-product-name product-name">セーター</td>
          <td class="width-product-code"><a href="ProductRegisteringPage.html">ABCD654</a></td>
          <td class="width-product-genre">トップス</td>
          <td class="width-product-image"><img src="img/ABCD654.png" /></td>
          <td class="width-product-size product-size">S/M/L</td>
          <td class="width-product-color">グレー</td>
          <td class="width-product-unit-price product-unit-price">7,800</td>
          <td class="width-product-stock product-stock">300</td>
          <td class="width-product-end">-</td>
        </tr>
        <tr>
          <td class="width-no">5</td>
          <td class="width-product-name product-name">ビジネスバッグ</td>
          <td class="width-product-code"><a href="ProductRegisteringPage.html">XYZ3210</a></td>
          <td class="width-product-genre">バッグ</td>
          <td class="width-product-image"><img src="img/XYZ3210.png" /></td>
          <td class="width-product-size product-size">30×70cm</td>
          <td class="width-product-color">黒/茶</td>
          <td class="width-product-unit-price product-unit-price">24,000</td>
          <td class="width-product-stock product-stock">400</td>
          <td class="width-product-end">-</td>
        </tr>
        <tr>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
        </tr>
        <tr>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
        </tr>
        <tr>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
        </tr>
        <tr>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
        </tr>
        <tr>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
        </tr>
        <tr>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
        </tr>
        <tr>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
        </tr>
      </tbody>
    </table>
  </div>
</section>
```

### b. 「product-listing-page.component.html」の編集

#### タイトル

```
<h1>商品一覧</h1>
```

↓

```
<h1 id="title">{{ 'productListingPage.title' | translate }}</h1>
```

#### エラーメッセージディレクティブ（コンポーネント）を追加

４行目の下に追加する

```
<app-error-messaging></app-error-messaging>
```

#### フォーム

```
<form>
```

↓

```
<form [formGroup]="searchForm">
```

#### 商品名

```
<input id="product-name" type="text" placeholder="商品名">
```

↓

```
<mat-form-field class="form-field">
  <input id="product-name" matInput type="text" formControlName="productName"
    placeholder="{{ 'productListingPage.productName' | translate }}" maxlength=50
    matTooltip="{{ 'tooltip.searchCriteria.prefixMatching' | translate }}">
</mat-form-field>
```

#### 商品コード

```
<input id="product-code" type="text" placeholder="商品コード">
```

↓

```
<mat-form-field class="form-field">
  <input id="product-code" matInput type="text" formControlName="productCode"
    placeholder="{{ 'productListingPage.productCode' | translate }}" maxlength=20
    matTooltip="{{ 'tooltip.searchCriteria.prefixMatching' | translate }}">
</mat-form-field>
```

#### ジャンル

```
<select id="product-genre-label" value="">
  <option value='' disabled selected style='display:none;'>ジャンル</option>
  <option value="1">靴・スニーカー</option>
  <option value="2">トップス</option>
  <option value="3">バッグ</option>
</select>
```

↓

```
<mat-form-field class="form-field">
  <mat-label id="product-genre-label">{{ 'productListingPage.productGenre' | translate }}</mat-label>
  <mat-select id="product-genre" formControlName="productGenre"
    matTooltip="{{ 'tooltip.searchCriteria.pullDown' | translate }}">
    <mat-option class="product-genre-option" (click)="unselectProductGenre()"></mat-option>
    <mat-option class="product-genre-option" *ngFor="let code of genres" [value]="code">
      {{ 'genre.' + code | translate }}
    </mat-option>
  </mat-select>
</mat-form-field>
```

#### 販売終了

```
<input id="end-of-sale" type="checkbox" name="product-end" value="1">販売終了
```

↓

```
<mat-checkbox id="end-of-sale" formControlName="endOfSale"
  matTooltip="{{ 'productListingPage.tooltip.endOfSale' | translate }}">
  {{ 'productListingPage.endOfSale' | translate }}
</mat-checkbox>
```

##### ページネーター

```
-- Paginator area --
```

↓

```
<mat-paginator [length]="resultsLength" [pageSize]="initialPageSize" [pageSizeOptions]="[10, 50, 100]">
</mat-paginator>
```

#### ボタン

```
<a href="#" id="new-button" class="button">新規</a>
<a href="#" id="clear-button" class="button">クリア</a>
<a href="#" id="search-button" class="button active">検索</a>
```

↓

```
<button mat-flat-button id="new-button" class="flat-button" (click)="clickNewButton()"
  matTooltip="{{ 'productListingPage.tooltip.newBtn' | translate }}">{{ 'productListingPage.newButton' | translate }}</button>

<button mat-flat-button id="clear-button" class="flat-button" (click)="clickClearButton()"
  matTooltip="{{ 'tooltip.searchBtn.clearBtn' | translate }}">{{ 'productListingPage.clearButton' | translate }}</button>

<button mat-flat-button id="search-button" class="flat-button active" (click)="clickSearchButton()"
  matTooltip="{{ 'tooltip.searchBtn.searchBtn' | translate }}">{{ 'productListingPage.searchButton' | translate }}</button>
```

#### 一覧部分

下記の部分は削除

```
<table class="search-results">
  <thead>
    <tr>
      <th class="width-no">No.</th>
      <th class="width-product-name">商品名</th>
      <th class="width-product-code">商品コード</th>
      <th class="width-product-genre">ジャンル</th>
      <th class="width-product-image">イメージ</th>
      <th class="width-product-size">サイズ・規格</th>
      <th class="width-product-color">色</th>
      <th class="width-product-unit-price">単価</th>
      <th class="width-product-stock">在庫数</th>
      <th class="width-product-end">販売終了</th>
    </tr>
  </thead>
  <tbody>

    ～

  </tbody>
</table>
```

下に書き換える
↓

```
<table id="results" class="search-results" mat-table *ngIf="resultsLength>0"
  [dataSource]="productSearchResponseDtos">
  <ng-container matColumnDef="no">
    <th class="width-no" mat-header-cell *matHeaderCellDef>
      {{ 'productListingPage.no' | translate }}
    </th>
    <td class="width-no" mat-cell *matCellDef="let element"> {{element.no}} </td>
  </ng-container>
  <ng-container matColumnDef="productName">
    <th class="width-product-name" mat-header-cell *matHeaderCellDef>
      {{ 'productListingPage.productName' | translate }}
    </th>
    <td class="width-product-name" mat-cell *matCellDef="let element">
      {{element.productName}} </td>
  </ng-container>
  <ng-container matColumnDef="productCode">
    <th class="width-product-code" mat-header-cell *matHeaderCellDef>
      {{ 'productListingPage.productCode' | translate }}</th>
    <td class="width-product-code" mat-cell *matCellDef="let element">
      {{element.productCode}} </td>
  </ng-container>
  <ng-container matColumnDef="productGenre">
    <th class="width-product-genre" mat-header-cell *matHeaderCellDef>
      {{ 'productListingPage.productGenre' | translate }}</th>
    <td class="width-product-genre" mat-cell *matCellDef="let element">
      {{ 'genre.' + element.productGenre | translate }} </td>
  </ng-container>
  <ng-container matColumnDef="productImage">
    <th class="width-product-image" mat-header-cell *matHeaderCellDef>
      {{ 'productListingPage.productImage' | translate }}</th>
    <td class="width-product-image" mat-cell *matCellDef="let element"><img src={{element.productImageUrl}} /></td>
  </ng-container>
  <ng-container matColumnDef="productSizeStandard">
    <th class="width-product-size" mat-header-cell *matHeaderCellDef>
      {{ 'productListingPage.productSizeStandard' | translate }}</th>
    <td class="width-product-size" mat-cell *matCellDef="let element">
      {{element.productSizeStandard}} </td>
  </ng-container>
  <ng-container matColumnDef="productColor">
    <th class="width-product-color" mat-header-cell *matHeaderCellDef>
      {{ 'productListingPage.productColor' | translate }}</th>
    <td class="width-product-color" mat-cell *matCellDef="let element">
      {{element.productColor}} </td>
  </ng-container>
  <ng-container matColumnDef="productUnitPrice">
    <th class="width-product-unit-price" mat-header-cell *matHeaderCellDef>
      {{ 'productListingPage.productUnitPrice' | translate }}</th>
    <td class="width-product-unit-price" mat-cell *matCellDef="let element">
      {{element.productUnitPrice }}</td>
  </ng-container>
  <ng-container matColumnDef="productStockQuantity">
    <th class="width-product-stock" mat-header-cell *matHeaderCellDef>
      {{ 'productListingPage.productStockQuantity' | translate }}</th>
    <td class="width-product-stock" mat-cell *matCellDef="let element">
      {{element.productStockQuantity }} </td>
  </ng-container>
  <ng-container matColumnDef="endOfSale">
    <th class="width-endOfSale" mat-header-cell *matHeaderCellDef>
      {{ 'productListingPage.endOfSale' | translate }}</th>
    <td class="width-endOfSale" mat-cell *matCellDef="let element">
      {{ 'productListingPage.endOfSaleValue.' + element.endOfSale | translate}} </td>
  </ng-container>
  <tr mat-header-row *matHeaderRowDef="displayColumns; sticky: true"></tr>
  <tr mat-row *matRowDef="let row; columns: displayColumns;" (click)="clickListRow(row)"></tr>
</table>
```

### c. 「style.css」の編集

#### 検索条件

下は削除

```
.search-criteria input[type=text] {
  border-width: 0;
  display: block;
}
```

↓

下のコメントを外す

```
.search-criteria .form-field {
  margin: 0 auto 0;
  width: 200px;
}

.search-criteria .date-from .form-field,
.search-criteria .date-to .form-field {
  width: 100px;
}
```

#### ボタン

下は削除

```
.search-functions-wrapper .button {
  display: inline-block;
  padding: 10px 0;
  margin-right: 5px;
  width: 200px;
  border-radius: 2px;
  background-color: #78BBE6;
  color: #ffffff;
  text-decoration: none;
  text-align: center;
  letter-spacing: 1px;
  font-size: 1.2rem;
}

.search-functions-wrapper .button:hover {
  opacity: 0.8;
}

.search-functions-wrapper .button.active {
  background-color: #F99F48;
  color: #ffffff;
}
```

↓

下のコメントを外す

```
.search-functions-wrapper .mat-paginator-container {
  justify-content: left;
  padding-left: 8px;
  min-height: 50px;
  min-width: 400px;
  background-color: #ECEFF1;
}

.search-functions-wrapper .flat-button {
  margin-right: 10px;
  width: 200px;
  border-radius: 2px;
  background-color: #78BBE6;
  color: #ffffff;
  letter-spacing: 1px;
  font-size: 1.2rem;
}

.search-functions-wrapper .flat-button:hover {
  opacity: 0.8;
}

.search-functions-wrapper .flat-button.active {
  background-color: #F99F48;
  color: #ffffff;
}
```

#### 検索結果の一覧

新たに作るので下記は削除する

```
.search-results-wrapper .search-results .width-no {
  width: 5%;
}

.search-results-wrapper .search-results .width-product-name {
  width: 30%;
}

.search-results-wrapper .search-results .width-product-code {
  width: 5%;
}

.search-results-wrapper .search-results .width-product-genre {
  width: 10%;
}

.search-results-wrapper .search-results .width-product-image {
  width: 100px;
}

.search-results-wrapper .search-results .width-product-size {
  width: 25%;
}

.search-results-wrapper .search-results .width-product-color {
  width: 5%;
}

.search-results-wrapper .search-results .width-product-unit-price {
  width: 5%;
}

.search-results-wrapper .search-results .width-product-stock {
  width: 5%;
}

.search-results-wrapper .search-results .width-product-end {
  width: 5%;
}

.search-results-wrapper .search-results .product-name,
.search-results-wrapper .search-results .product-size {
  text-align: left;
}

.search-results-wrapper .search-results .product-unit-price,
.search-results-wrapper .search-results .product-stock {
  text-align: right;
}

.search-results-wrapper table img {
  max-height: 30px;
}
```

### d. 「product-listing-page.component.scss」の編集

下記を新たに追加する。

```
/* --------------------------------------------------------------------------------
 * Product Listing page specific settings.
 * -------------------------------------------------------------------------------- */
.search-results-wrapper .search-results .width-product-name,
.search-results-wrapper .search-results .width-product-code,
.search-results-wrapper .search-results .width-product-genre,
.search-results-wrapper .search-results .width-product-image,
.search-results-wrapper .search-results .width-product-size,
.search-results-wrapper .search-results .width-product-color {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  text-align: left;
}

.search-results-wrapper .search-results .width-product-unit-price,
.search-results-wrapper .search-results .width-product-stock {
  text-align: right;
}

.search-results-wrapper .search-results td.width-endOfSale {
  text-align: center;
}


/* no */
.search-results-wrapper .search-results .width-no {
  padding-left: 5px;
  max-width: 10px;
  width: 5%;
}

/* product name */
.search-results-wrapper .search-results .width-product-name {
  max-width: 100px;
  width: 15%;
}

/* product code */
.search-results-wrapper .search-results .width-product-code {
  max-width: 100px;
  width: 15%;
}

/* product genre */
.search-results-wrapper .search-results .width-product-genre {
  max-width: 200px;
  width: 10%;
}

/* product image */
.search-results-wrapper .search-results .width-product-image {
  max-width: 50px;
  width: 10%;
}

/* product size */
.search-results-wrapper .search-results .width-product-size {
  max-width: 100px;
  width: 15%;
}

/* product color */
.search-results-wrapper .search-results .width-product-color {
  max-width: 60px;
  width: 10%;
}

/* product unit price */
.search-results-wrapper .search-results .width-product-unit-price {
  max-width: 10%;
  width: 10%;
}

/* product stock */
.search-results-wrapper .search-results .width-product-stock {
  max-width: 10%;
  width: 10%;
}

/* end of sale */
.search-results-wrapper .search-results .width-endOfSale {
  max-width: 10%;
  width: 10%;
}

/* end of sale th only */
.search-results-wrapper .search-results th.width-endOfSale {
  padding-right: 40px;
}

.search-results-wrapper table img {
  max-width: 50px;
  max-height: 30px;
}
```

### e. 一覧のインターフェース作成

#### cli コマンド

```
ng g interface pages/models/dtos/responses/product-search-response-dto
```

#### 「product-search-response-dto.ts」の編集

下記の変数を追加する。

```
export interface ProductSearchResponseDto {
  no: number;
  productName: string;
  productCode: string;
  productGenre: string;
  productImageUrl: string;
  productSizeStandard: string;
  productColor: string;
  productUnitPrice: number;
  productStockQuantity: number;
  endOfSale: boolean;
}
```

### f. 「product-listing-page.component.ts」の編集

#### class に下記を追加

```
, AfterViewChecked
```

#### コンストラクタ

コンストラクタに追加する

```
constructor(
  private accountService: AccountService,
  private formBuilder: FormBuilder,
  private loadingService: LoadingService,
  private routingService: RoutingService,
  private titleI18Service: TitleI18Service,
  public translateService: TranslateService
) {}
```

##### フォームコントロールや変数

下記を追加する

```
productName = new FormControl('', []);
productCode = new FormControl('', []);
productGenre = new FormControl('', []);
endOfSale = new FormControl(false, []);

searchForm = this.formBuilder.group({
  productName: this.productName,
  productCode: this.productCode,
  productGenre: this.productGenre,
  endOfSale: this.endOfSale
});

/** Select item of genre */
genres: string[];

/** Material table's header */
displayColumns: string[] = [
  'no',
  'productName',
  'productCode',
  'productGenre',
  'productImage',
  'productSizeStandard',
  'productColor',
  'productUnitPrice',
  'productStockQuantity',
  'endOfSale'
];

/** Search result */
productSearchResponseDtos: ProductSearchResponseDto[];
resultsLength = 0;

/** Paginator */
@ViewChild(MatPaginator)
public paginator: MatPaginator;
initialPageSize = 50;
```

#### メソッド

下記を追加する

```
/**
 * on init
 */
ngOnInit(): void {
  this.setupLanguage();
}

/**
 * after view checked
 */
ngAfterViewChecked(): void {
  this.titleI18Service.setTitle(UrlConst.PATH_PRODUCT_LISTING);
}

/**
 * Clicks new button
 */
clickNewButton(): void {
  this.routingService.navigate(UrlConst.PATH_PRODUCT_REGISTERING_NEW);
}

/**
 * Clicks clear button
 */
clickClearButton(): void {
}

/**
 * Clicks search button
 */
clickSearchButton(): void {
}

/**
 * Clicks list row
 * @param productSearchResponseDto Product search response dto
 */
clickListRow(productSearchResponseDto: ProductSearchResponseDto): void {
}

/**
 * Unselects product genre
 */
unselectProductGenre(): void {
}
// --------------------------------------------------------------------------------
// private methods
// --------------------------------------------------------------------------------
private setupLanguage(): void {
  const lang = this.accountService.getUser().userLanguage;
  this.translateService.setDefaultLang(lang);
  this.translateService.use(lang);
}
```

クイックフィックスできない場合

```
import { ProductSearchResponseDto } from '../../models/dtos/responses/product-search-response-dto';
```
