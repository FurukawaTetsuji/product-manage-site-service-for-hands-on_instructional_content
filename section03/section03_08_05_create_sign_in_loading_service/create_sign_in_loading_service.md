# ① ローディングの追加

## ① - 1 ローディングを編集

### a. core モジュールに MaterialModule を取込む

imports: []に MaterialModule を追加する

### b. ローディングサービスを編集

「loading.service.ts」を編集する
（ローディングの表示を開始する、終了する）

```
export class LoadingService {
  isLoading: boolean;

  constructor() {}

  /**
   * Starts loading
   */
  startLoading() {
    this.isLoading = true;
  }

  /**
   * Stops loading
   */
  stopLoading() {
    this.isLoading = false;
  }
}
```

### c. コンポーネントの ts を編集

「loading.component.ts」を編集

コンストラクタにローディングサービスを追加する

```
constructor(public loadingService: LoadingService) {}
```

### d. コンポーネントの html を編集

「loading.component.html」を編集する

```
<div class="loading-shade" *ngIf="loadingService.isLoading">
  <mat-spinner class="loading-spinner"></mat-spinner>
</div>
```

### e. コンポーネントの css を編集

```
.loading-shade {
  position: absolute;
  width: 100%;
  height: 100vh;
  background: rgba(0, 0, 0, 0.15);
  z-index: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}
```

## ① - 2 ローディングの組込み

### a. サインインページコンポーネントの ts を編集

「sign-in-page.component.ts」を編集する

コンストラクタに LoadingService を追加する

```
constructor()
  private accountService: AccountService,
  private formBuilder: FormBuilder,
  private loadingService: LoadingService,
  private routingService: RoutingService,
  public translateService: TranslateService
) {}
```

ローディングの開始と終了を追加サインインメソッドに追加する

```
private signIn(signInRequestDto: SignInRequestDto) {
  // Starts Loading.
  this.loadingService.startLoading();

  // Signs in and gets response dto.
  const signInResponseDto: Observable<SignInResponseDto> = this.accountService.signIn(signInRequestDto);
  signInResponseDto.subscribe(responseDto => {
    if (responseDto != null) {
      // Sets account information.
      this.setUpUserAccount(responseDto);
      // Moves to the Product listing page.
      this.routingService.navigate(UrlConst.PATH_PRODUCT_LISTING);
    }
    // Stops Loading.
    this.loadingService.stopLoading();
  });
}
```

## ① - 3 起動してローディングの確認

コマンド

```
ng serve -o --proxy-config proxy.conf.json
```
