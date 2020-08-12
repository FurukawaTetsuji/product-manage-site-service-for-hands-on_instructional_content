# ① URL によってデザインを切り替え

以下ルートまたはサインインのパスの時に
SidenavComponent、HeaderComponent と FooterComponent を表示しないようにする。

## ① - 1「RoutingService」を追加

ルーティングを使いたいので RoutingService を追加作成する。

```
ng g service core/services/routing
```

「routing.service.ts」に下記のようにコンストラクタと Router を public で追加する。

```
constructor(public router: Router) {}
```

そして public メソッド navigate() を追加する。
（引数で指定したパスに移動するメソッド）

```
/**
  * Navigates to path.
  * @param path path to pages.
  */
public navigate(path: string) {
  // navigates to path.
  this.router.navigate([UrlConst.SLASH + path]);
}
```

## ① - 2「app.component.ts」を修整

コンストラクタと上記で作った RoutingService を public で追加する。
現在のルーターの url がサインインページかどうかを判定するメソッドを追加する。

```
constructor(private routingService: RoutingService) {}
```

```
/**
  * Determines whether sign in page is displayed
  * @returns true if sign in page
  */
public isSignInPage(): boolean {
  console.log('AppComponent #isSignInPage() this.routingService.router.url:' + this.routingService.router.url);

  if (UrlConst.SLASH === this.routingService.router.url) {
    return true;
  }
  if (UrlConst.SLASH + UrlConst.PATH_SIGN_IN === this.routingService.router.url) {
    return true;
  }
  return false;
}
```

## ① - 3「app.component.html」にルーティング関連を追記

```
<app-sidenav *ngIf="!this.isSignInPage()"></app-sidenav>
```

```
<app-header *ngIf="!this.isSignInPage()"></app-header>
```

```
<app-footer *ngIf="!this.isSignInPage()"></app-footer>
```

# ② その他 log について

本番では #console.log() が出力されないよう設定しておきます。

「main.ts」の下記に

```
if (environment.production) {
  enableProdMode();
}
```

こちらを追加します（空のファンクションで上書きします）

```
// Do not output console log
window.console.log = () => {};
```

# ③ 確認 URL の一覧

http://localhost:4200/

http://localhost:4200/sign-in

http://localhost:4200/product-listing

http://localhost:4200/product-registering/new

http://localhost:4200/purchase-history-listing

http://localhost:4200/stock-registering

http://localhost:4200/dummy-purchasing
