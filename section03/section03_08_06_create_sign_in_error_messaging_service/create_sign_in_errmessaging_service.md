# ① エラーメッセージの追加

エラーメッセージのコンポーネントとサービスを編集する

## ① - 1 core モジュールに NgxTranslateModule を取込む

imports: []に NgxTranslateModule を追加する

## ① - 2 エラーメッセージサービスの編集

### a. エラーメッセージサービスを編集する

「error-messaging.service.ts」 を編集する  
4 つのメソッドを追加する  
エラーメッセージは文言を直接やり取りせずに他言語の json ファイルのメッセージプロパティをわたす

```
export class ErrorMessagingService {
  private messageProperty: string;

  constructor() {}

  /**
   * Gets message property
   * @returns message property
   */
  getMessageProperty(): string {
    return this.messageProperty;
  }

  /**
   * Sets message property
   * @param message message property
   */
  setMessageProperty(message: string): void {
    this.messageProperty = message;
  }

  /**
   * Clears message property
   */
  clearMessageProperty(): void {
    this.messageProperty = '';
  }

  /**
   * Setups page error message from response
   * @param error error from api response
   */
  setupPageErrorMessageFromResponse(error: any) {
    switch (error.status) {
      case 400:
        this.setMessageProperty('errMessage.http.badRequest');
        break;
      case 401:
        this.setMessageProperty('errMessage.http.unAuthorized');
        break;
      case 404:
        this.setMessageProperty('errMessage.http.notFound');
        break;
      case 500:
        if ('Duplicated key.' === error.error.message) {
          this.setMessageProperty('errMessage.http.duplicateKeyException');
        } else if ('Exclusive error occurred.' === error.error.message) {
          this.setMessageProperty('errMessage.http.exclusiveProcessingException');
        } else if ('There is no stock.' === error.error.message) {
          this.setMessageProperty('errMessage.http.outOfStockException');
        } else if ('Data not found.' === error.error.message) {
          this.setMessageProperty('errMessage.http.datNotFoundException');
        } else {
          this.setMessageProperty('errMessage.http.internalServerError');
        }
        break;
      default:
        this.setMessageProperty('errMessage.http.GenericError');
        break;
    }
  }
}
```

## ① - 3 エラーメッセージコンポーネントを編集

### a. コンポーネントの ts を編集する

（コンストラクタの TranslateService は html 側で必要）

```
export class ErrorMessagingComponent implements OnInit {
  constructor(public errorMessagingService: ErrorMessagingService, public translateService: TranslateService) {}

  ngOnInit(): void {
    this.errorMessagingService.clearMessageProperty();
  }
}
```

### b. コンポーネントの html を編集する

```
<p *ngIf="errorMessagingService.getMessageProperty()">{{errorMessagingService.getMessageProperty() | translate}}</p>
```

### c. コンポーネントの scss を編集する

```
p {
  text-align: center;
  margin: 0 auto 0;
  padding: 10px 0;
  color: #ffffff;
  background-color: #f48fb1;
}
```

## ① - 4 エラーメッセージの組込み

### a. pages モジュールに Core モジュールを取込み

imports: []に CoreModule を追加する

### b. アカウントサービスを編集

「account.service.ts」を編集する

コンストラクタに ErrorMessagingService を追加する

```
constructor(private http: HttpClient, private errorMessageService: ErrorMessagingService) {}
```

サインインメソッド中の catchError((error) => {} に #setupPageErrorMessageFromResponse() メソッドの呼び出しを追加する

```
signIn(signInRequestDto: SignInRequestDto): Observable<SignInResponseDto> {
  const webApiUrl = ApiConst.PATH_API_ROOT + ApiConst.PATH_SIGN_IN;
  const headers = new HttpHeaders({
    authorization: 'Basic ' + btoa(signInRequestDto.Username + ':' + signInRequestDto.Password)
  });

  return this.http
    .post<SignInResponseDto>(webApiUrl, signInRequestDto, { headers })
    .pipe(
      catchError((error) => {
        this.errorMessageService.setupPageErrorMessageFromResponse(error);
        return of(null as SignInResponseDto);
      })
    );
}
```

### c. エラーメッセージディレクティブを取込み

sign-in-page.component.html のタイトルの下にエラーメッセージのディレクティブを追加する

```
<div class="sign-in-title-wrapper">
  <p>EXAPMLE SITE</p>
</div>
<app-error-messaging></app-error-messaging>
```

## ① - 5 起動確認

コマンド

```
ng serve -o --proxy-config proxy.conf.json
```
