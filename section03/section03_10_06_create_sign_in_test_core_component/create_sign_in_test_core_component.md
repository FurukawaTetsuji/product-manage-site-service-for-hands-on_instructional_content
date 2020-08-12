# ① テストコードの作成

## ① - 1 Core のコンポーネント

ngx-translate のテスト用モジュールをインストールする  
コマンドは下記

```
npm install ngx-translate-testing --save-dev
```

### a. 「error-messaging.component.spec.ts」 を編集する

xdescribe( を describe( に戻す

もともと書かれていたソースを describe でかこむ

```
describe('#constractor', () => {
  it('should create', () => {
    expect(component).toBeTruthy();
  });
});
```

インポート文を追加する

```
import { TranslateTestingModule } from 'ngx-translate-testing';
```

describe の下に変数を追加

```
let errorMessagingServiceSpy: { clearMessageProperty: jasmine.Spy; getMessageProperty: jasmine.Spy };
```

beforeEach(async(() => { の下に spy を追加

```
errorMessagingServiceSpy = jasmine.createSpyObj('ErrorMessagingService', [
  'clearMessageProperty',
  'getMessageProperty'
]);
```

TestBed.configureTestingModule({ の下に追加

```
schemas: [NO_ERRORS_SCHEMA],
imports: [TranslateTestingModule.withTranslations({ ja: require('src/assets/i18n/ja.json') })],
providers: [{ provide: ErrorMessagingService, useValue: errorMessagingServiceSpy }],
```

beforeEach(async(() => {})); は、まとめると下記になる

```
beforeEach(async(() => {
  errorMessagingServiceSpy = jasmine.createSpyObj('ErrorMessagingService', [
    'clearMessageProperty',
    'getMessageProperty'
  ]);
  TestBed.configureTestingModule({
    schemas: [NO_ERRORS_SCHEMA],
    imports: [TranslateTestingModule.withTranslations({ ja: require('src/assets/i18n/ja.json') })],
    providers: [{ provide: ErrorMessagingService, useValue: errorMessagingServiceSpy }],
    declarations: [ErrorMessagingComponent]
  }).compileComponents();
}));
```

テストメソッドを新たに追加する

```
describe('#ngOnInit', () => {
  it('should init', () => {
    component.ngOnInit();
    expect(errorMessagingServiceSpy.clearMessageProperty.calls.count()).toBeGreaterThan(1);
  });

  it('should error dipslay message', () => {
    const errorMessageProperty = 'errMessage.http.badRequest';
    const expectedValue = '入力情報が正しくありません。';

    errorMessagingServiceSpy.getMessageProperty.and.returnValue(errorMessageProperty);
    fixture.detectChanges();

    const htmlParagraphElement: HTMLParagraphElement = fixture.debugElement.query(By.css('p')).nativeElement;
    expect(htmlParagraphElement.innerText).toEqual(expectedValue);
  });
});
```

## その他参考

日本語リファレンス　コンポーネントのテスト

https://angular.jp/guide/testing#%E3%82%B3%E3%83%B3%E3%83%9D%E3%83%BC%E3%83%8D%E3%83%B3%E3%83%88%E3%81%AEdom%E3%81%AE%E3%83%86%E3%82%B9%E3%83%88

ngx-translate-testing
https://www.npmjs.com/package/ngx-translate-testing
