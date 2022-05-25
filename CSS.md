# CSS

## 縦書き 
再現するのにcssのwriting-modeを使うと思うが、出来る限り影響範囲は狭めた方がいい。

大きい親要素でまとめて指定しているとaタグでIDを指定した内部リンクが正常に飛ばなくなる

## 疑似要素 
cssで挿入できる要素

::beforeと::afterがある

使用にはcontentが必要

### （指定方法）

content: ‘文字列’;

content: url(画像URL);

* ちなみにimgタグには疑似要素は作れない
* デフォルトでインライン要素なので幅、高さなどの指定の際にはdisplay:blockが必要

## リンクの下線
text-decorationを使うパターンより下にある場合、疑似要素を使う

position:relativeを使い、疑似要素にposition:absolute、背景色、width:100%、height:1pxで線に見せる

## nth-of-type 

* nth-of-type(n + 4)で四件目以降のタグが該当するようになる

## グラデーション＆影
### box-shadow
影をつけることができる、insetを指定することで内側に影をつける事もできる
### background:liner-gradient(to ‘方向’, 色１, 色２)
背景にグラデーションをつけることができる
* background-colorではない点に注意
### text-shadow
テキストに影をつけることができる

コンマ区切りで複数回指定することで影を濃くしたり、縁取りすることができる

## 透過
opacity

## 画像のトリミング
object-fit:coverとobject-position,幅と高さの指定でトリミングが出来る

## リストの中央揃え 
display:table, margin:0px auto

## 中央揃え,左右揃え 
### 中央揃え
* text-align:center
* margin: 0px auto
* top: 50%;left: 50%;transform: translate(-50%,-50%);→上下左右中央揃え
* display: flex;
　justify-content: center;
　align-items: center;

### 左揃え
* text-align:left
* margin-right:auto
### 右揃え
* text-align:right
* margin-left:auto

### text-align
インライン要素に有効で、親要素に記述する

### margin: auto
ブロック要素に有効で要素本体に記述する、合わせて幅も指定する

## 画像の余白
imgタグには余白が設定されており、vertical-align:bottomで余白を消す？ことが出来る

## 均等な三等分の作り方
calc(100% / 3)で33.33%より正確な三等分ができる

## ブラウザ表示に合わせる
長さの単位で100vw,100vhなどがある
この際、親要素を超えて表示することができる

## フォントの複数指定
コンマで区切る,名前に半角スペースが入っている場合、’’で囲う

## 文字幅、行間 #css 
文字幅→letter-spacing

行間→line-height

## インラインSVG
HTMLに直接パスが打ち込まれたSVGはfill属性で塗りつぶしの色を指定できる

画像ファイルの場合、ホバーなどで色を反転させたい時２つの画像ファイルが必要だが、この方法だと画像ファイルなしで出来る（と思う）
* fill→塗りつぶしの色
* fill-opacity→透過度
* stroke→線の色
* stroke-width→線の幅

## Inputなどの初期値(placeholder)にCSSを当てる方法 
```
セレクタ::placeholder {
    〜
}
```
## border-box
HTMLだけに指定しておくといくつかの要素で適用されていない場合があるので*での指定推奨

html *などで全体に適用させても疑似要素には適用されていないので注意

```
html {
    box-sizing: border-box;
}
*,
*:before,
*:after {
    box-sizing: inherit;
}
```
とするのがお勧めらしい
https://jajaaan.co.jp/web-production/frontend/box-sizing/

## フォントサイズ
* html要素に指定した「ルートのフォントサイズ」
* body要素に指定した「デフォルトのフォントサイズ」

ルートのフォントサイズとは、ブラウザの基準となるフォントサイズ。
このサイズを基準としてremやemのサイズを指定する。

デフォルトのフォントサイズとは、要素にフォントサイズを指定しなかった場合、
自動的に継承して適用されるフォントサイズ。

## CSSだけでカウントする方法 
```
body { 
    counter-reset: 変数名;
}
セレクタ::before {   
    counter-increment: 変数名; 
    content: counter(変数名);
}
```
## フレックスボックスの均等配置で左詰めする方法
3列想定のデザインで２列しかないと左右に配置されてしまう件対応
```
セレクタ::after {
    content: "";
    width: 32%;  /* .boxに指定したwidthと同じ幅を指定する */
    height: 0;
}
```
同じ理屈でbeforeも使えば、４列までは可能

## position: stickyについて
親要素どころか祖先要素にoverflow:hiddenが指定されていると機能しなくなる

* つまり、直系の要素以外がoverflow:hiddenになってる場合は機能する

top指定が必要でその座標にくっ付いたように振る舞う

## Mix-blend-modeについて
```
background: white;
mix-blend-mode: difference;
```
でネガポジ反転ができる。

他にも下記のようなものもある
|名称|パラメタ
|--|--
|焼きこみ|color-burn
|覆い焼き|color-dodge
|オーバーレイ|overlay


## CSSアニメの動作速度について 
transform: translate3d(0,0,0);やbackface-visibility: hidden;には描画速度を速くする効果がある（らしい）

3dは一箇所でもあるとページ全体に影響が出るのでbodyタグあたりに書くと良い

backfaseは見えない部分を非表示にする

## position: fixedが効かない原因 
親要素にtransformを指定していませんか？

https://dezanari.com/css-position-fixed-not-work/

## nth-childとnth-of-typeの違い
まず原則として、nth-childは、相手がhタグだろうとpタグだろうとaタグだろうと関係なく、とにかくn番目の要素を指定します。

その原則を踏まえた上で、さらに細かく条件づけしたいときに、「p:nth-child(3)」 ( = ３番目の要素で、なおかつpタグのとき)のような形をとります。

「３番目にでてくるpタグ」ではありません。それは「p:nth-of-type(3)」です。
https://learning.mihune-web.com/nth-child/
