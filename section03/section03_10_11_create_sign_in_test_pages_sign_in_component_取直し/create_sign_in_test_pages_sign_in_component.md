# ① テストコードの作成

## ① - 1 Pages のコンポーネント

### a. 「sign-in-page.component.spec.ts」 を編集する

インポート文を追加（クイックフィックスができなかったため）

```
import { TranslateTestingModule } from 'ngx-translate-testing';
import { MaterialModule } from 'src/app/material/material.module';
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
```

xdescribe( を describe( に戻す

もともと書かれていたソースを下記で囲む

```
describe('#constractor', () => {
  it('should create', () => {
    expect(component).toBeTruthy();
  });
});
```

describe のすぐ下に変数を追加

```
const expectedSignInRequestDto: SignInRequestDto = createExpectedRequestDto();
const expectedSignInResponseDto: SignInResponseDto = createExpectedResponseDto();
```

let fixture の下に追加

```
let accountServiceSpy: { signIn: jasmine.Spy; setUser: jasmine.Spy };
let titleI18ServiceSpy: { setTitle: jasmine.Spy };
let router: Router;
```

beforeEach(async(() => { の下に spy を追加

```
accountServiceSpy = jasmine.createSpyObj('AccountService', ['signIn', 'setUser']);
titleI18ServiceSpy = jasmine.createSpyObj('TitleI18Service', ['setTitle']);
```

TestBed.configureTestingModule({ の下に追加

```
schemas: [NO_ERRORS_SCHEMA],
imports: [
  RouterTestingModule,
  TranslateTestingModule.withTranslations({ ja: require('src/assets/i18n/ja.json') }),
  MaterialModule,
  BrowserAnimationsModule,
  ReactiveFormsModule
],
providers: [
  FormBuilder,
  { provide: AccountService, useValue: accountServiceSpy },
  { provide: TitleI18Service, useValue: titleI18ServiceSpy }
],
```

compileComponents(); の下に追加

```
router = TestBed.inject(Router);
```

beforeEach(async(() => {})); は、まとめると下記になる

```
beforeEach(async(() => {
  accountServiceSpy = jasmine.createSpyObj('AccountService', ['signIn', 'setUser']);
  titleI18ServiceSpy = jasmine.createSpyObj('TitleI18Service', ['setTitle']);

  TestBed.configureTestingModule({
    schemas: [NO_ERRORS_SCHEMA],
    imports: [
      RouterTestingModule,
      TranslateTestingModule.withTranslations({ ja: require('src/assets/i18n/ja.json') }),
      MaterialModule,
      BrowserAnimationsModule,
      ReactiveFormsModule
    ],
    providers: [
      FormBuilder,
      { provide: AccountService, useValue: accountServiceSpy },
      { provide: TitleI18Service, useValue: titleI18ServiceSpy }
    ],
    declarations: [SignInPageComponent]
  }).compileComponents();
  router = TestBed.inject(Router);
}));
```

beforeEach(() => {の一番下に追加

```
setupBrowserLanguage('ja');
```

テストメソッドを新たに追加する

```
describe('#ngAfterViewChecked', () => {
  it('should set title', () => {
    component.ngAfterViewChecked();
    expect(titleI18ServiceSpy.setTitle.calls.count()).toBeGreaterThan(1);
  });
});

describe('#singnIn', () => {
  it('should not sign in', () => {
    accountServiceSpy.signIn.and.returnValue(of(null));
    component.clickSignInButton();
    expect(accountServiceSpy.setUser.calls.count()).toEqual(0);
  });

  it('should sign in', () => {
    accountServiceSpy.signIn.and.returnValue(of(expectedSignInResponseDto));
    spyOn(router, 'navigate');

    component.clickSignInButton();

    expect(accountServiceSpy.setUser.calls.count()).toEqual(1);
    expect(router.navigate).toHaveBeenCalledWith([UrlConst.SLASH + UrlConst.PATH_PRODUCT_LISTING]);
  });
});

describe('#getLanguage', () => {
  const privateMethodName = 'getLanguage';

  it('lang without hypen', () => {
    const language = 'ja';
    const expectedLanguage = 'ja';
    expect(component[privateMethodName](language)).toEqual(expectedLanguage);
  });

  it('lang with hypen', () => {
    const language = 'ja-JP';
    const expectedLanguage = 'ja';
    expect(component[privateMethodName](language)).toEqual(expectedLanguage);
  });
});
```

クラスの外側にメソッドを追加する。

```
function createExpectedRequestDto(): SignInRequestDto {
  return { Username: 'Username', Password: 'Password' };
}

function createExpectedResponseDto(): SignInResponseDto {
  return {
    userAccount: 'userAccount',
    userName: 'userName',
    userLocale: 'ja-JP',
    userLanguage: 'ja',
    userTimezone: 'Asia/Tokyo',
    userTimezoneOffset: '+0900',
    userCurrency: 'JPY'
  };
}

function setupBrowserLanguage(language: string) {
  const defineGetter = '__defineGetter__';
  window.navigator[defineGetter]('language', () => {
    return language;
  });
}
```

### その他

インポート文の追加文（クイックフィックスができなかった場合こちらを使って手動で追加してください）

```
import { TranslateTestingModule } from 'ngx-translate-testing';
import { MaterialModule } from 'src/app/material/material.module';
import { AccountService } from 'src/app/pages/services/account.service';
import { TitleI18Service } from 'src/app/shared/services/title-i18.service';

import { NO_ERRORS_SCHEMA } from '@angular/core';
import { async, ComponentFixture, TestBed } from '@angular/core/testing';
import { FormBuilder, ReactiveFormsModule } from '@angular/forms';
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
import { RouterTestingModule } from '@angular/router/testing';

import { SignInPageComponent } from './sign-in-page.component';
```
