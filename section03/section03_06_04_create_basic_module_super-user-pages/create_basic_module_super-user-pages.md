# ⑤ super-user-pages の作業

## ⑤ - 1 モジュール作成

### ● コマンド

↓ dummy-purchasing-page は遅延バインディングさせるのでコマンドが違（router 付き）

ng g module super-user-pages/dummy-purchasing-page --route dummy-purchasing-page --module app.module

# ⑥ 「app-routing.module」を編集する（routing の設定）

const routes: Routes = [];  
の中に下記を追加

```
  { path: '', redirectTo: UrlConst.SLASH + UrlConst.PATH_SIGN_IN, pathMatch: 'full' },
  { path: UrlConst.PATH_SIGN_IN, component: SignInPageComponent },
  { path: UrlConst.PATH_PRODUCT_LISTING, component: ProductListingPageComponent},
  { path: UrlConst.PATH_PRODUCT_REGISTERING_NEW, component: ProductRegisteringPageComponent},
  {
    path: UrlConst.PATH_PRODUCT_REGISTERING + UrlConst.SLASH + ':productCode',
    component: ProductRegisteringPageComponent
  },
  {
    path: UrlConst.PATH_PURCHASE_HISTORY_LISTING,
    component: PurchaseHistoryListingPageComponent
  },
  { path: UrlConst.PATH_STOCK_REGISTERING, component: StockRegisteringPageComponent},
  {
    path: UrlConst.PATH_DUMMY_PURCHASING,
    loadChildren: () =>
      import('./super-user-pages/dummy-purchasing-page/dummy-purchasing-page.module').then(
        m => m.DummyPurchasingPageModule
      )
  }
```

# 動作確認

起動コマンド

ng s -o

http://localhost:4200/
http://localhost:4200/sign-in
http://localhost:4200/product-listing
http://localhost:4200/product-registering/new
http://localhost:4200/purchase-history-listing
http://localhost:4200/stock-registering
http://localhost:4200/dummy-purchasing
