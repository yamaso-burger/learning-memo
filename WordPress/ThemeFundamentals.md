# テーマの基本
[参照元：公式ドキュメント Theme Handbook](https://developer.wordpress.org/themes/)
▼ テーマとは
WordPress によって保持しているコンテンツを読み込んで、どのように表示するかを記述したもの

## テーマの型について
2種類ある
- [Block themes](https://developer.wordpress.org/themes/getting-started/what-is-a-theme/#block-themes)
    - 新しいテーマの書き方。こっちを学ぶ
    - templates をエディターの中で編集できる
    - 
- [Classic themes](https://developer.wordpress.org/themes/getting-started/what-is-a-theme/#classic-themes)
    - PHP を用いた方法

## テーマ開発にあたって
### 開発に必要な技術
HTML、CSS、Javascript の知識が必要になるので適宜公式ドキュメントを参照すること

- [MDN Web Docs: HTML](https://developer.mozilla.org/en-US/docs/Web/HTML)
- [MDN Web Docs: CSS](https://developer.mozilla.org/en-US/docs/Web/CSS)
- [PHP official documentation](https://www.php.net/docs.php)
- [MDN Web Docs: JSON](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON)
- [MDN Web Docs: JavaScript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript)

### 開発に必要な環境セットアップやツールについて
[参考リンク](https://developer.wordpress.org/themes/getting-started/tools-and-setup/)

例えば、便利なプラグインを入れておくことで開発がスムーズに進められる

### 新しいテーマを作成する
WordPress 公式のプラグインを用いることで空のテーマテンプレートを作成できる
→ [Create Block Theme](https://developer.wordpress.org/themes/getting-started/quick-start-guide/#using-the-create-block-theme-plugin)

## テーマファイルの基本
### ディレクトリ構成
基本的なテーマは例として以下のような構成をとる
```
parts
└ footer.html
└ header.thml
patterns
└ example.php
styles
└ example.json
templates
└ 404.html
└ archive.html
└ index.html (required)
└ singular.html
README.txt
functions.php
screenshot.png
style.css (required)
theme.json
```
中でも以下のファイルは WordPress において最低限テーマとして認識されるのに必要なファイルである
- templates/index.html
- sytle.css

#### style.css メインスタイルシート
[参考ドキュメント](https://developer.wordpress.org/themes/core-concepts/main-stylesheet/)
メインスタイルシートは WordPress テーマに必須なファイルで、そのテーマに関する情報を記載する
```css
/*
Theme Name: Burger Diary
Theme URI: https://github.com/yamaso-burger/burger-diary-remodel.git
Author: Yamaso
Author URI: https://github.com/yamaso-burger
Description: ハンバーガーブログサイト Burger Diary 専用テーマ
Requires at least: 6.0
Tested up to: 6.4.3
Requires PHP: 5.7
Version: 1.0.0
License: GNU General Public License v2 or later
License URI: http://www.gnu.org/licenses/gpl-2.0.html
Text Domain: burgerdiary
Tags: blog, full-site-editing, 
*/
```
スタイルシートなのでもちろん、CSS を記述して見た目を制御することは可能である。
→ しかし、理想的には全てのデザインの情報は`theme.json`で管理されるのがよい


# テーマによってできること
## 管理画面 Theme -> Appearance での表示のカスタマイズ
テーマでの設定やルールをもとに、開発者やテーマのユーザは自由に表示をカスタマイズできる
- Style での色の変更
- Style の文字の大きさや書式の変更