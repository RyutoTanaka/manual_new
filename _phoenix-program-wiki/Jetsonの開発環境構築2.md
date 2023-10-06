---
layout: manual
title: Jetsonの環境開発構築2
category: phoenix-wiki
---
# 2. Jetsonの開発環境構築2(phoenix-programの実行)

## 2.1. phoenix-programのclone

## 2.2. spi設定([参考](https://sciencompass.com/electrical_work/jetson-nano/spi-set))
以下のコマンドを実行します。  

`sudo /opt/nvidia/jetson-io/jetson-io.py`  

pythonスクリプトが実行されるので以下の順に設定を行います。
1. Configure Jetson for compatible hardwareを選択
2. spi1、spi2にチェックを入れBackを選択
3. 設定を反映しリブートを選択

## 2.3. phoenix-programのinstall
適当なディレクトリ(Documets等)に移動して、[コード](https://github.com/kiksworks/phoenix-program.git)をcloneします。  
cloneができたらphoenix-program/Jetsonに移動して、  

    . install.bash

    kiks setup

を実行します。表示に従って入力を行います。
エラーが出た場合は一度ターミナルを閉じてからもう一度実行してください。

以上でJetson Nanoのセットアップは終わりです。