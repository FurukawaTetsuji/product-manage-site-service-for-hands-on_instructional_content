# ① ページネーターの多言語化

## ① - 1 MatPaginatorI18nService

### a. 「MatPaginatorI18nService」の追加

CLI コマンド

```
ng g service core/services/mat-paginator-i18n
```

### b. 「mat-paginator-i18n.service.ts」の編集

#### コンスト

```
const ITEMS_PER_PAGE = 'Items per page:';
const NEXT_PAGE = 'Next page';
const PREV_PAGE = 'Previous page';
```

#### クラス

```
export class MatPaginatorI18nService extends MatPaginatorIntl {
  constructor(private translate: TranslateService) {
    super();

    this.translate.onLangChange.subscribe((e: Event) => {
      this.setupLabels();
    });

    this.setupLabels();
  }

  getRangeLabel = (page: number, pageSize: number, length: number) => {
    if (length === 0 || pageSize === 0) {
      return `0 / ${length}`;
    }
    length = Math.max(length, 0);
    const startIndex = page * pageSize;
    const endIndex = startIndex < length ? Math.min(startIndex + pageSize, length) : startIndex + pageSize;
    return `${startIndex + 1} – ${endIndex} / ${length}`;
    // tslint:disable-next-line: semicolon
  };

  private setupLabels(): void {
    this.itemsPerPageLabel = this.translate.instant(ITEMS_PER_PAGE);
    this.nextPageLabel = this.translate.instant(NEXT_PAGE);
    this.previousPageLabel = this.translate.instant(PREV_PAGE);
  }
}
```

### c. pages.module への組込み

「pages.module.ts」を編集する  
下記 privider を追加

```
  providers: [{ provide: MatPaginatorIntl, useClass: MatPaginatorI18nService }],

```

クイックフィックスが効かない場合はこちら

import { MatPaginatorI18nService } from '../core/services/mat-paginator-i18n.service';

## 参考

https://material.angular.io/components/paginator/api
