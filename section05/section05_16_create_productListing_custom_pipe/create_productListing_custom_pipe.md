# ① 数量と金額の書式設定

## ① - 1 カスタムパイプを作成

### a. パイプで使用するクラスを追加（正規表現の文字列クラス）

「RegexConst」の作成

#### Cli コマンド

作成コマンド

```
ng g class core/constants/regex-const
```

#### 「regex-const.ts」の編集

クラスの内容は下記

```
export class RegexConst {
  static readonly HalfWidthBlank = ' ';
  static readonly HalfWidthComma = ',';
  static readonly HalfWidthPeriod = '.';

  static readonly SINGLE_BYTE_ALPHANUMERIC = '^[0-9a-zA-Z]+$';
  static readonly SINGLE_BYTE_NUMERIC_COMMA_PERIOD_SPACE = '^[0-9,. ]+$';
  }
```

### b. パイプで使用するクラスを追加（簡単なヘルパークラス）

「parse-helper」を作成

#### Cli コマンド

作成コマンド

```
ng g class core/utilities/parse-helper
```

#### 「parse-helper.ts」の編集

```
export class ParseHelper {
  /**
   * Parses
   * @param value value with thousand separator
   * @param locale locale
   * @returns parsed value
   */
  static parse(value: any, locale: string): string {
    let thousandsSeparator = new DecimalPipe(locale).transform('1111', '', locale).charAt(1);
    thousandsSeparator = ParseHelper.escapePeriod(thousandsSeparator);

    return (
      value
        .toString()
        // Remove actual thousand separator
        .replace(new RegExp(thousandsSeparator, 'g'), '')
        // Changes to a period if the decimal point is a comma
        .replace(new RegExp(RegexConst.HalfWidthComma, 'g'), RegexConst.HalfWidthPeriod)
    );
  }

  private static escapePeriod(thousandSeparator: string): string {
    if (thousandSeparator === '.') {
      // Escapes if thousand separator is Period
      return '\\' + thousandSeparator;
    }
    return thousandSeparator;
  }
}
```

### c. 数量の書式を設定するカスタムパイプ

「formatted-number.pipe」の作成

#### Cli コマンド

作成コマンド

```
ng g pipe core/pipes/formatted-number
```

#### 「formatted-number.pipe.ts」の編集

クラスの内容は下記

```
export class FormattedNumberPipe implements PipeTransform {
  transform(value: any, locale: string): string {
    const regexp = new RegExp(RegexConst.SINGLE_BYTE_NUMERIC_COMMA_PERIOD_SPACE);

    // If the format is not proper, returnes the character string without conversion.
    if (!value.toString().match(regexp)) {
      return value;
    }

    // Removes blanks and commas before converting
    const blankCommaRemovedValue = value
      .toString()
      // First converts with half-width blank
      .replace(new RegExp(RegexConst.HalfWidthBlank, 'g'), '')
      .replace(new RegExp(RegexConst.HalfWidthComma, 'g'), '');

    return new DecimalPipe(locale).transform(blankCommaRemovedValue, '1.0-0', locale);
  }

  parse(value: any, locale: string): string {
    return ParseHelper.parse(value.toString(), locale);
    // If the Parse Helper class is difficult, use the comment line below instead.
    // return value.toString().replace(/,/g, '');
  }
}
```

その他、クイックフィックスが効かない場合

```
import { RegexConst } from '../constants/regex-const';
import { ParseHelper } from '../utilities/parse-helper';
```

### d. 金額の書式を設定するパイプ

「formatted-currency.pipe」の作成

#### Cli コマンド

作成コマンド

```
ng g pipe core/pipes/formatted-currency
```

#### 「formatted-currency.pipe.ts」の編集

クラスの内容は下記

```
export class FormattedCurrencyPipe implements PipeTransform {
  transform(value: any, locale: string, currency: string): string {
    const regexp = new RegExp(RegexConst.SINGLE_BYTE_NUMERIC_COMMA_PERIOD_SPACE);

    // If the format is not proper, returnes the character string without conversion.
    if (!value.toString().match(regexp)) {
      return value;
    }

    // Removes blanks and commas before converting
    const blankCommaRemovedValue = value
      .toString()
      // First converts with half-width blank
      .replace(new RegExp(RegexConst.HalfWidthBlank, 'g'), '')
      .replace(new RegExp(RegexConst.HalfWidthComma, 'g'), '');

    return new CurrencyPipe(locale).transform(blankCommaRemovedValue, currency, '', '', locale);
  }

  parse(value: any, locale: string): string {
    return ParseHelper.parse(value.toString(), locale);
    // If the Parse Helper class is difficult, use the comment line below instead.
    // return value.toString().replace(/,/g, '');
  }
}
```

その他、クイックフィックスが効かない場合

```
import { RegexConst } from '../constants/regex-const';
import { ParseHelper } from '../utilities/parse-helper';
```

### e. core module にパイプ組込み

#### 「core.module.ts」の編集

exports: []に下記を追加する

```
, FormattedNumberPipe, FormattedCurrencyPipe
```

### e. 商品一覧にパイプ組込み

#### 「product-listing-page.component.html」の編集

検索結果の一覧部分の「productUnitPrice」と「productStockQuantity」にカスタムパイプを追加する

```
<ng-container matColumnDef="productUnitPrice">
  <th class="width-product-unit-price" mat-header-cell *matHeaderCellDef>
    {{ 'productListingPage.productUnitPrice' | translate }}</th>
  <td class="width-product-unit-price" mat-cell *matCellDef="let element">
    {{element.productUnitPrice | formattedCurrency:locale:currency }}</td>
</ng-container>
<ng-container matColumnDef="productStockQuantity">
  <th class="width-product-stock" mat-header-cell *matHeaderCellDef>
    {{ 'productListingPage.productStockQuantity' | translate }}</th>
  <td class="width-product-stock" mat-cell *matCellDef="let element">
    {{element.productStockQuantity | formattedNumber:locale }} </td>
</ng-container>
```

#### 「product-listing-page.component.ts」の編集

locale、currency を追加（searchForm の下近辺）

```
/** Locale, Currency */
locale: string = this.accountService.getUser().userLocale;
currency: string = this.accountService.getUser().userCurrency;
```

## 補足

海外の数字の区切り方サンプル

| 地域                   | 数値           |
| ---------------------- | -------------- |
| 日本                   | 123,456,789.00 |
| アメリカ               | 123,456,789.00 |
| イギリス               | 123,456,789.00 |
| ドイツ                 | 123.456.789,00 |
| フランス               | 123 456 789,00 |
| 韓国                   | 123,456,789.00 |
| 中国                   | 123,456,789.00 |
| アルゼンチン           | 123.456.789,00 |
| イタリア               | 123.456.789,00 |
| インドネシア           | 123.456.789,00 |
| オランダ               | 123.456.789,00 |
| オーストラリア         | 123,456,789.00 |
| オーストリア           | 123.456.789,00 |
| ギリシャ               | 123.456.789,00 |
| スイス                 | 123'456'789.00 |
| スウェーデン           | 123 456 789,00 |
| スペイン               | 123.456.789,00 |
| デンマーク             | 123.456.789,00 |
| トルコ                 | 123.456.789,00 |
| ブラジル               | 123.456.789,00 |
| ボスニアヘルツェゴビナ | 123.456.789,00 |
| メキシコ               | 123,456,789.00 |
| ロシア                 | 123 456 789,00 |

## 参考サイト

Angular 日本語リファレンス

https://angular.jp/guide/pipes#marking-a-class-as-a-pipe
