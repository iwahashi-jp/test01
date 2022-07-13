# Dockerの調査 メモ
## 注意事項
* コンテナのOSにはLinuxしか利用できない
* M1MacのCPUアーキテクチャはarm64、mysqlなどはamd64をサポート。これを意識していないと上手く動作しない。
* Docker Desktopは商用利用に制限あり
## Docker Desktop のインストール方法
* https://matsuand.github.io/docs.docker.jp.onthefly/desktop/mac/install/

M1MacはAppleチップのMacなのでそちらをインストールする
## Docker Hub
様々なイメージがあり、ここから取得して環境を整えるのが一般的

取得可能なイメージは Explore Official Repositories で検索できる
## VSCode の拡張機能でリモートでコーティングできる
+ ただしローカルで出来ることができなかったりするので利便性は低い（ファイル、フォルダの新規作成など）
## Linuxのコマンド
### 管理者権限を一時的に付与する
```
sudo [コマンド]
```
### コマンドラインで十字キーやTabキーが使えない
シェルをbashに切り替える
```
bash
```
## Wordpressのインストールまでの道のり
### 1. ~~ubuntuのイメージを使ってXAMPPをインストール~~
https://style.potepan.com/articles/19086.html
### 2. ~~続いてWordpressのインストール（GUIインターフェースが必要だったので行き詰まり）~~
https://webkaru.net/linux/wordpress-install-centos/
### 2.1. ~~→DockerはGUIインターフェースが使えるのか~~
https://atsblog.org/docker-gui-ubuntu/
ブラウザ経由でGUIを操作するので操作性が悪い
### 2.2. ~~→CUIでwordpressはインストールできるのか~~
+ ~~WP-CLIなるものがあるらしい→Wordpressインストール後、ブラウザを使わずにプラグインなどをインストールするアプリケーションのようなので放置~~
### 3. 『結論』 Wordpressのイメージを使う(CPUアーキテクチャの絡みで失敗していたので後回しになっていた)
下記サイトの手順通り(プロジェクトの構築を実行する前に3.1.のdocker-compose.ymlを修正すること)
* https://docs.docker.jp/compose/wordpress.html
### 3.1. docker-compose.ymlを修正する
### 3.1.1. CPUアーキテクチャの変更
db:の項目に下記を追加する
```
platform: linux/amd64
```
[参考サイト]https://zenn.dev/marumarumeruru/articles/55173a98863d4e
### 3.1.2. バインドマウントの方法（ホストコンテナ間のファイル共有設定）
例)wordpress:の項目に下記を追加する
```
volumes:
    - ./themes:/var/www/html/wp-content/themes
```
[参考サイト]https://qiita.com/sayama0402/items/0f77861e059b38ea547a
### 3.1.3. 同一イメージで複数コンテナを動かす方法（複数のWordpressサイトを同時に開発できる）
db:とwordpress:の項目に下記を追加し、wordpress:の項目にあるports:を被らないように変更する
```
container_name: "[固有の名前]"
```
[参考サイト]https://teratail.com/questions/312380?sort=3
### Linux版Wordpressのインストール先
```
/var/www/html
```
## まとめ
### メリットなど
* CPUアーキテクチャに左右されない環境構築が出来る（PythonでCPU問題にぶつかりまくったので個人的に大きい）
* 今使っているPCのHDDを汚さず開発出来る（Pythonでかなり汚してしまっているので個人的に大きい）
* 使いたいコンテナがDocker Hubにあれば開発環境を整えるのが早い
* (試してないが)Dockerさえ入っていれば開発環境を本番環境などに移行できる
* 動作自体は軽そう
* バインドマウントを使えばホスト⇄コンテナ間のファイル共有が出来る、もとい、使わないとかなり開発しづらいと思われる
### デメリット
* GUIが使えない(使えるイメージもあったがGUIに特化した環境構築がされている為、その他諸々インストールする羽目になり手間が増える)
* OSがLinux限定、コマンドラインに慣れる必要がある
* Dockerの知識が必要
* (推測)ローカルでは気にならないが本番を想定した場合、アクセスが集中した際などのレスポンスは落ちるのではないかと思われる。
# Dockerで書籍『PythonによるAI・機械学習・深層学習アプリの作り方』で使える環境構築をしてみる
## **Dockerイメージがサンプルのunsupportedに付属されていたのでこれを行う必要なはないかも？**
## jupyter lab 用のイメージを使う
https://kagakucafe.com/2022053118742.html
### rootパスワードが分からない為、sudoコマンドが使えない
Dockerの外からユーザー指定でLinuxコマンドが使えるコマンドがある
```
docker exec -it --user [ユーザ名] [コンテナID] [コマンド] [コマンドのオプション]
```
#### コンテナIDの調べ方
```
docker container ls
```
### MeCabのインストール
ホスト側
```
docker exec -it --user root e8abf469e18c apt update
docker exec -it --user root e8abf469e18c apt install mecab
docker exec -it --user root e8abf469e18c apt install libmecab-dev
docker exec -it --user root e8abf469e18c apt install mecab-ipadic-utf8
```
コンテナ側
```
git clone https://github.com/neologd/mecab-ipadic-neologd.git
```
上記gitコマンドが終了次第、ホスト側に戻って
```
docker exec -it --user root e8abf469e18c apt install git make curl xz-utils file
docker exec -it --user root e8abf469e18c /home/jovyan/mecab-ipadic-neologd/bin/install-mecab-ipadic-neologd -n -p /var/lib/mecab/dic/mecab-ipadic-neologd
```
* mecab-python3は後にインストールする
### pythonのバージョンが高い為、tensorflow　1系がインストール出来ない
[参考サイト]https://teratail.com/questions/282134
#### anacondaの仮想環境を使う
[参考サイト]https://qiita.com/aical/items/5d632a7ee4a3233bb8fc

コンテナ側で
```
conda create -n test python=3.5
conda activate test
pip install --upgrade tensorflow==1.5.0
* import時に警告が出るが動く
pip install --upgrade keras==2.1.4
pip install scikit-learn
pip install pandas
pip install matplotlib
pip install gensim
pip install mecab-python3
※ インストール中に盛大にエラーメッセージが出てるが Successfully installed とあるし、動作に問題はないっぽいので成功してる(と思う)
pip install opencv-python
pip install h5py
```
あらかたインストールが終わったら念の為、コンテナを再起動
#### anacondaの仮想環境をjupyter labのカーネルに登録する
[参考サイト]https://sakizo-blog.com/13/
```
pip install ipykernel
python -m ipykernel install --user --name=test
```
# 書籍:PythonによるAI･機械学習•深層学習アプリのつくり方で行き詰まった所や誤植、Colaboratoryで行う場合の修正箇所など
## p.146-148
```
from sklearn.externals import joblib
↓
Import joblib
```
## p.152
```
cnts = cv2.findContours(im2, cv2.RETR_LIST, cv2.CHAIN_APPROX_SIMPLE)[1]
↓
cnts = cv2.findContours(im2, cv2.RETR_LIST, cv2.CHAIN_APPROX_SIMPLE)[0]
```
## p.157
同上
## p.163
```
frame = cv2.resize(frame, (500, 300))
↓
frame = cv2.resize(frame, dsize=(500, 300))
```
## p.209
```
wiki.txt → wiki.wp2.txt
```
## p.210
```
size → vector_size
```
## p.392
tensorflow==1.5.0がないので2.5.0をインストール
+ 1.5.0 がないのはインストールしているPythonのバージョンが高い為
+ Colaboratoryにはtensorflow2系が入っている為、大幅な修正が必要
+ 環境を整える方が楽かと思われる
## p.289
```
keras.utils.np_utils.to_categoricalでなるっぽい
```
## ColaboratoryでRMSpropがimportできない問題
```
from tensorflow.keras import optimizers *importエラーが出てるが問題なく動く

[使用例]
optimizer=optimizers.RMSprop(),

```
## mkdirが通らない
中間フォルダも作成できるmakedirsを使う
## 
```
plt.plot(hist.history[“acc”]
↓
plt.plot(hist.history["accuracy"])
```
val_accも同様
## ColaboratoryでFlickrAPIのインストール
!pip install flickrapi
## Chap4のmarkov.pyが動かない
[参考サイト]https://github.com/SamuraiT/mecab-python3
```
    # markov.pv
    tagger = MeCab.Tagger("-d /var/lib/mecab/dic/mecab-ipadic-neologd")
    ↓ に変更
    tagger = MeCab.Tagger("-r /dev/null -d /var/lib/mecab/dic/mecab-ipadic-neologd")
```