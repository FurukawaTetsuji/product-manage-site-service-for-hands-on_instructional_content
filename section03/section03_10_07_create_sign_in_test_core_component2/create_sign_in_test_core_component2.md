# ① テストコードの作成

## ① - 1 Core のコンポーネント

### b. 「loading.component.spec.ts」 を編集する

xdescribe( を describe( に戻す

もともと書かれていたソースを describe でかこむ

```
describe('#constractor', () => {
  it('should create', () => {
    expect(component).toBeTruthy();
  });
});
```

describe('LoadingComponent', () => { の下に下記変数を追加する

```
let loadingService: LoadingService;
```

TestBed.configureTestingModule({ の下に追加

```
schemas: [NO_ERRORS_SCHEMA],
imports: [BrowserAnimationsModule],
```

compileComponents(); の下に追加

```
loadingService = TestBed.inject(LoadingService);
```

あらたにメソッドを追加

```
describe('#ngOnInit', () => {
  it('should not display loading spinner', () => {
    loadingService.stopLoading();
    fixture.detectChanges();
    const debugElement: DebugElement = fixture.debugElement.query(By.css('.loading-spinner'));
    expect(debugElement).toBeFalsy();
  });

  it('should display loading spinner', () => {
    loadingService.startLoading();
    fixture.detectChanges();
    const debugElement: DebugElement = fixture.debugElement.query(By.css('.loading-spinner'));
    expect(debugElement).toBeTruthy();
  });
});
```
