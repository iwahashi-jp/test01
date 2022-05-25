# HTML
## フォント
* ゴシック体→sans-serif
* 明朝体→serif

## labelタグ
formのinputタグなどに使い、ラベルで表示した見出しをクリックすることで該当するinputタグにフォーカスが移るようになる

for属性とinputタグのIDが一致する場合、またはlabelタグでinputタグを囲んだ場合、機能する

## pictureタグ
souceタグとimgタグでレスポンシブで画像が切り替わるように作ることが出来る
```
<picture>
    <sorce srcset=“URL” media=“(メディアクエリ)”>
    <img src=“URL”>
</picture>
```
と使うと上から順に表示される
また、Webp画像に対応していないブラウザ用に他の画像を表示させるのにも使える

## ファビコン
```
<link rel=“icon” href=“URL”>
```
## 外部サイト
特に理由がない場合は、```<a target=“_blank”>```が望ましい
