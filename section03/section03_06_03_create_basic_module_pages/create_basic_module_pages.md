# ④ pages module の作業

## ④ - 1 モジュール作成

### ● コマンド

ng g module pages --module app.module

## ④ - 2 component 作成

下記 5 つのコンポーネントを作成します

1. sign-in-page
1. product-listing-page
1. product-registering-page
1. stock-registering-page
1. purchase-history-listing-page

### ● コマンド

ng g component pages/components/sign-in-page

ng g component pages/components/product-listing-page

ng g component pages/components/product-registering-page

ng g component pages/components/stock-registering-page

ng g component pages/components/purchase-history-listing-page

## ④ - 3 servie 作成

1. account.service

### ● コマンド

ng g service pages/services/account

## ④ - 4 定数作成

下記の定数をあつかうクラスを作ります

1. app-const
1. url-const
1. api-const

### ● コマンド

ng g class pages/constants/app-const

ng g class pages/constants/url-const

ng g class pages/constants/api-const

### ● 手作業

#### app-const に下記を追加

```
// Session Strage keys
static readonly STRAGE_KEY_USER = 'USER';
```

#### url-const に下記を追加

```
static readonly SLASH = '/';
static readonly PATH_SIGN_IN = 'sign-in';
static readonly PATH_MENU = 'menu';
static readonly PATH_PRODUCT_LISTING = 'product-listing';
static readonly PATH_PRODUCT_REGISTERING = 'product-registering';
static readonly PATH_PRODUCT_REGISTERING_NEW = 'product-registering/new';
static readonly PATH_PURCHASE_HISTORY_LISTING = 'purchase-history-listing';
static readonly PATH_STOCK_REGISTERING = 'stock-registering';
static readonly PATH_DUMMY_PURCHASING = 'dummy-purchasing';
```

#### api-const に下記を追加

```
static readonly PATH_API_ROOT = '/api/';

static readonly PATH_SIGN_IN = 'sign-in/v1';
static readonly PATH_SIGN_OUT = 'sign-out/v1';
static readonly PATH_PRODUCT_SEARCH = 'product-search/v1';
static readonly PATH_PRODUCT = 'product/v1';
static readonly PATH_PURCHASE_HISTORY_SEARCH = 'product-purchase-history-search/v1';
static readonly PATH_STOCK = 'product-stock/v1';
static readonly PATH_PURCHASE = 'product-purchase/v1';
static readonly PATH_MENU = 'menu/v1';
static readonly PATH_AVAILABLE_PAGES = 'available-pages/v1';
static readonly PATH_GENRE = 'product-genre/v1';
```
