# Dockerの調査 メモ
## インストールについて
Pythonの書籍をやるにあたってメモを取る前にDocker,DockerDesktopなどをインストールしてしまっていた為、Dockerのインストール周りは割愛
* Dockerはターミナルから、DesktopはWebサイトからダウンロード、インストール出来たはず
## 注意事項
+ コンテナのOSにはLinuxしか利用できない
+ M1Macのプラットフォームはarm64、mysqlなどの多くはamd64をサポート。この違いで泣かされたので結構、重要
## Docker Hub
あらかじめよく使われる便利なイメージが登録されています。
ここからイメージを取得して、それをベースに改造する形を取るのが一般的な利用方法になります。
取得可能なイメージは Explore Official Repositories で検索できます。
## VSCode の拡張機能でリモートでコーティングできる
+ メモをし損ねて肝心の拡張機能名が分からない
+ ただしローカルで出来ることができなかったりする
（ファイル、フォルダの新規作成など）
## Docker Desktopについて
+ Macだとアプリを再起動しようとしても出来ない
（プロセスが残っておりアクティブモニターなどでそれを停止させる必要がある）
## linuxの管理者権限を一時的に付与する
```
sudo [コマンド]
```
## コマンドラインで十字キーやTabキーが使えない
シェルをbashに切り替える
```
bash
```
## wordpressのインストールまでの道のり
### ~~XAMPPのインストール~~
https://style.potepan.com/articles/19086.html
### ~~続いてWordpressのインストール（GUIインターフェースが必要だったので行き詰まり）~~
https://webkaru.net/linux/wordpress-install-centos/
### ~~→DockerはGUIインターフェースが使えるのか~~
https://atsblog.org/docker-gui-ubuntu/
ブラウザ経由でGUIを操作するので操作性が悪い
### ~~→CUIでwordpressはインストールできるのか~~
+ ~~WP-CLIなるものがあるらしい→Wordpressインストール後、ブラウザを使わずにプラグインなどをインストールするアプリケーションのようなので放置~~
### 『結論』Wordpressのコンテナイメージを使う
ほぼ手順通り
* https://docs.docker.jp/compose/wordpress.html

下記サイトを参考にdocker-compose.ymlを修正する
* https://zenn.dev/marumarumeruru/articles/55173a98863d4e
### wordpressのインストール先
/var/www/html
## まとめ
### メリットなど
* 今使っているPCの中身を汚さず開発出来る
* 使いたいコンテナがDocker Hubにあれば開発環境を整えるのが早い
* (試してないが)Dockerさえ入っていれば開発環境を本番環境などに移行できる
* 動作自体は軽そう
* バインドマウントを使えばホスト⇄コンテナ間のファイル共有が出来る、もとい使わないとかなり開発しづらいと思われる
バインドマウントについてこちらを参考→https://qiita.com/sayama0402/items/0f77861e059b38ea547a
### デメリット
* GUIが使えない(使えるコンテナもあったがGUIに特化した環境構築がされている為、その他諸々インストールする羽目になり手間が増える)
* OSがLinux限定、コンマンドラインに慣れる必要がある
* Dockerの知識が必要
* (推測)ローカルでは気にならないが本番を想定した場合、アクセスが集中した際のレスポンスは流石に下がるのではないかと思われる。