# ① テストコードの作成

## ① - 1 Core のインターセプター

### a. 「xhr-interceptor.spec.ts」 を編集する

xdescribe( を describe( に戻す

※クイックフィックスが効かないところがあるので下の import 文を追加

import { HttpClientTestingModule, HttpTestingController } from '@angular/common/http/testing';

describe の中を下記に書き換える。

```
let httpClient: HttpClient;
let httpTestingController: HttpTestingController;

beforeEach(() => {
  TestBed.configureTestingModule({
    imports: [HttpClientTestingModule],
    providers: [{ provide: HTTP_INTERCEPTORS, useClass: XhrInterceptor, multi: true }]
  });
  httpTestingController = TestBed.inject(HttpTestingController);
  httpClient = TestBed.inject(HttpClient);
});
```

テストメソッドを追加する

```
describe('#intercept', () => {
  const headerName = 'X-Requested-With';
  const expectedValue = 'XMLHttpRequest';

  it('should intercept get method', () => {
    httpClient.get('/test').subscribe((res) => expect(res).toBeTruthy());
    const req = httpTestingController.expectOne({ method: 'GET' });
    expect(req.request.headers.get(headerName)).toEqual(expectedValue);
  });

  it('should intercept post method', () => {
    httpClient.post('/test', null).subscribe((res) => expect(res).toBeTruthy());
    const req = httpTestingController.expectOne({ method: 'POST' });
    expect(req.request.headers.get(headerName)).toEqual(expectedValue);
  });
});
```

テストコマンド

```
ng test
```

カバレッジを取りたい時

angular.json

```
"test": {
  "options": {
    "codeCoverage": true
  }
}
```

## その他参考

testing

https://angular.jp/guide/testing#%E3%83%86%E3%82%B9%E3%83%88

Interceptor

https://alligator.io/angular/testing-http-interceptors/
