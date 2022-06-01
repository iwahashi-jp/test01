# React
## 概要
ユーザーインタフェース構築のためのJavaScriptライブラリ。
React Nativeを使うとiOSとAndroid両方のスマホアプリが作れる。
## Reactのお約束
```
class App extends React.Componet {
    render() {
        ~
    }
}
```
## JSX
JavaScriptの構文に対する拡張である。
HTMLと外観が似ているが、多くの開発者がよく知っている構文を使用して、構造化されたコンポーネントを描画する方法を提供する。
Reactは通常、JSXを使用して書かれる。
```
複数の要素があるとエラーになる
return (
    <h1></h1>
    <h2></h2>
    <p></p>
);
```
```
上記の場合、divタグなどでまとめるとOK
return {
    <div>
        <h1></h1>
        <h2></h2>
        <p></p>
    </div>
}
```
### JSXのお約束
#### コメント
```
{/* コメント */}
```
#### 閉じタグ
HTMLでは閉じタグが不要な要素でもJSXでは必要となる。
具体的には下記の通り
```
<img src="" />
```
また、textareaには閉じタグが必要だが、JSXでは
```
<textarea />
```
となる。
#### クラス名
JSXはHTMLと似たような構文だがクラスの指定方法が違う。
```
return (
    <div>
        ×<h1 class='title>Hello World</h1>
        ○<h1 className='title'>Hello World</h1>
    </div>
);
```
### App.jsの構成
```
import React from 'react';
class App extends React.Component {
    render() {
        // return外ならJavaScriptの記述も可能、コメント様式も同様
        const text = "Hello World";
        const imgUrl ="https://~.png"
        return(
            {/* 通常、return内はJSXのみ記述可能 */}
            {/* {}で囲むことでJavaScriptも記述できる */}
            <div>
                <div>{ text }</div>
                <img src= { imgUrl } />
            </div>
        );
    }
}
export default App;
```
### イベントの書き方
ES6のアロー関数を使用する。
```
<button イベント名={() => {処理}}></button>
```
#### イベント名
|イベント名|記述
|--|--
|クリック|onClick

### state
ユーザーの動きに合わせて変わる値のことをstateと呼ぶ。
#### stateの定義
```
constructor(props) {    // お約束
    super(props);       // お約束
    this.state = {プロパティ:値};
}
```
#### stateの表示
```
this.state.プロパティ
```
#### stateの変更
```
this.setState({プロパティ:変更する値})
```
また、Reactにはstateの値を直接代入してはいけない決まりがある
```
×
this.state = {プロパティ:値};
this.state.プロパティ = 値;
```
###　クラスの定義(ES6)
```
class クラス名 {
    constructor(){
        ~
    }
    メソッド名(){
        ~
    }
}
```
## app.jsとindex.jsの関係
index.jsに下記のように記述することでApp.jsのJSXがHTMLに変換される。
```
import React from 'react';
import ReactDOM from 'react-dom';
import App from './components/App';
ReactDOM.render(<App />,document.getElementById('root'));

```
## コンポーネント(ES6)
部品やパーツという意味で見た目を機能ごとにコンポーネント化して、組み合わせることでWebサイトの見た目を作ります。
```
export default クラス名;
```
### インポート(ES6)
```
import クラス名 from './ファイル名';
```
### コンポーネントの呼び出し
```
<コンポーネント名 />
```
### 値の引渡し
App.jsからコンポーネントに値を渡すにはpropsを使う
```
<コンポーネント名
    props名='値'
    props名='値'
/>
```
#### propsの利用方法
```
this.props
```
また、this.propsは{props名:値}というオブジェクトになる。
