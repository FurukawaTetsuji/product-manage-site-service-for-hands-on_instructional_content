# ① サイドナビ作成

## ① - 1 Material について

https://material.angular.io/

list  
https://material.angular.io/components/list/overview

## ① - 2 Sidenav 作成

html ファイル

```
<mat-nav-list>
  <mat-list-item routerLink="{{initialDisplayScreenUrl}}" (click)="clickHome()"
    matTooltip="{{ 'menu.tooltip.home' | translate }}">
    <mat-icon>home</mat-icon>{{ 'menu.home' | translate }}
  </mat-list-item>

  <ng-container *ngFor=" let item of menuListResponseDto">
    <mat-list-item [matMenuTriggerFor]="appMenu">
      <mat-icon>arrow_drop_down</mat-icon>
      <a matline>{{ 'menu.' +item.menuCode | translate }}</a>
    </mat-list-item>
    <mat-menu #appMenu="matMenu">
      <ng-container *ngFor="let subMenuCode of item.subMenuCodeList">
        <button mat-menu-item (click)="clickSubmenu()"
          matTooltip="{{ 'menu.tooltip.subMenu.' + subMenuCode | translate }}"
          routerLink="/{{subMenuCode}}">{{ 'subMenu.' + subMenuCode | translate }}
        </button>
      </ng-container>
    </mat-menu>
  </ng-container>
</mat-nav-list>
```

ts ファイル

constructor の上に追加

```
// Clicks sidenav and throw event
@Output() sidenavClose = new EventEmitter();

// Initial display screen URL
initialDisplayScreenUrl: string = UrlConst.SLASH + UrlConst.PATH_PRODUCT_LISTING;

// Menu response data
menuListResponseDto: MenuListResponseDto[];
```

constructor に追加

```
constructor(
  private accountService: AccountService,
  public routingService: RoutingService
) {}
```

以下のメソッドを追加

```
/**
 * on init
 */
ngOnInit(): void {
  this.getMenu();
  // }
}

/**
 * Clicks submenu
 */
clickSubmenu(): void {
  this.sidenavClose.emit();
}

/**
 * Clicks home
 */
public clickHome(): void {
  this.sidenavClose.emit();
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

## ① - 3 app component の編集

「app.component.html」

サイドナビの箇所　　
sidenavClose イベントを受取ったときに sidenav クローズする

```
<app-sidenav (sidenavClose)="sidenav.close()" *ngIf="!this.isSignInPage()"></app-sidenav>
```

ヘッダーの箇所
sidenavToggle イベント（クリック）を受取ったときに sidenav を開く

```
<!-- Header -->
<app-header (sidenavToggle)="sidenav.toggle()" *ngIf="!this.isSignInPage()"></app-header>
```
