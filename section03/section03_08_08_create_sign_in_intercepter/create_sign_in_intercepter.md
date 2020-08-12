# ① インターセプターの追加

サインインのエラー時にダイアログが表示されないようにする為のインターセプター

## ① - 1 インターセプターを作成

### a. 「xhr-interceptor.ts」 を作成する

コマンド

```
ng g interceptor core/interceptors/xhr
```

メソッド追加

```
// Intercepts Http request and add XML Http request header to hide Basic auth requiring dialog.
intercept(request: HttpRequest<unknown>, next: HttpHandler): Observable<HttpEvent<unknown>> {
  const xhr = request.clone({
    headers: request.headers.set('X-Requested-With', 'XMLHttpRequest')
  });
  return next.handle(xhr);
}
```

### b. xhr-interceptor を組込む

「core.module.ts」を編集する

providers に HTTP_INTERCEPTORS を追加する

```
providers: [
  { provide: HTTP_INTERCEPTORS, useClass: XhrInterceptor, multi: true }
],
```

## ① - 2 起動して確認

コマンド

```
ng serve -o --proxy-config proxy.conf.json
```

## その他

スプリングチュートリアル

https://spring.io/guides/tutorials/spring-security-and-angular-js/#using-curl
