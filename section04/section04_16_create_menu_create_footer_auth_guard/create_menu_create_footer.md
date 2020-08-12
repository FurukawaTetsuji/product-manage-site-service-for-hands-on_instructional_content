# ① フッター作成

## ① - 1 footer component を編集

html ファイル

```
<footer class="footer-wrapper">
  <p>©copy right</p>
</footer>
```

# ② AuthGuard 設定

## ② - 1 AuthGuard を追加

Angular 日本語リファレンス  
https://angular.jp/guide/router#canactivate-requiring-authentication

コマンド

```
ng g guard pages/guards/auth
```

コンストラクタ全体を追加

```
constructor(private accountService: AccountService, private routingService: RoutingService) {}
```

メソッド追加

```
  /**
   * Determines whether activate can
   * @param next Activated route snapshot
   * @param state Router state snapshot
   * @returns Whether activate can
   */
  canActivate(
    next: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ): Observable<boolean | UrlTree> | Promise<boolean | UrlTree> | boolean | UrlTree {
    return this.accountService.getAvailablePages().pipe(
      map((availablePages) => {
        if (availablePages === null) {
          return this.cantActivate();
        }
        const filteredMenu = availablePages.filter((availablePage) =>
          state.url.toString().startsWith(UrlConst.SLASH + availablePage)
        );
        if (filteredMenu.length === 0) {
          return this.cantActivate();
        }
        return true;
      })
    );
  }

private cantActivate(): boolean {
  this.routingService.navigate(UrlConst.PATH_SIGN_IN);
  return false;
}
```

## ② - 2 「app-routing.module.ts」 に AuthGuard を追加

下記を追加する

```
, canActivate: [AuthGuard]
```

追加後

```
const routes: Routes = [
  { path: '', redirectTo: UrlConst.SLASH + UrlConst.PATH_SIGN_IN, pathMatch: 'full' },
  { path: UrlConst.PATH_SIGN_IN, component: SignInPageComponent },
  { path: UrlConst.PATH_PRODUCT_LISTING, component: ProductListingPageComponent, canActivate: [AuthGuard] },
  { path: UrlConst.PATH_PRODUCT_REGISTERING_NEW, component: ProductRegisteringPageComponent, canActivate: [AuthGuard] },
  {
    path: UrlConst.PATH_PRODUCT_REGISTERING + UrlConst.SLASH + ':productCode',
    component: ProductRegisteringPageComponent,
    canActivate: [AuthGuard]
  },
  {
    path: UrlConst.PATH_PURCHASE_HISTORY_LISTING,
    component: PurchaseHistoryListingPageComponent,
    canActivate: [AuthGuard]
  },
  { path: UrlConst.PATH_STOCK_REGISTERING, component: StockRegisteringPageComponent, canActivate: [AuthGuard] },
  {
    path: UrlConst.PATH_DUMMY_PURCHASING,
    loadChildren: () =>
      import('./super-user-pages/dummy-purchasing-page/dummy-purchasing-page.module').then(
        (m) => m.DummyPurchasingPageModule
      ),
    canActivate: [AuthGuard]
  }
];
```
