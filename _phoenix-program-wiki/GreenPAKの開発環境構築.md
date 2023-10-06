---
layout: manual
title: GreenPAKの環境開発構築
category: phoenix-wiki
---

# 1. はじめに
GreenPAKは[Dialog](https://www.dialog-semiconductor.com/)のもので、NVMプログラマブルデバイスです。GreenPAK Developer Kitを用いることで、カスタム回路の作成、プログラムをすることができます。主な用途として、電源シーケンス制御で、Main基板ではこの用途として使用しています。<br>
このGreenPAKを開発するためには[GreenPAK Designer](https://www.dialog-semiconductor.com/greenpak-designer-software)が必要です。このソフトウェアはコードを書くわけではなく回路図エディタのように開発します。

基本的にこの素子のコードを変更することはないと思うが、一応書いておきます。

この回路の設計者のページである[ここ](https://github.com/Nkyoku/phoenix-pcb/wiki/Programming-SLG46826G)と[ここ](https://blankalilio.blogspot.com/2020/11/phoenix-sequence.html)はぜひ読んでほしいです。

# 2. GreenPAK Designerのインストール
## 2.1. GreenPAK Designerのダウンロード
[ここ](https://www.dialog-semiconductor.com/greenpak-designer-software)から必要事項を入力し、ダウンロードします。必要入力事項は`名前`、`会社`(StudentでOK)、`国`、`住所`、`メールアドレス`、`ダウンロードするバージョン(OS)`です。自分の環境にあったものを選択してください。
## 2.2. GreenPAK Designerのインストール
ダウンロードしたものを解凍し、`setup`を実行します。「Install」を押せば結構です。

# 3. 起動
2.2.でいろいろインストールしましたが、使うのはその中のGreenPAK6 Designerです。<br>
GreenPAK6 DesignerをWindowsのプログラムから起動すると、Set chip revisionを聞かれますが、適当にOKで大丈夫です。<br>
その後、「Open」からcloneしたリポジトリの中で起動したいものを選択し、起動します。<br>

![image](https://user-images.githubusercontent.com/51772315/103542738-b9b1a500-4ee0-11eb-8997-6b35b7e6feb2.png)

# 4. Main基板への書き込み
<工事中><br>
[参考元](https://github.com/Nkyoku/phoenix-pcb/wiki/Programming-SLG46826G)