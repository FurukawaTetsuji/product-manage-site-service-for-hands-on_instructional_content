# ① テストコードの作成

## ① - 1 SearchParams サービス

### a. 「search-params.service.spec.ts」 を編集する

もともと書かれていたソースを describe でかこむ
expect(service).toBeTruthy();の上下にコードを追加する

```
  describe('#constractor', () => {
    it('should be created', () => {
      spyOn(translateService.onLangChange, 'emit').and.callThrough();
      expect(service).toBeTruthy();
      translateService.onLangChange.emit();
      expect(translateService.onLangChange.emit).toHaveBeenCalled();
    });
  });
```

let の下に下記を追加

```
let translateService: TranslateService;
```

TestBed.configureTestingModule({　の下に下記を追加

```
schemas: [NO_ERRORS_SCHEMA],
imports: [TranslateTestingModule.withTranslations({ ja: require('src/assets/i18n/ja.json') })]
```

service = TestBed.inject(MatPaginatorI18nService); の下に下記を追加

```
translateService = TestBed.inject(TranslateService);
```

beforeEach(() => {}); は、まとめると下記になる

```
beforeEach(() => {
  TestBed.configureTestingModule({
    schemas: [NO_ERRORS_SCHEMA],
    imports: [TranslateTestingModule.withTranslations({ ja: require('src/assets/i18n/ja.json') })]
  });
  service = TestBed.inject(MatPaginatorI18nService);
  translateService = TestBed.inject(TranslateService);
});
```

テストメソッドを新たに追加する

```
describe('#constractor', () => {
  it('should be created', () => {
    spyOn(translateService.onLangChange, 'emit').and.callThrough();
    expect(service).toBeTruthy();
    translateService.onLangChange.emit();
    expect(translateService.onLangChange.emit).toHaveBeenCalled();
  });
});

describe('#getRangeLabel', () => {
  const parameters = [
    {
      page: 0,
      pageSize: 10,
      length: 0,
      expectedValue: '0 / 0',
      no: 1
    },
    {
      page: 0,
      pageSize: 10,
      length: 1,
      expectedValue: '1 – 1 / 1',
      no: 2
    },
    {
      page: 1,
      pageSize: 10,
      length: 11,
      expectedValue: '11 – 11 / 11',
      no: 3
    },
    {
      page: 2,
      pageSize: 10,
      length: 21,
      expectedValue: '21 – 21 / 21',
      no: 4
    },
    {
      page: 0,
      pageSize: 50,
      length: 1,
      expectedValue: '1 – 1 / 1',
      no: 5
    },
    {
      page: 1,
      pageSize: 10,
      length: 20,
      expectedValue: '11 – 20 / 20',
      no: 6
    },
    {
      page: 1,
      pageSize: 50,
      length: 100,
      expectedValue: '51 – 100 / 100',
      no: 7
    }
  ];

  parameters.forEach((parameter) => {
    it(
      'should be displayed correctly as ' +
        parameter.expectedValue +
        ', When the page is ' +
        parameter.page +
        ' and pageSize is ' +
        parameter.pageSize +
        ' and length is ' +
        parameter.length +
        ' (no = ' +
        parameter.no +
        ')',
      () => {
        expect(service.getRangeLabel(parameter.page, parameter.pageSize, parameter.length)).toBe(
          parameter.expectedValue
        );
      }
    );
  });
});

describe('#getAndInitTranslations', () => {
  const privateMethodName = 'setupLabels';

  it('should set Japanese on the itemsPerPageLabel', () => {
    service[privateMethodName]();
    expect(service.itemsPerPageLabel).toBe('件数：');
  });
  it('should set Japanese on the nextPageLabel', () => {
    service[privateMethodName]();
    expect(service.nextPageLabel).toBe('次ページへ');
  });
  it('should set Japanese on the previousPageLabel', () => {
    service[privateMethodName]();
    expect(service.previousPageLabel).toBe('前ページへ');
  });
});
```
