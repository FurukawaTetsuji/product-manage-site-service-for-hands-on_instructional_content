# ① 商品一覧画面の作成

## ① - 1 入力値を大文字にするディレクティブを追加する

### a. ngx-upper-case-directive を追加

入力値を大文字にするディレクティブを追加する。
Angular の UpperCasePipe からカスタムディレクティブを作ってもよいが、  
今回は ngx-upper-case-directive をインストールして使う。

#### Cli コマンド

作成コマンド

```
npm install ngx-upper-case-directive --save
```

#### 「pages.module.ts」の編集

imports: []に追加

```
  imports: [
    NgxUpperCaseDirectiveModule,
  ],
```

#### 「product-listing-page.component.html」の編集

商品コードの検索条件欄に　`upperCase`を追加する

```
<mat-form-field class="form-field">
  <input id="product-code" matInput type="text" formControlName="productCode" upperCase
    placeholder="{{ 'productListingPage.productCode' | translate }}" maxlength=20
    matTooltip="{{ 'tooltip.searchCriteria.prefixMatching' | translate }}">
</mat-form-field>
```

その他  
クイックフィックスが効かなかったとき

```
import { NgxUpperCaseDirectiveModule } from 'ngx-upper-case-directive';
```

参考
Angular Upper Case Directive  
https://www.npmjs.com/package/ngx-upper-case-directive
