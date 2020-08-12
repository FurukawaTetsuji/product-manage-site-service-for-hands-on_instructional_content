# ① テストコードの作成

## ① - 1 アカウントサービス

### a. 「account.service.spec.ts」 を編集する

テストメソッドを新たに追加する  
（describe('#getUser,#setUser', () => {});の上）

```
describe('#getMenu', () => {
  const webApiUrl = ApiConst.PATH_API_ROOT + ApiConst.PATH_MENU;

  it('should return expected response', () => {
    const expectedMenuListResponseDto = createExpectedMenuListResponseDto();

    // Subscribes to api.
    service.getMenu().subscribe((response) => {
      expect(response).toEqual(expectedMenuListResponseDto);
      expect(errorMessagingServiceSpy.setupPageErrorMessageFromResponse.calls.count()).toBe(0);
    }, fail);

    // Responds with the mock
    const req = httpTestingController.expectOne(webApiUrl);
    expect(req.request.method).toEqual('GET');
    req.flush(expectedMenuListResponseDto);
  });

  it('should return null when response is 404 Not Found', () => {
    const errorStatus = 404;
    const errorMessage = '404 Not Found';

    // Subscribes to api.
    service.getMenu().subscribe((response) => {
      expect(response).toBeNull();
      expect(errorMessagingServiceSpy.setupPageErrorMessageFromResponse.calls.count()).toBe(1);
    }, fail);

    // Responds with the mock
    const req = httpTestingController.expectOne(webApiUrl);
    expect(req.request.method).toEqual('GET');
    req.flush(errorMessage, { status: errorStatus, statusText: errorMessage });
  });
});

describe('#getAvailablePages', () => {
  const webApiUrl = ApiConst.PATH_API_ROOT + ApiConst.PATH_AVAILABLE_PAGES;

  it('should return expected response', () => {
    const expectedResponse = Array(UrlConst.PATH_PRODUCT_LISTING, UrlConst.PATH_PRODUCT_REGISTERING);

    // Subscribes to api.
    service.getAvailablePages().subscribe((response) => {
      expect(response).toEqual(expectedResponse);
      expect(errorMessagingServiceSpy.setupPageErrorMessageFromResponse.calls.count()).toBe(0);
    }, fail);

    // Responds with the mock
    const req = httpTestingController.expectOne(webApiUrl);
    expect(req.request.method).toEqual('GET');
    req.flush(expectedResponse);
  });

  it('should return null when response is 404 Not Found', () => {
    const errorStatus = 404;
    const errorMessage = '404 Not Found';

    // Subscribes to api.
    service.getAvailablePages().subscribe((response) => {
      expect(response).toBeNull();
      expect(errorMessagingServiceSpy.setupPageErrorMessageFromResponse.calls.count()).toBe(1);
    }, fail);

    // Responds with the mock
    const req = httpTestingController.expectOne(webApiUrl);
    expect(req.request.method).toEqual('GET');
    req.flush(errorMessage, { status: errorStatus, statusText: errorMessage });
  });
});

describe('#signOut', () => {
  const webApiUrl = ApiConst.PATH_API_ROOT + ApiConst.PATH_SIGN_OUT;

  it('should return expected response', () => {
    const user: User = new User();
    service.setUser(user);

    // Subscribes to api.
    service.signOut().subscribe((response) => {
      expect(service.getUser()).toBeNull();
      expect(errorMessagingServiceSpy.setupPageErrorMessageFromResponse.calls.count()).toBe(0);
    });

    // Responds with the mock
    const req = httpTestingController.expectOne(webApiUrl);
    expect(req.request.method).toEqual('POST');
    req.flush(null);
  });
});
```

function createUser() {} の上に追加する

```
function createExpectedMenuListResponseDto() {
  const codeList = new Array('subMenu1', 'subMenu2', 'subMenu3');
  const menuListResponseDto: MenuListResponseDto = {
    menuCode: 'menu1',
    subMenuCodeList: codeList
  };
  const expectedMenuListResponseDto = Array(menuListResponseDto);
  return expectedMenuListResponseDto;
}
```
