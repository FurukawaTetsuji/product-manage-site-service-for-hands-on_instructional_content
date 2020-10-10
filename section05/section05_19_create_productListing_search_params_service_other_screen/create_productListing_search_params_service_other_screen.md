# ① 検索条件の保存と削除

## ① - 1 ヘッダーコンポーネント

### a. 「header.component.html」の編集

`<button mat-menu-item>`にクリックイベント(click)="clickSubmenu()"を追加する

```
<div class="toolBarMenu">
  <ng-container *ngFor="let item of menuListResponseDto">
    <button mat-button [matMenuTriggerFor]="appMenu" id="{{'menu-'+ item.menuCode}}" class="menu mainMenu">
      <div>{{ 'menu.' + item.menuCode | translate }}</div>
    </button>
    <mat-menu #appMenu="matMenu">
      <ng-container *ngFor="let subMenuCode of item.subMenuCodeList">
        <button mat-menu-item id="{{'subMenu-' + subMenuCode}}" class="menu subMenu"
          matTooltip="{{ 'menu.tooltip.subMenu.' + subMenuCode | translate }}" routerLink="/{{subMenuCode}}"
          (click)="clickSubmenu()">
          {{'subMenu.' + subMenuCode | translate }}
        </button>
      </ng-container>
    </mat-menu>
  </ng-container>
</div>
```

### b. 「header.component.ts」の編集

#### コンストラクタ

コンストラクタに下記を追加

```
private searchParamsService: SearchParamsService,
```

#### clickSubmenu

clickSubmenu メソッド を追加する
（clickSidenav の下）

```
/**
  * Clicks submenu
  */
clickSubmenu(): void {
  this.searchParamsService.removeProductListingSearchParamsDto();
}
```

#### signOut

signOut に searchParamsService.removeProductListingSearchParamsDto を追加

```
private signOut(): void {
  this.loadingService.startLoading();
  this.accountService.signOut().subscribe((res) => {
    this.searchParamsService.removeProductListingSearchParamsDto();
    this.loadingService.stopLoading();
    this.routingService.navigate(UrlConst.PATH_SIGN_IN);
  });
}
```

## ① - ２ サイドナビコンポーネント

### a. 「sidenav.component.ts」の編集

#### コンストラクタ

コンストラクタに下記を追加

```
private searchParamsService: SearchParamsService,
```

#### clickSubmenu

searchParamsService.removeProductListingSearchParamsDto を追加する

```
clickSubmenu(): void {
  this.searchParamsService.removeProductListingSearchParamsDto();
  this.sidenavClose.emit();
}
```
