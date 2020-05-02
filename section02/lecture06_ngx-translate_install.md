# ngx-translate のインストール

## ngx-translate のサイト

https://github.com/ngx-translate/core

## インストールコマンド

```
npm install @ngx-translate/core --save
npm install @ngx-translate/http-loader --save
```

## module を作る

```
ng g module ngx-translate
```

## HttpLoaderFactory を追加する

```
import { TranslateHttpLoader } from '@ngx-translate/http-loader';

export function HttpLoaderFactory(http: HttpClient) {
return new TranslateHttpLoader(http, './assets/i18n/', '.json');
}
```

## 使用する予定のロケールを取り込む

プログラムで動的にロケールを変えるので多めに登録する（今回使用するのは日本語と英語のみ）
英語はデフォルトで読み込まれるので省略する。

```
import localeDe from '@angular/common/locales/de';
import localeFr from '@angular/common/locales/fr';
import localeJa from '@angular/common/locales/ja';

registerLocaleData(localeJa);
registerLocaleData(localeFr);
registerLocaleData(localeDe);
```

providers: []に

```
{ provide: LOCALE_ID, useValue: 'ja-JP' },
{ provide: LOCALE_ID, useValue: 'en-US' },
{ provide: LOCALE_ID, useValue: 'fr-FR' },
{ provide: LOCALE_ID, useValue: 'de-DE' }
```

を追加する。  
（providers は オブジェクトを DI するときの変数の値を指定したり、どのクラスを実際に使うかを指定できる）

```
exports:[TranslateModule]
```

を追加する。

## app.module に追加する

NgxTranslateModule を imports に追加する

## json を作成する

./assets/に i18n フォルダを追加、
ja.json、en.json ファイルを追加する。

# その他、参考サイト

https://blog.angulartraining.com/how-to-internationalize-i18n-your-angular-application-tutorial-dee2c6984bc1
