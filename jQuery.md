# jQuery/javascript
## jQueryの読み込み順

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

jQuery3では「outside ready → outside load → ready」の順番、
jQuery2では「outside ready → ready → outside load → load」の順番でconsoleが表示されました。

また、$(function)が非同期になったことで、$(function)内に$(window).on(‘load’, function);が入っていると実行されなくなっているようです。

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

    var webStore = function () {
         ~ 
    };
    webStorage();

localStorageとsessionStorageの二種類がある

使用例

sessionStorage.getItem(‘引数’);で値を取得する

sessionStorage.setItem(‘引数’,’値’);で値をセットする
