# ① サインイン画面の作成

## ① - 3 Material 化

Input やボタンを Material 化する

### a. pages.module.ts の編集

imports: []に MaterialModule, ReactiveFormsModule を追加する

### b. sign-in-page.component.ts の編集

コンストラクタに Reactive form の FormBuilder を追加する

```
private formBuilder: FormBuilder,
```

SignInPageComponent クラス中に Reactive form の FormControl を 2 つ追加する（アカウント、パスワード）  
FormBuilder のグループに FormControl を追加する

```
signInUserAccount = new FormControl('', [Validators.required]);
signInUserPassword = new FormControl('', [Validators.required]);

signInForm = this.formBuilder.group({
  signInUserAccount: this.signInUserAccount,
  signInUserPassword: this.signInUserPassword
});
```

### c. sign-in-page.component.html の編集

<form>タグにフォームグループを追加する

```
<form [formGroup]="signInForm">
```

input の内容を material に変更する

```
<mat-form-field class="form-field">
  <input id="signin-user-account" matInput type="text" formControlName="signInUserAccount"
    placeholder="{{ 'signInPage.userAccount' | translate }}">
</mat-form-field>
```

```
<mat-form-field class="form-field">
  <input id="signin-user-password" matInput type="password" formControlName="signInUserPassword"
    placeholder="{{ 'signInPage.userPassword' | translate }}">
</mat-form-field>
```

```
<button mat-flat-button id="sign-in-button" class="flat-button" type="button"
[disabled]="!signInForm.valid">{{ 'signInPage.signInButton' | translate }}</button>
```

## ① - 4 バリデーション

Validation を追加する

```
<mat-error class="validation-error" *ngIf="signInUserAccount.hasError('required')">
  {{'validationError.required'|translate}}
</mat-error>
```

```
<mat-error class="validation-error" *ngIf="signInUserPassword.hasError('required')">
  {{'validationError.required'|translate}}
</mat-error>
```

## ① - 5 Material の css

「sign-in-page.component.scss」を編集する

```
.sign-in-criteria-wrapper input[type=text],
.sign-in-criteria-wrapper input[type=password] {
  border-width: 0;
  display: block;
  width: 20rem;
  margin: 50px auto 50px;
}

.sign-in-criteria-wrapper .button {
  display: block;
  padding: 10px 0;
  margin: 50px auto 50px;
  width: 200px;
  border-radius: 2px;
  background-color: #78BBE6;
  color: #ffffff;
  text-decoration: none;
  text-align: center;
  letter-spacing: 1px;
  font-size: 1.2rem;
}

.sign-in-criteria-wrapper .button:hover {
  opacity: 0.9;
}
```

は削除して、以降のコメントアウト部分を復活させる
