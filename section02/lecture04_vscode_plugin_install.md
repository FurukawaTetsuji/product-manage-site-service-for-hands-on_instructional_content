# vscode plugin のインストール

## vscode の設定を変更（保管時に自動整形するよう）

Visual Studio Code のメニューから、「ファイル」-「基本設定」-「設定」
「formatOn」で検索して  
「Format on Paste」、「Format on Save」、「Format on Type」をチェック ON

## Tslint

TypeScript のルールをチェックするプラグイン

## Prettier

コードフォーマッタ

- HTML、css、scss は除外して使っている。
- デフォルトでは文字列はダブルクオート区切りなのでシングルクオートに統一した。

Visual Studio Code のメニューから、「ファイル」-「基本設定」-「設定」
「prettier」で検索して  
「disable languages」に「html」、「css」、「scss」を追加する  
「.prettierrc.js」ファイルをプロジェクト直下に追加して、下記を追加する

```
module.exports = {
tabWidth: 2,
singleQuote: true,
trailingComma: 'none',
printWidth: 120
};
```

## Beautify css/sass/scss/less

コードフォーマッタ  
HTML、css、scss 用に使っている

## Angular Support

HTML ←→ Typescript と双方に定義へ飛ぶことができるプラグイン。

## vscode-angular-html

HTML に Angular 用のシンタックスハイライトを追加してくれるプラグイン

## Comments in Typescript

TypeScript にコメントを作成できるプラグイン  
「Ctrl」+「Alt」+「C」2 回でコメントを自動生成できる

## TypeScript Import Sorter

import 文をソートするプラグイン
Visual Studio Code のメニューから、「ファイル」-「基本設定」-「設定」  
「importsorter」で検索して  
importSorter.generalConfiguration.sortOnBeforeSave をチェック ON
