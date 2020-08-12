# ① テストコードの作成

## ① - 1 Shared のサービス

### a. 「title-i18.service.spec.ts」 を編集する

xdescribe( を describe( に戻す

もともと書かれていたソースを describe でかこむ

```
describe('#constractor', () => {
  it('should be created', () => {
    expect(service).toBeTruthy();
  });
});
```

TestBed.configureTestingModule({ の下に追加

```
imports: [TranslateTestingModule.withTranslations({ ja: require('src/assets/i18n/ja.json') })]
```

テストメソッドを新たに追加する

```
describe('#setTitle', () => {
  it('should set title', () => {
    const screenName = 'sign-in';
    const expectedTitleSystem = '【Example Site】';
    const expectedTitleSub = 'ログイン';

    service.setTitle(screenName);
    expect(service.title.getTitle()).toEqual(expectedTitleSystem + expectedTitleSub);
  });
});
```
