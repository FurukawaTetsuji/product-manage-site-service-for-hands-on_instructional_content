# ① タイトルサービスの追加

ブラウザの左上システムのタイトル部分の表示内容

## ① - 1 タイトルサービスを編集

### a. 「title-i18.service.ts」 を編集する

コンストラクタ追加

```
constructor(
  private translateService: TranslateService,
  public title: Title
) {}
```

```
/**
  * Sets title
  * @param screenName Name of screen
  */
public setTitle(screenName: string) {
  const titleSystem = this.translateService.instant('title.system');
  const titleSub = this.translateService.instant('title.' + screenName);
  this.title.setTitle(titleSystem + titleSub);
}
```

### b. TitleI18Service を組込む

「sign-in-page.component.ts」を編集する

コンストラクタに TitleI18Service を追加する

```
private titleI18Service: TitleI18Service,
```

SignInPageComponent の implements に AfterViewChecked を追加する

```
export class SignInPageComponent implements OnInit, AfterViewChecked {...}
```

クラス内に下記のパブリックメソッド（ngAfterViewChecked イベント）を追加する

```
/**
  * after view checked
  */
ngAfterViewChecked() {
  this.titleI18Service.setTitle(UrlConst.PATH_SIGN_IN);
}
```

## ① - 2 起動してタイトルを確認

コマンド

```
ng serve -o --proxy-config proxy.conf.json
```
