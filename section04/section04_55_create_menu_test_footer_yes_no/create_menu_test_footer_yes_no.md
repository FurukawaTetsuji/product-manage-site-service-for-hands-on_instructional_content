# ① テストコードの作成

## ① - 1 その他のコンポーネント

### a. 「footer.component.spec.ts」 を編集する

xdescribe( を describe( に戻す

もともと書かれていたソースを describe でかこむ

```
describe('#constractor', () => {
  it('should create', () => {
    expect(component).toBeTruthy();
  });
});
```

TestBed.configureTestingModule({ の下に追加

```
schemas: [NO_ERRORS_SCHEMA],
```

### b. 「yes-no-dialog.component.spec.ts」 を編集する

もともと書かれていたソースを describe でかこむ

```
describe('#constractor', () => {
  it('should create', () => {
    expect(component).toBeTruthy();
  });
});
```

TestBed.configureTestingModule({ の下に追加

```
schemas: [NO_ERRORS_SCHEMA],
imports: [MatDialogModule, TranslateTestingModule.withTranslations({ ja: require('src/assets/i18n/ja.json') })],
providers: [{ provide: MAT_DIALOG_DATA, useValue: {} }],
```
