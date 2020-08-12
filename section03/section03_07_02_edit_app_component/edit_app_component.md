# ①「app.component.html」の修整

「app.component.html」を下記で書き換える

```
<!-- Sidenav container -->
<mat-sidenav-container>

  <!-- Side navigation -->
  <mat-sidenav #sidenav mode="side" opened>
    <app-sidenav></app-sidenav>
  </mat-sidenav>

  <!-- Main contents -->
  <mat-sidenav-content>

    <!-- Loading Spinner -->
    <app-loading></app-loading>

    <!-- Wrapper of body -->
    <div class="body-wrapper">
      <!-- Header -->
      <app-header></app-header>

      <!-- Router -->
      <router-outlet></router-outlet>

      <!-- Footer -->
      <app-footer></app-footer>

    </div>

  </mat-sidenav-content>

</mat-sidenav-container>
```

## 「core.module.ts」を修整

app-loading が不明（見れない）とエラーになるので  
core.module で これらを exports する

core.module.ts」の`@NgModule({})`に下記を追加

```
exports: [LoadingComponent, ErrorMessagingComponent]
```

## 「shared.module.ts」を修整

app-header や app-footer が不明（見れない）とエラーになるので  
shared.module で これらを exports する

「shared.module.ts」の`@NgModule({})`に下記を追加

```
exports: [SidenavComponent, HeaderComponent, FooterComponent]
```

## 「pages.module.ts」を修整

「pages.module.ts」も同様に exports を追加する

````
exports: [
  SignInPageComponent,
  ProductListingPageComponent,
  ProductRegisteringPageComponent,
  StockRegisteringPageComponent,
  PurchaseHistoryListingPageComponent
]
```

## URL の一覧

http://localhost:4200/
http://localhost:4200/sign-in
http://localhost:4200/product-listing
http://localhost:4200/product-registering/new
http://localhost:4200/purchase-history-listing
http://localhost:4200/stock-registering
http://localhost:4200/dummy-purchasing
````
