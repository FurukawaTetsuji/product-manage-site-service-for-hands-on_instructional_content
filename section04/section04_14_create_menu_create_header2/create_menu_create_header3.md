# ① Header にサインアウトを追加

マテリアルのダイアログについて

Material dialog  
https://material.angular.io/components/dialog/overview

## ① - 1 yes-no-dialog-data を作成

ダイアログに渡す情報のインターフェースを作る

ng g interface core/models/yes-no-dialog-data

内部に持つ変数

```
title: string;
message: string;
captionYes: string;
captionNo: string;
```

## ① - 2 yes-no-dialog を作成

コマンド

```
ng g component core/components/yes-no-dialog
```

html ファイル

```
<h1 mat-dialog-title>{{data.title | translate}}</h1>
<p>{{data.message | translate}}</p>
<div mat-dialog-actions>
  <button mat-flat-button id="yesNoDialog_button_no" class="flat-button" mat-dialog-close>{{data.captionNo | translate}}
  </button>
  <button mat-flat-button id="yesNoDialog_button_yes" class="flat-button"
    [mat-dialog-close]="true">{{data.captionYes | translate}}
  </button>
</div>
```

css ファイル

```
.mat-dialog-title {
  background-color: rgba(0, 0, 0, 0.12);
}

.mat-dialog-actions {
  padding-left: 10px;
  padding-top: 30px;
}

.mat-dialog-actions .flat-button {
  width: 140px;
  border-radius: 2px;
  background-color: #78BBE6;
  color: #ffffff;
  letter-spacing: 1px;
  font-size: 1.2rem;
  margin-left: 5px;
}

.mat-dialog-actions .flat-button:hover {
  opacity: 0.8;
}

h1 {
  font-size: 1.2rem;
  padding-left: 15px;
}

p {
  font-size: 1rem;
  padding-top: 15px;
  padding-left: 15px;
}
```

ts ファイル　　
constructor

```
constructor(public translateService: TranslateService, @Inject(MAT_DIALOG_DATA) public data: YesNoDialogData) {}
```

## ① - 3 app-const.ts に ダイアログの高さ、幅を追加

「app-const.ts」に追加

```
// Dialog size
static readonly YES_NO_DIALOG_HEIGHT = '195px';
static readonly YES_NO_DIALOG_WIDTH = '315px';
```

## ② - 1 header に signout を追加

ts ファイル

constructor に追加

```
constructor(
  private matDialog: MatDialog,
) {}
```

メソッド追加

```
/**
 * Clicks sign out
 */
clickSignOut(): void {
  const dialogData: YesNoDialogData = {
    title: this.translateService.instant('menu.saveYesNoDialog.title'),
    message: this.translateService.instant('menu.saveYesNoDialog.message'),
    captionNo: this.translateService.instant('menu.saveYesNoDialog.captionNo'),
    captionYes: this.translateService.instant('menu.saveYesNoDialog.captionYes')
  };

  const dialogRef = this.matDialog.open(YesNoDialogComponent, {
    height: AppConst.YES_NO_DIALOG_HEIGHT,
    width: AppConst.YES_NO_DIALOG_WIDTH,
    data: dialogData
  });

  dialogRef.afterClosed().subscribe((result) => {
    if (result) {
      this.signOut();
    }
  });
}
```

プライベートメソッドを追加

```
private signOut(): void {
  this.loadingService.startLoading();
  this.accountService.signOut().subscribe((res) => {
    this.loadingService.stopLoading();
    this.routingService.navigate(UrlConst.PATH_SIGN_IN);
  });
}
```

## ② - 2 core.module に imports 追加

「core.module.ts」を編集する

下記を追加する

```
entryComponents: [YesNoDialogComponent],
```

下記も追加する

```
exports: [YesNoDialogComponent],
```

まとめると下記の状態

```
@NgModule({
  declarations: [LoadingComponent, ErrorMessagingComponent, YesNoDialogComponent],
  imports: [CommonModule, MaterialModule, NgxTranslateModule],
  providers: [{ provide: HTTP_INTERCEPTORS, useClass: XhrInterceptor, multi: true }],
  entryComponents: [YesNoDialogComponent],
  exports: [LoadingComponent, ErrorMessagingComponent, YesNoDialogComponent]
})
```

## ② - 3 shared.module に imports 追加

「shared.module.ts」を編集する

```
imports: [CommonModule, MaterialModule, RouterModule, CoreModule, NgxTranslateModule],
```

まとめると下記の状態

```
@NgModule({
  declarations: [SidenavComponent, HeaderComponent, FooterComponent],
  imports: [CommonModule, MaterialModule, RouterModule, CoreModule, NgxTranslateModule],
  exports: [SidenavComponent, HeaderComponent, FooterComponent]
})
```
