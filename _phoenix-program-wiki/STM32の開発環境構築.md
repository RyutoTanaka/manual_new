---
layout: manual
title: JSTM32の環境開発構築
category: phoenix-wiki
---
# 1. はじめに

STM32マイコンはphoenix基板には搭載されていませんが、kicker基板、extension基板に`STM32F303K8`が搭載されています。(2022年8月現在)<br>
KIKSではSTM32の開発ツールとして、STM32CubeIDEを使用しています。<br>
ここではSTM32CubeIDEの環境開発手順を説明します。

# 2. STM32CubeIDEのインストール

[ここ](https://www.st.com/ja/development-tools/stm32cubeide.html)
にアクセスしてSTM32CubeIDE-Winの最新バージョンを取得をクリックします。

![stm1](https://user-images.githubusercontent.com/87122408/201463044-e83e9168-418d-454a-ae67-e69ad911b561.png)

その後、名、氏、メールアドレスを入力してダウンロードを押します。

![stm3](https://user-images.githubusercontent.com/87122408/201463051-973a17e4-faca-4f6e-8667-2038c5542768.png)

しばらくすると、登録したメールアドレスに下のようなメールが届くので、Download now を押して.zipファイルをダウンロードします。

![stm4](https://user-images.githubusercontent.com/87122408/201463055-75236bd1-24c4-4141-a943-cb5816aea756.png)

ダウンロードした.zipファイルを解凍し、中に入っている.exeファイルを実行します。
下のような画面が開くのでインストーラーの指示に従ってインストールしてください。(すべて初期設定のまま)

![stm5](https://user-images.githubusercontent.com/87122408/201463066-e7e691e0-a662-453a-87ee-5d3930f7b471.jpg)

もし下のような確認が出たらインストールしないを選択してください。

![stm6](https://user-images.githubusercontent.com/87122408/201463069-97423d72-fbd6-43f9-a940-6f8ebe0f99b0.png)

これでSTM32CubeIDEのインストールは完了です。
スタートメニューにSTM32CubeIDEがあることを確認してください。

![stm7](https://user-images.githubusercontent.com/87122408/201463113-ed7008d0-56d8-49d7-bc9b-9907da8e610f.png)

# 2. phoenix-programの開発環境構築

## 2.0. clone
このリポジトリをcloneしてください。
```
git clone https://github.com/kiksworks/phoenix-program.git
```
<br>
## 2.1. projectの生成 
STM32CubeIDEを起動します。<br>
出てきたウィンドウでWorkspaceを`hoge/phoenix-program/STM32`に設定してLaunchを押します。<br>

![stm8](https://user-images.githubusercontent.com/87122408/201465573-aee83c2e-d6fc-4137-ba01-fc22a7b8ad6d.png)

下ような警告が出た場合はアクセスを許可を押してください

![stm9](https://user-images.githubusercontent.com/87122408/201465665-56b7d270-dba5-46ff-a92b-824ee5f27d87.png)

出てきた画面でStart new project from an existing STM32CUbeMX configuration file (.ioc)　を選択ししばらく待ちます。

![stm10](https://user-images.githubusercontent.com/87122408/201465685-6446b3f1-aca5-4f63-8ac0-b09a4c6ca8c4.png)

このようなウィンドウが開くのでFileにphoenix-program/STM32/Kicker/Kicker.iocを選択します

![stm11](https://user-images.githubusercontent.com/87122408/201465703-bce2b809-135e-43a9-85b3-2474cfb663e4.png)

finishを押して、出てきたwindowでyesを押していくと下のような画面が開きます

![stm12](https://user-images.githubusercontent.com/87122408/201465727-d93914b8-0c82-4c08-8b7c-0a205df232a3.png)

次にextensionのprojectを追加します

File->New->STM32 project from an existing STM32CUbeMX configuration file (.ioc)<br>
を押し、

![stm14](https://user-images.githubusercontent.com/87122408/201465754-bc8fbef7-7aa9-423a-8c56-94cc1b6ed872.png)

でてきたウィンドウでphoenix-program/STM32/Extension/Extension.iocを選択します<br>
![stm15](https://user-images.githubusercontent.com/87122408/201465761-0bfd60f9-1b20-4851-bfe4-696115e310a1.png)

finishを押して、出てきたwindowでyesを押していくとextensionのprojectが完成します<br>
下の図のようにKicerとExtensionの２つのプロジェクトができていることを確認してください。

![stm16 (2)](https://user-images.githubusercontent.com/87122408/201465989-e3943b7e-37d4-4478-a7af-8857204437ca.png)

最後にphoenix-programで`git checout .` コマンドを実行します。<br>

![stm17 (2)](https://user-images.githubusercontent.com/87122408/201466009-2ca1e37b-a912-4dc3-8269-25f2696efa35.png)

## 2.1. build

最後にプロジェクトごとにビルドを行います。<br>
ビルドしないほうのディレクトリを右クリックしてclose projectを押します。

![stm18](https://user-images.githubusercontent.com/87122408/201466020-2dee6c32-70f6-4ff6-8425-509247a81303.png)

その後、ツールバーの金槌マーク、またはCtrl + B を押してビルドします。

次に先ほどビルドしたプロジェクトをcloseして、closeプロジェクトを右クリックしてopen projectを押しopenしビルドします。

![stm19](https://user-images.githubusercontent.com/87122408/201466028-f399e7cf-4454-404e-a4cf-74ca2e06b4fe.png)

２つのプロジェクトでエラーが出なければ環境構築は成功です。

![stm20](https://user-images.githubusercontent.com/87122408/201466072-ecb227ec-b461-4d37-a245-ef2f8ec5a624.png)
