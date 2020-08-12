# ① テストコードの作成

## ① - 1 Pages のコンポーネント

### b. 「sign-in-page.component.spec.ts」 に DOM のテストを追加する

DOM テスト用のクラスを１つ追加する

```
ng g class testing/html-element-utility
```

中にメソッドを追加する

```
/**
  * Sets value to htmlinput element
  * @template T type of target component
  * @param fixture target fixture
  * @param querySelector css selector string
  * @param setValue value to set element
  */
static setValueToHTMLInputElement<T>(fixture: ComponentFixture<T>, querySelector: string, setValue: any) {
  const htmlInputElement: HTMLInputElement = fixture.debugElement.query(By.css(querySelector)).nativeElement;
  htmlInputElement.value = setValue;
  htmlInputElement.dispatchEvent(new Event('input'));
  htmlInputElement.dispatchEvent(new Event('blur'));
  fixture.detectChanges();
}
```

html-element-utility のテストクラスは削除する

テストメソッドを新たに追加する

```
// --------------------------------------------------------------------------------
// DOM test cases
// --------------------------------------------------------------------------------
describe('DOM placeholder', () => {
  it('title', () => {
    const htmlInputElement: HTMLInputElement = fixture.debugElement.query(By.css('.sign-in-title-wrapper'))
      .nativeElement;
    expect(htmlInputElement.innerText).toContain('EXAPMLE SITE');
  });

  it('sign in user account', () => {
    const htmlInputElement: HTMLInputElement = fixture.debugElement.query(By.css('#signin-user-account'))
      .nativeElement;
    expect(htmlInputElement.dataset.placeholder).toContain('ユーザアカウント');
  });
  it('sign in user password', () => {
    const htmlInputElement: HTMLInputElement = fixture.debugElement.query(By.css('#signin-user-password'))
      .nativeElement;
    expect(htmlInputElement.dataset.placeholder).toContain('パスワード');
  });
  it('saveBtn', () => {
    const htmlInputElement: HTMLInputElement = fixture.debugElement.query(By.css('#sign-in-button')).nativeElement;
    expect(htmlInputElement.innerText).toContain('サインイン');
  });
});

describe('DOM input test', () => {
  it('sign in user account', () => {
    const expectedValue = 'Username';
    HtmlElementUtility.setValueToHTMLInputElement(fixture, '#signin-user-account', expectedValue);
    expect(component.signInUserAccount.value).toEqual(expectedValue);
  });
  it('sign in user password', () => {
    const expectedValue = 'Password';
    HtmlElementUtility.setValueToHTMLInputElement(fixture, '#signin-user-password', expectedValue);
    expect(component.signInUserPassword.value).toEqual(expectedValue);
  });
});

describe('DOM input validation test', () => {
  it('sign in user account', () => {
    HtmlElementUtility.setValueToHTMLInputElement(fixture, '#signin-user-account', '');
    const validationError = fixture.debugElement.query(By.css('.validation-error')).nativeElement;
    expect(validationError).toBeTruthy();
  });
  it('sign in user password', () => {
    HtmlElementUtility.setValueToHTMLInputElement(fixture, '#signin-user-password', '');
    const validationError = fixture.debugElement.query(By.css('.validation-error')).nativeElement;
    expect(validationError).toBeTruthy();
  });
});

describe('DOM input test', () => {
  it('Should Enter input and create request', () => {
    HtmlElementUtility.setValueToHTMLInputElement(fixture, '#signin-user-account', expectedSignInRequestDto.Username);
    HtmlElementUtility.setValueToHTMLInputElement(
      fixture,
      '#signin-user-password',
      expectedSignInRequestDto.Password
    );
    const privateMethodName = 'createSignInRequestDto';
    const signInRequestDto: SignInRequestDto = component[privateMethodName]();
    expect(signInRequestDto).toEqual(expectedSignInRequestDto);
  });
});
```
