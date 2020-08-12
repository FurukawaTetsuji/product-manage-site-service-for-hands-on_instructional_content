# ① ヘッダー作成

## ① - 1 Material について

https://material.angular.io/

toolbar  
https://material.angular.io/components/toolbar/examples

menu  
https://material.angular.io/components/menu/examples

## ① - 2 Header コンポーネントの編集

html ファイル

```
<header class="header-wrapper">
  <mat-toolbar class="toolbar">
    <mat-toolbar-row class="toolbar-row">
      <button class="btnSidenav" mat-icon-button (click)="clickSidenav()">
        <mat-icon>menu</mat-icon>
      </button>
      <button mat-button routerLink="{{initialDisplayScreenUrl}}" class="menu btnSystemName"
        matTooltip="{{ 'menu.tooltip.home' | translate }}">
        <div>EXAPMLE SITE</div>
      </button>

      <div class="toolBarMenu">
        <ng-container *ngFor="let item of menuListResponseDto">
          <button mat-button [matMenuTriggerFor]="appMenu" id="{{'menu-'+ item.menuCode}}" class="menu mainMenu">
            <div>{{ 'menu.' + item.menuCode | translate }}</div>
          </button>
          <mat-menu #appMenu="matMenu">
            <ng-container *ngFor="let subMenuCode of item.subMenuCodeList">
              <button mat-menu-item id="{{'subMenu-' + subMenuCode}}" class="menu subMenu"
                matTooltip="{{ 'menu.tooltip.subMenu.' + subMenuCode | translate }}" routerLink="/{{subMenuCode}}">
                {{'subMenu.' + subMenuCode | translate }}
              </button>
            </ng-container>
          </mat-menu>
        </ng-container>
      </div>

      <button mat-button id="sign-out-button" class="menu btnSignOut"
        matTooltip="{{ 'menu.tooltip.signOut' | translate }}" (click)="clickSignOut()">
        <mat-icon>exit_to_app</mat-icon>
      </button>
    </mat-toolbar-row>
  </mat-toolbar>
</header>
```

css ファイル

「style.css」のヘッダー部分を使う  
切り取ってヘッダーコンポーネントの css に移動する  
上の部分は消して、コメントアウトされている部分を使用する  
最後の 3 行はもう一度コメントアウト

```
/* --------------------------------------------------------------------------------
 * Header.
 * -------------------------------------------------------------------------------- */
.header-wrapper {
  position: fixed;
  z-index: 100;
  top: 0;
  left: 0;
  margin: 0;
  height: 50px;
  width: 100%;
  background-color: #1B435D;
}

.header-wrapper:hover {
  opacity: 0.9;
}

.header-wrapper ul {
  color: #ffffff;
  font-size: 1.4rem;
  list-style-type: none;
}

.header-wrapper ul li {
  padding: 15px 30px 15px 10px;
  display: inline-block;
}

.header-wrapper ul li:hover {
  opacity: 0.9;
  cursor: pointer;
}

/* .header-wrapper .toolbar {
  position: fixed;
  z-index: 10;
  top: 0;
  left: 0;
  margin: 0;
  min-height: 50px;
  max-height: 50px;
  background-color: #1B435D;
  color: #ffffff;
  font-size: 1.4rem;
}

.header-wrapper .toolbar-row {
  padding: 0 0;
}

.header-wrapper .menu {
  height: 50px;
}

.header-wrapper .menu:hover {
  opacity: 0.7;
}

.header-wrapper .menu.btnSystemName {
  padding-left: 10px;
}

.header-wrapper .menu.btnSignOut {
  text-align: right;
  padding-right: 10px;
  margin-right: 0;
  right: 0;
  position: absolute;
  height: 50px;
}

.header-wrapper .btnSidenav {
  display: none;
} */
```

「style.css」のマテリアル部分のコメントアウトを外す

```
/* --------------------------------------------------------------------------------
 * Other Material settings.
 * -------------------------------------------------------------------------------- */
/* .mat-dialog-container {
  background-color: #ECEFF1;
  box-shadow: none;
  padding: 0 !important;
}

.mat-checkbox.mat-accent.mat-checkbox-checked .mat-checkbox-background {
  background-color: #F99F48;
} */
```

ts ファイル

constructor の上に追加

```
// Clicks sidenav and throw event
@Output() sidenavToggle = new EventEmitter();

// Initial display screen URL
initialDisplayScreenUrl: string = UrlConst.SLASH + UrlConst.PATH_PRODUCT_LISTING;

// Menu response data
menuListResponseDto: MenuListResponseDto[];
```

constructor に追加

```
constructor(
  private accountService: AccountService,
  private loadingService: LoadingService,
  private translateService: TranslateService,
  public routingService: RoutingService
) {}
```

```
/**
 * on init
 */
ngOnInit(): void {
  this.getMenu();
}

/**
 * Clicks toggle sidenav
 */
clickSidenav(): void {
  this.sidenavToggle.emit();
}

/**
 * Clicks sign out
 */
clickSignOut(): void {
}

// --------------------------------------------------------------------------------
// private methods
// --------------------------------------------------------------------------------
private getMenu(): void {
  this.accountService.getMenu().subscribe((menuListResponseDto) => {
    this.menuListResponseDto = menuListResponseDto;
  });
}
```
