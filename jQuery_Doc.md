# jQuery/javascript
## jQueryの読み込み順
```
$(function() {
    console.log('ready');
    $(window).on('load', function() {
        console.log('load');
    });
});
console.log('outside ready');
$(window).on('load', function() {
    console.log('outside load');
});
```
jQuery3では「outside ready → outside load → ready」の順番、

jQuery2では「outside ready → ready → outside load → load」の順番でconsoleが表示されました。

また、```$(function)```が非同期になったことで、```$(function)```内に```$(window).on(‘load’, function);```が入っていると実行されなくなっているようです。

詳しくは
https://cly7796.net/blog/javascript/started-with-jquery3/

## プラグイン 
スライド
* slick(jQuery)
* swiper

パララックス（視差の実装）
* simpleParallax

画像のモーダルウィンドウ
* lightbox(jQuery)

## webStorage
クッキーとは別のWebクライアントにデータを保存する仕組み
```
var webStore = function () {
     ~ 
};
webStorage();
```
localStorageとsessionStorageの二種類がある

使用例

sessionStorage.getItem(‘引数’);で値を取得する

sessionStorage.setItem(‘引数’,’値’);で値をセットする
## テンプレート文字列(ES6)
``（バッククオート）で文字列を囲むと、${}で文字列内に変数展開ができ、改行も反映できる。
```
console.log(`My name is ${name}`);
```
## アロー関数(ES6)
よくある関数定義
```
const 関数名 = function() {
    ~
};
```
アロー関数を用いると下記になる
```
const 関数名 = () => {
    ~
};
```
function()が()=>に置き換わっている
## コンストラクタ(ES6)
クラスにはコンストラクタと呼ばれる機能が用意されています。
コンストラクタはインスタンスを生成するときに実行したい処理や設定を追加するための機能です。
```
class クラス名 {
    constructor() {
        ~
    }
}
```
### プロパティ
コンストラクタの中で「this.プロパティ名 = 値」とすることで、生成されたインスタンスにプロパティと値を追加することができます。
```
~
constructor() {
    this.name = '名前';
}
~
```
### クラス内の呼び出し
クラス内でプロパティやメソッドを使用する場合、thisを使う
## エクスポートとインポート(ES6)
エクスポートやインポートできるのはクラスだけではない。
文字列や数値や関数など、どんな値でもエクスポートが可能。
```
export default 'クラス名など';
```
```
import クラス名など from './ファイル名';
```
*export defaultは一つしかエクスポートできない。*
### 名前つきエクスポート
この方法を使うことで複数のエクスポートが可能
```
export {名前1,名前2,~};
import {名前1,名前2,~} from './ファイル名';
```
### パッケージのimport
```
import 定数名 from 'パッケージ名';
```
## 配列操作(ES6)
### push
配列の後ろに値を追加する
```
配列.push(値);
```
### forEach
配列から一つずつ取り出す
```
配列.forEach((インデックス) => {
    処理;
});
```
見ての通りアロー関数が使用されている
また、この部分をコールバック関数という（らしい）
### find
条件式に合う1つ目の要素を配列の中から取り出すメソッド
```
const 定数名 = 配列.find((インデックス) => {
    return 条件式;
});
```
### filter
記述した条件式に合う要素のみを取り出して新しい配列を作成するメソッド
```
const 定数名 = 配列.filter((インデックス) => {
    return 条件式;
});
```
### map
配列の全ての要素に同じ処理をして新しい配列を作るメソッド
```
const 定数名 = 配列.map((インデックス) => {
    return 処理;
});
```
## コールバック関数(ES6)
JavaScriptでは引数に関数を渡すことができる
引数に渡される関数をコールバック関数と呼ぶ
```
const 定数名 = (引数名) => {
    処理
};
定数名(関数名);
定数名が処理されると関数名が引数名に渡される
```
