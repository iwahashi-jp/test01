# Dockerでディープラーニングの学習環境を構築してみる
## ディープラーニング用のイメージでjupyter labを入れる（結論：失敗）
```
docker pull sonoisa/deep-learning-coding:pytorch1.9.0_tensorflow2.6.0
```
PullしたイメージをRunする。
コンテナ起動後、シェルを起動する。
aptのアップデート(しないとMeCabがインストール出来ない)
```
sudo apt update
```
### MeCabのインストール
https://qiita.com/kado_u/items/e736600f8d295afb8bd9
```
sudo apt install mecab
sudo apt install libmecab-dev
sudo apt install mecab-ipadic-utf8
```
mecab-ipadic-neologdのインストール(失敗)
```
git clone https://github.com/neologd/mecab-ipadic-neologd.git
cd mecab-ipadic-neologd
sudo bin/install-mecab-ipadic-neologd → 失敗:file is not found.
```
教本p.397を試してみる(成功)
```
sudo apt install git make curl xz-utils file
./bin/install-mecab-ipadic-neologd -n -p /var/lib/mecab/dic/mecab-ipadic-neologd
```
続いて教本p.398
```
pip3 install gensim
pip3 install mecab-python3
```
続いてjupyter labのインストール

https://www.orzs.tech/install-jupyter-lab-on-ubuntu/
```
pip3 install jupyterlab
jupyter lab → 起動はするもののアクセスできず
```

## jupyter lab 用のイメージをディープラーニング環境にしていく(今のところ成功)
https://kagakucafe.com/2022053118742.html

流石にJupiter labは起動する。
### rootパスワードが分からない為、sudoコマンドが使えない
Dockerの外からユーザー指定でLinuxコマンドが使えるコマンドがあった
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
docker exec -it --user root 834ff53b28f7 sudo apt update
docker exec -it --user root 834ff53b28f7 sudo apt install mecab
docker exec -it --user root 834ff53b28f7 sudo apt install libmecab-dev
docker exec -it --user root 834ff53b28f7 sudo apt install mecab-ipadic-utf8
```
* よくよく考えたらrootで実行しているのでsudoコマンドは要らないかも？

コンテナ側
```
git clone https://github.com/neologd/mecab-ipadic-neologd.git
```
ホスト側に戻って
```
docker exec -it --user root 834ff53b28f7 sudo apt install git make curl xz-utils file
docker exec -it --user root 834ff53b28f7 sudo /home/jovyan/mecab-ipadic-neologd/bin/install-mecab-ipadic-neologd -n -p /var/lib/mecab/dic/mecab-ipadic-neologd
```
コンテナ側に戻って
```
pip3 install gensim
pip3 install mecab-python3
```
**とりあえず、Python・MeCab・jupyter labのある環境構築は出来た。**
```
pip3 install opencv-python
```
### pythonのバージョンが高い為、tensorflow　1系がインストール出来ない
[参考サイト]https://teratail.com/questions/282134
#### ~~バージョンの変更を試みる(失敗)~~
[参考サイト]https://qiita.com/piyo_parfait/items/5abbe4bee2495a62acdc
```
docker exec -it --user root 834ff53b28f7 update-alternatives --install /usr/bin/python python /usr/bin/python3 1 →失敗
```
#### ~~python3.7をインストール後、バージョン変更を試みる(失敗)~~
```
docker exec -it --user root 834ff53b28f7 apt-get install python3.7 → インストールもアップグレードもダウングレードもされず失敗
```
#### ~~pyenvを使用してバージョン管理を試してみる(失敗)~~
[参考サイト]https://codeaid.jp/pyenv-linux/

コンテナ側
```
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile
echo 'eval "$(pyenv init --path)"' >> ~/.bash_profile
echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
source ~/.bash_profile
pyenv install 3.5.0 → インストールの途中でエラーも出ずに止まってしまう(解決する糸口がない為、別のアプローチを試す）
```
#### anacondaの仮想環境を使う
[参考サイト]https://qiita.com/aical/items/5d632a7ee4a3233bb8fc

anacondaとpipは基本的に互換性がないとどこかで見たのでcondaコマンドに置き換え
* condaとpip 両方試しているがcondaでインストールするとライブラリーが上手く入らないケースが多い気がするので結局pipを使うことに
```
conda create -n test python=3.5
conda activate test
conda install tensorflow==1.5.0
conda install keras → 書籍通りにバージョンを指定すると見つからない為、指定なしで試す
ここまでで一旦、動作チェックしてみたらimport tensorflowで盛大にエラーが出てるのに気づく
https://www.techpit.jp/courses/44/curriculums/47/sections/378/parts/1204
conda ではなく pip を使えとのこと
conda uninstall -n test tensorflow
pip install --upgrade tensorflow==1.5.0
pip install --upgrade keras==2.1.4
* import時に警告？が出るが動く
* python3.5をインストールしてしまっている為、anacondaなら入っているようなライブラリーが入っていないので手間が非常にかかることに気づく。
* ネックだった部分は大体インストール出来ているのであとは必要に応じて入れていけば大丈夫そう。
pip install scikit-learn
pip install pandas
pip install matplotlib
pip install gensim
pip install mecab-python3 → インストール中に盛大にエラーメッセージが出てるがSuccessfully installed mecab-python3-1.0.3と出てるので成功してるっぽい
pip install opencv-python
```
##### anacondaの仮想環境をjupyter labのカーネルに登録する
[参考サイト]https://sakizo-blog.com/13/
```
pip install ipykernel
python -m ipykernel install --user --name=test
```
### 教本のChap4のmarkov.pyが動かない
[参考サイト]https://github.com/SamuraiT/mecab-python3
```
    # markov.pv
    tagger = MeCab.Tagger("-d /var/lib/mecab/dic/mecab-ipadic-neologd")
    ↓ に変更
    tagger = MeCab.Tagger("-r /dev/null -d /var/lib/mecab/dic/mecab-ipadic-neologd")
```
