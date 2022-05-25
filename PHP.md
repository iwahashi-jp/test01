# PHP/Wordpress

## デバッグ
```
<?php var_export(値); ?>
<?php var_dump(値); ?>
```
## カテゴリー別のPHPファイル
名前にcategory-‘カテゴリー名’.phpでカテゴリー別に作れる

詳しくは
https://wpdocs.osdn.jp/%E3%83%86%E3%83%B3%E3%83%97%E3%83%AC%E3%83%BC%E3%83%88%E9%9A%8E%E5%B1%A4

## contact form 7
[response]→応答メッセージボックスを好きな位置に設置する

## URLetcの取得 
```
get_template_directory_uri();
home_url();
```
詳しくは
https://blog-and-destroy.com/22257

## 曜日取得について(Wordpress)
get_the_dateには翻訳機能がついており、日本語で曜日が取得される
英語の方が好ましい場合、get_post_timeを使うといい

## 三項演算子について
基本構文
```
(条件式) ? (真式) : (偽式);
```
また、echoを使うとエラーになるのでprintを使う