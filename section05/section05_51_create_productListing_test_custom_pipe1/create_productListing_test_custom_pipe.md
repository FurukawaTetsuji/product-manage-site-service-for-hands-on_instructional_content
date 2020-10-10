# ① テストコードの作成

## ① - 1 カスタムパイプ のテスト

### a. 「FormattedNumberPipe.spec.ts」 を編集する

下記コードの describe の中を上書きする。

```
describe('FormattedNumberPipe', () => {
  const LOCALE_JP = 'ja-JP';
  const LOCALE_EN = 'en-US';
  const LOCALE_FR = 'fr-FR';
  const LOCALE_DE = 'de-DE';

  let pipe: FormattedNumberPipe;

  beforeEach(() => {
    TestBed.configureTestingModule({
      providers: [FormattedNumberPipe]
    });
    pipe = TestBed.inject(FormattedNumberPipe);
  });

  describe('#constractor', () => {
    it('should be created', () => {
      expect(pipe).toBeTruthy();
    });
  });
  describe('#transform', () => {
    describe('When the value to be converted is a number, it should return transformed result', () => {
      const parameters = [
        {
          value: '1234567',
          locale: LOCALE_JP,
          expectedValue: '1,234,567'
        },
        {
          value: '1234,567',
          locale: LOCALE_JP,
          expectedValue: '1,234,567'
        },
        {
          value: '1234567',
          locale: LOCALE_EN,
          expectedValue: '1,234,567'
        },
        {
          value: '1,234567',
          locale: LOCALE_EN,
          expectedValue: '1,234,567'
        },
        {
          value: '1234567',
          locale: LOCALE_FR,
          expectedValue: '1 234 567'
        },
        {
          value: '1234567',
          locale: LOCALE_DE,
          expectedValue: '1.234.567'
        }
      ];

      parameters.forEach((parameter) => {
        it(
          'should correctly converted from ' +
            parameter.value +
            ' to ' +
            parameter.expectedValue +
            ' in locale ' +
            parameter.locale,
          () => {
            expect(pipe.transform(parameter.value, parameter.locale)).toEqual(parameter.expectedValue);
          }
        );
      });
    });

    describe('When the value to be converted is not a number, it should return non transformed result', () => {
      it('should return non transformed result when the value to be converted is Japanese', () => {
        const value = 'あいうえお';
        const locale = 'ja-JP';
        expect(pipe.transform(value, locale)).toEqual(value);
      });
    });
  });

  describe('#parse', () => {
    it('should return parsed result', () => {
      const value = '1,234,567';
      const locale = 'ja-JP';
      expect(pipe.parse(value, locale)).toEqual('1234567');
    });
  });
});
```

### b. 「formatted-currency.pipe.spec.ts」 を編集する

下記コードの describe の中を上書きする。

```
describe('FormattedCurrencyPipe', () => {
  const LOCALE_JP = 'ja-JP';
  const LOCALE_EN = 'en-US';
  const LOCALE_FR = 'fr-FR';
  const LOCALE_DE = 'de-DE';
  const CURRENCY_JPY = 'JPY';
  const CURRENCY_USD = 'USD';
  const CURRENCY_EUR = 'EUR';

  let pipe: FormattedCurrencyPipe;

  beforeEach(() => {
    TestBed.configureTestingModule({
      providers: [FormattedCurrencyPipe]
    });
    pipe = TestBed.inject(FormattedCurrencyPipe);
  });

  describe('#constractor', () => {
    it('should be created', () => {
      expect(pipe).toBeTruthy();
    });
  });
  describe('#transform', () => {
    describe('When the value to be converted is an amount, it should return transformed result', () => {
      const parameters = [
        {
          value: '1234567',
          locale: LOCALE_JP,
          currency: CURRENCY_JPY,
          expectedValue: '1,234,567'
        },
        {
          value: '1234,567',
          locale: LOCALE_JP,
          currency: CURRENCY_JPY,
          expectedValue: '1,234,567'
        },
        {
          value: '1234567.12',
          locale: LOCALE_EN,
          currency: CURRENCY_USD,
          expectedValue: '1,234,567.12'
        },
        {
          value: '1,234567.12',
          locale: LOCALE_EN,
          currency: CURRENCY_USD,
          expectedValue: '1,234,567.12'
        },
        {
          value: '1234567.123',
          locale: LOCALE_FR,
          currency: CURRENCY_EUR,
          expectedValue: '1 234 567,12'
        },
        {
          value: '1234567.456',
          locale: LOCALE_DE,
          currency: CURRENCY_EUR,
          expectedValue: '1.234.567,46'
        }
      ];

      parameters.forEach((parameter) => {
        it(
          'should correctly converted from ' +
            parameter.value +
            ' to ' +
            parameter.expectedValue +
            ' in locale ' +
            parameter.locale +
            ' and in currency ' +
            parameter.currency,
          () => {
            expect(pipe.transform(parameter.value, parameter.locale, parameter.currency)).toEqual(
              parameter.expectedValue
            );
          }
        );
      });
    });

    describe('When the value to be converted is not an amount, it should return non transformed result', () => {
      it('should return non transformed result when the value to be converted is Japanese', () => {
        const value = 'あいうえお';
        const locale = 'ja-JP';
        const currency = 'JPY';
        expect(pipe.transform(value, locale, currency)).toEqual(value);
      });
    });
  });

  describe('#parse', () => {
    it('should return parsed result', () => {
      const value = '1,234,567';
      const locale = 'ja-JP';
      expect(pipe.parse(value, locale)).toEqual('1234567');
    });
  });
});
```

### c. 「parse-helper.spec.ts」 を編集する

下記コードの describe の中を上書きする。

```
describe('ParseHelper', () => {
  const LOCALE_JP = 'ja-JP';
  const LOCALE_EN = 'en-US';
  const LOCALE_FR = 'fr-FR';
  const LOCALE_DE = 'de-DE';
  const CURRENCY_JPY = 'JPY';
  const CURRENCY_USD = 'USD';
  const CURRENCY_EUR = 'EUR';

  describe('#parse', () => {
    describe('When the value is number', () => {
      const parameters = [
        {
          locale: LOCALE_JP,
          value: '1,234,567',
          expectedValue: '1234567'
        },
        {
          locale: LOCALE_EN,
          value: '1,234,567',
          expectedValue: '1234567'
        },
        {
          locale: LOCALE_FR,
          value: '1 234 567',
          expectedValue: '1234567'
        },
        {
          locale: LOCALE_DE,
          value: '1.234.567',
          expectedValue: '1234567'
        }
      ];

      parameters.forEach((parameter) => {
        it('should correctly converted in locale ' + parameter.locale, () => {
          expect(ParseHelper.parse(parameter.value, parameter.locale)).toEqual(parameter.expectedValue);
        });
      });
    });

    describe('When the value is currency', () => {
      const parameters = [
        {
          locale: LOCALE_JP,
          currency: CURRENCY_JPY,
          value: '1,234,567',
          expectedValue: '1234567'
        },
        {
          locale: LOCALE_EN,
          currency: CURRENCY_USD,
          value: '1,234,567.89',
          expectedValue: '1234567.89'
        },
        {
          locale: LOCALE_FR,
          currency: CURRENCY_EUR,
          value: '1 234 567,89',
          expectedValue: '1234567.89'
        },
        {
          locale: LOCALE_DE,
          currency: CURRENCY_EUR,
          value: '1.234.567,89',
          expectedValue: '1234567.89'
        }
      ];

      parameters.forEach((parameter) => {
        it(
          'should correctly converted in locale ' + parameter.locale + ' and in currency ' + parameter.currency,
          () => {
            expect(ParseHelper.parse(parameter.value, parameter.locale)).toEqual(parameter.expectedValue);
          }
        );
      });
    });
  });
});
```

## ① - 2 余分なテストクラス の削除

### a. 「regex-const.spec.ts」 を削除する

「regex-const.spec.ts」 が CLI コマンドで作成されていれば、それは削除する
