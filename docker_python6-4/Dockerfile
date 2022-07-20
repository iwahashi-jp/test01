FROM python:3.5.5
USER root

WORKDIR /app

RUN apt update; apt -y upgrade
RUN apt install mecab -y
RUN apt install libmecab-dev
RUN apt install mecab-ipadic-utf8 -y

RUN git clone --depth 1 https://github.com/neologd/mecab-ipadic-neologd.git
RUN apt install git make curl xz-utils file

RUN pip install --upgrade pip
RUN pip install tensorflow==1.5.0
RUN pip install keras==2.1.4
RUN pip install scikit-learn
RUN pip install pandas
RUN pip install matplotlib
RUN pip install gensim
RUN pip install mecab-python3
RUN pip install opencv-python
RUN pip install h5py
RUN pip install flask

# 上手く自動化が出来なかったのでコンテナが実行されたら実行する
# /app/mecab-ipadic-neologd/bin/install-mecab-ipadic-neologd -n -p /var/lib/mecab/dic/mecab-ipadic-neologd
