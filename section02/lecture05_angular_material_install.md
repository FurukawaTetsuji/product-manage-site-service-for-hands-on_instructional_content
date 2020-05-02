# Angular Material のインストール

## Angular Material のサイト

https://material.angular.io/guide/getting-started

## Material のインストールコマンド

```
ng add @angular/material
```

## Material をひとつにまとめた module を作る

```
ng g module material
```

## module に追加する内容 1

```
import { MatButtonModule } from '@angular/material/button';
```

imports: [] と
exports: [] に
MatButtonModule を加える

## module に追加する内容 2

```
import { MatButtonToggleModule } from '@angular/material/button-toggle';
import { MatCardModule } from '@angular/material/card';
import { MatCheckboxModule } from '@angular/material/checkbox';
import { MatNativeDateModule } from '@angular/material/core';
import { MatDatepickerModule } from '@angular/material/datepicker';
import { MatDialogModule } from '@angular/material/dialog';
import { MatDividerModule } from '@angular/material/divider';
import { MatFormFieldModule } from '@angular/material/form-field';
import { MatGridListModule } from '@angular/material/grid-list';
import { MatIconModule } from '@angular/material/icon';
import { MatInputModule } from '@angular/material/input';
import { MatListModule } from '@angular/material/list';
import { MatMenuModule } from '@angular/material/menu';
import { MatPaginatorModule } from '@angular/material/paginator';
import { MatProgressSpinnerModule } from '@angular/material/progress-spinner';
import { MatRadioModule } from '@angular/material/radio';
import { MatSelectModule } from '@angular/material/select';
import { MatSidenavModule } from '@angular/material/sidenav';
import { MatTableModule } from '@angular/material/table';
import { MatToolbarModule } from '@angular/material/toolbar';
import { MatTooltipModule } from '@angular/material/tooltip';
```

imports: [] と
exports: [] に

```
MatButtonToggleModule,MatCardModule,MatCheckboxModule,MatDatepickerModule,MatDialogModule,MatDividerModule,MatFormFieldModule,MatGridListModule,MatIconModule,MatInputModule,MatListModule,MatMenuModule,MatNativeDateModule,MatPaginatorModule,MatProgressSpinnerModule,MatRadioModule,MatSelectModule,MatSidenavModule,MatTableModule,MatToolbarModule,MatTooltipModule
```

を加える

## app.module に MaterialModule を追加

app.module の imports に MaterialModule を追加する

## Material icon のインストール

## コマンド

```
npm install material-design-icons
```

## 脆弱性のチェック

```
npm audit
```

## 脆弱性のチェック（修整も）

```
npm audit fix
```

（自動で修整ができない分は保留）

## その他

参考にしたサイト  
https://github.com/angular/angular-cli/issues/2662
