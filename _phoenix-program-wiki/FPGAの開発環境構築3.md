---
layout: manual
title: FPGAの環境開発構築3
category: phoenix-wiki
---
# 1. はじめに
"FPGAを開発する"とは大きく分けて以下の2つになります。
 1. ハードウェア記述言語(HDL)でFPGA内のディジタル回路を開発する
 2. C++でFPGA内のソフトコア・プロセッサ(今回の場合はNios® Ⅱ)を開発する

現在使用しているMain回路にはIntel社のcyclone 10 lpが搭載されているため、これらの開発にはIntel社の[Quartus Prime](https://fpgasoftware.intel.com/?edition=lite)およびそれの周辺ソフトウェアが必要となります。

このページではFPGAへの書き込み作業について説明しています。install作業については [FPGAの開発環境構築1]を参照してください。環境構築作業については [FPGAの開発環境構築2]を参照してください。

## 以下のマニュアルは未検証です。手順が間違っている可能性があります。
## 写真が不十分です

# 4. 書き込み
## 4.1 USB-blaster 
USB blasterを用意します。

USB blasterが認識されない場合は
[ここ](https://www.macnica.co.jp/business/semiconductor/articles/intel/130517/)
を参考に作業してみてください。

USB blasterはある型番だとと書き込めない場合があります。
（しかし、その型番が不明になってしまいました）

## 4.2 jicファイルの生成

FPGAの開発環境構築2の最後にQuartus prime でbuildを行いました。

buildを行うと、`phoenix-program/FPGA/output_files`に`Phoenix_FPGA.sof`というファイルが生成されます。このファイルをFPGAに書き込むことで、FPGAにプログラムを書き込むことができます。

しかし、sofファイルは一度FPGAの電源が落ちると、データが消えてしまいます。

そこで、jicファイルと呼ばれるファイルを作成し、それをFPGA外部のEPCQ16というメモリーに書き込み、起動するたびにFPGAにプログラムを書き込むことで、この問題を解決します。

メニューバーからfile->Convert Programming File を選択します。

するとウィンドウが開くので、以下の写真と同じように編集します。

最後に右下のGenarateを押すと、jicファイルが生成されます。

その後、Save Conversion Setupをクリックして
phoenix_FPGA.cofファイルを生成します。

以降、jicファイルを作成する時はOpen Conversion Setup Dataからこのファイルを読み込むことで自動で設定を行ってくれます。

## 4.3基盤への書き込み

USB-blasterを基盤に以下の写真のように差し込みます。
その後、Tools->Programmer または以下のアイコンをクリックします。

開いたウィンドウからHardware setup を選択し `USB-Blaster [USB-0]`を選択します。
ここで候補にUSB-Blasterがない場合は正しく認識できていないです。

### sofファイルの書き込み
左側にあるadd File からphoenix_FPGA.sofを選択し、
start ボタンを押して書き込みます。

注意点
- sofを書き込んだ場合は、基盤の電源が一度切れてしまうと、プログラムが以前jicで書き込まれたものに上書きされてしまいます。

### jicファイルの書き込み
左側にあるadd File からphoenix_FPGA.sofを選択し、
以下の場所にチェックを入れます。
その後startボタンを押して書き込みます。

注意点
- jicを書き込んだ場合は、一度基盤の電源を落としてから再起動を行う必要があります。

## NiosⅡ SBTによるnios2プログラムの変更

Quartusでは一度のbuildに訳5分かかります。もし変更がnios2だけなのであれば、HDLで記述されたハードウェア側に変更が生まれないため、フルビルドは必要ありません。

まず、nios2 SBTを開き以下のボタンをクリックします。

project name を選択し


～～～

最後にrun を押すことでプログラムを書き込むことができます。

注意点
- Jetsonから速度指令を送っている状態だと書き込みに失敗します。
- 基盤の電源を落とすとプログラムは消えます。
















