# ① サインイン画面の作成

## ① - 1 準備

### a. html の準備

ひな形のレポジトリから「SignIn.html」の中身をコピーする  
https://github.com/FurukawaTetsuji/product-manage-site-service-for-hands-on_html-example

```
  <section class="sign-in-page-wrapper">
    <div class="sign-in-page">
      <div class="sign-in-title-wrapper">
        <p>EXAPMLE SITE</p>
      </div>
      <div class="sign-in-criteria-wrapper">
        <form>
          <input id="signin-user-account" type="text" placeholder="ユーザアカウント">
          <input id="signin-user-password" type="password" placeholder="パスワード">
        </form>
        <a href="ProductListingPage.html" class="button">サインイン</a>
      </div>
    </div>
  </section>
```

### b. 「style.css」の準備

ひな形のレポジトリで「style.css」からサインインページ以外の部分を「style.css」のファイルへコピーする

### c. 「sign-in-page.component.scss」の準備

ひな形のレポジトリで「style.css」からサインインページの部分を「sign-in-page.component.scss」のファイルへコピーする  
（下記がサインインページ部分）

```
/* --------------------------------------------------------------------------------
 * Sign in page.
 * -------------------------------------------------------------------------------- */
.sign-in-page-wrapper {
  width: 450px;
  height: 300px;
  margin: 50px auto 0;
  padding-top: 100px;
}

.sign-in-page {
  background-color: #ffffff;
  border: 1px solid rgb(236, 239, 241, 0.45);
}

.sign-in-title-wrapper {
  height: 30px;
  width: 440px;
  margin: 0 auto 0;
  padding: 0 5px;
  background-color: #1B435D;
}

.sign-in-title-wrapper p {
  line-height: 30px;
  color: #ffffff;
}

.sign-in-criteria-wrapper {
  width: 80%;
  margin: 20px auto 0;
  text-align: center;
}

.sign-in-criteria-wrapper input[type=text],
.sign-in-criteria-wrapper input[type=password] {
  border-width: 0;
  display: block;
  width: 20rem;
  margin: 50px auto 50px;
}

.sign-in-criteria-wrapper .button {
  display: block;
  padding: 10px 0;
  margin: 50px auto 50px;
  width: 200px;
  border-radius: 2px;
  background-color: #78BBE6;
  color: #ffffff;
  text-decoration: none;
  text-align: center;
  letter-spacing: 1px;
  font-size: 1.2rem;
}

.sign-in-criteria-wrapper .button:hover {
  opacity: 0.9;
}

/* .sign-in-criteria-wrapper .form-field {
  margin: 20px auto 0;
  width: 200px;
}

.sign-in-page-wrapper .flat-button {
  margin: 50px auto 50px;
  width: 200px;
  border-radius: 2px;
  background-color: #78BBE6;
  color: #ffffff;
  letter-spacing: 1px;
  font-size: 1.2rem;
  box-shadow: none;
}

.sign-in-page-wrapper .flat-button:hover {
  opacity: 0.8;
}

.sign-in-page-wrapper .flat-button:disabled {
  opacity: 0.4;
} */
```

### d. 起動して内容確認

ng s -o

## ① - 2 言語設定

### a. pages.module.ts の編集

imports: []に「HttpClientModule,NgxTranslateModule」を追加する
（HttpClientModule は NgxTranslateModule が json を読み込むのに使う）

### b. sign-in-page.component.ts の編集

TranslateService をコンストラクタに追加する

```
public translateService: TranslateService
```

### c. メソッド追加

下記 2 つの private メソッドを追加する。  
#setupLanguage()メソッドはブラウザに設定されているメインの言語を取得し TranslateService に設定する  
navigator.language はブラウザに設定されている言語を取得する  
navigator.language はブラウザ設定により「言語-ロケール」を返すことがある。  
#getLanguage()メソッドは navigator.language で「言語-ロケール」が返ってきたときに言語だけを取り出すメソッド

```
// --------------------------------------------------------------------------------
// private methods
// --------------------------------------------------------------------------------
private setupLanguage() {
  // Setups language using browser settings.
  this.translateService.setDefaultLang(this.getLanguage(navigator.language));
  this.translateService.use(this.getLanguage(navigator.language));
}

private getLanguage(language: string): string {
  console.log('SignInPageComponent #getLanguage() language:' + language);

  const CHAR_HYPHEN = '-';
  if (language.indexOf(CHAR_HYPHEN) > 0) {
    const splittedLanguage: string[] = language.split(CHAR_HYPHEN);
    console.log('SignInPageComponent #getLanguage() splittedLanguage[0]:' + splittedLanguage[0]);

    return splittedLanguage[0];
  }
  return language;
}
```

上記 2 つができた後は
ngOnInit で setupLanguage()メソッドを呼び出す。

```
/**
  * on init
  */
ngOnInit(): void {
  // Sets language from browser settings.
  this.setupLanguage();
}
```

### d. 起動して内容確認

ng s -o
