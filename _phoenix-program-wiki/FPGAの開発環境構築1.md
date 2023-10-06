---
layout: manual
title: FPGAの環境開発構築1
category: phoenix-wiki
---

# 1. はじめに
"FPGAを開発する"とは大きく分けて以下の2つになります。
 1. ハードウェア記述言語(HDL)でFPGA内のディジタル回路を開発する
 2. C++でFPGA内のソフトコア・プロセッサ(今回の場合はNios® Ⅱ)を開発する

現在使用しているMain回路にはIntel社のcyclone 10 lpが搭載されているため、これらの開発にはIntel社の[Quartus Prime](https://fpgasoftware.intel.com/?edition=lite)およびそれの周辺ソフトウェアが必要となります。

このページでは環境構築に必要なソフトウェアのインストール方法について説明しています。Clone後の作業については[[FPGAの開発環境構築2]] (工事中)を参照してください。書き込み作業については ~[[FPGAの開発環境構築3]]~ (工事中)を参照してください

# 2. FPGAの開発環境構築1
## 2.1. Quartus Primeのインストール
Quartus Primeをインストールします。<br>
Quartus Primeおよびその周辺ソフトウェアをダウンロードするためにはIntelのアカウントが必要となります。
### 2.1.0. アカウントの作成
アカウントを持っている場合はこの作業は飛ばしてもらって構いません。<br>
[ここ](https://www.intel.com/content/www/us/en/forms/basic-intel-registration.html)にアクセスしてアカウントを作ります。必要入力事項は`名前`、`メールアドレス`、`ユーザーネーム`、`パスワード`、`国/地域`、`仕事`（Other-Engineeringを選択すればOK)、`国/地域コード`、`電話番号`です。それらを入力した後は右下にある「次のステップ」をクリックし、「送信」を押します。その後、登録したメールアドレスからメールが来るため、そのメールにある通りリンクをクリックし、サインインすれば登録完了となります。

### 2.1.1. Quartus Primeのダウンロード
まずは[ここ](https://fpgasoftware.intel.com/?edition=lite)にアクセスします。<br>
`エディション`は無償のLiteを選択します。<br>
`バージョン`は好きなものを選択していただいて構いません。ただし、ここの解説では19.1以上を対象にしています。安定バージョンは18.1だそうですが、19.1以降から後に紹介するNios2 Software Build Tools for eclipseのインストール方法が異なります。今回は執筆時点でLiteエディションの中で最新の20.1.1を採用しています。

> <補足> 1.この`バージョン`は毎年新しいものがでます。2.Liteエディションの場合自動更新はないので、バージョンを更新するためには新しいバージョンをこのサイトからダウンロードしてインストールする必要がありそうです。

> <追記>上記リンクをクリックした後、左端のバーからインストールするものを絞り込めます![スクリーンショット (20)](https://user-images.githubusercontent.com/87122408/197147699-f5b11c92-1f6c-4646-99d6-9c608dae420f.png)

`オペレーティング・システム`は自分の環境にあったものを選択します。

> <補足> Quartus PrimeをMacOSで使用する場合はネット情報によるとLinux用のものをダウンロードすればよいみたいです **(未確認)**

下にスクロールし、「個別ファイル」を選択します。そして、一番上の **Quartus Prime (includes Nios II EDS)** と **ModelSim-Intel FPGA Edition (Includes Starter Edition)** をダウンロードします。<br>
次にDevicesの中から **Cyclone 10 LP device support** をダウンロードします。
![スクリーンショット 2022-10-21 172951](https://user-images.githubusercontent.com/87122408/197151352-89339fa1-bc1a-4d75-9f1d-465924c6f14b.jpg)

> <補足>今後ほかのデバイスを使いたいなどある場合はほかのデバイスもダウンロードします。これらの作業はインストール後でもできます。

次に「追加ソフトウェア」を選択し、**Quartus Prime Programmer and Tools**もダウンロードします。<br>
Quartus Prime Helpが欲しい場合はそれもダウンロードします（ここでは解説しません）。
![スクリーンショット 2022-10-21 173636](https://user-images.githubusercontent.com/87122408/197152177-3c40f756-6342-4408-9d4e-cdb5efc9728b.png)


これでダウンロードするものは以上です。ここまでで少なくても4つダウンロードするものがあるはずです。<br>
次にそれらをインストールしていきます。

### 2.1.2. Quartus Primeのインストール
Quartus Primeをインストールする前にwindowsのドライバー署名を無効化する必要があります。

> 設定→更新とセキュリティ→回復→PCの起動をカスタマイズする→今すぐ再起動

を押して再起動をします。すると青色のオプションの選択という画面が開くので

> トラブルシューティング→詳細オプション→スタートアップ設定
→再起動→"ドライバー署名の強制を無効にする"に対応した番号キー（７？）

を順に押していきます。<br>
するとwindowsが起動するので以下の方法でインストールを始めます。

まずは`QuartusLiteSetup`を実行します。基本的に、すべてNextで構いませんが、Select Componentsで下の写真のように`Cyclone 10 LP`と`ModelSim - Intel FPGA Starter Edition(Free)`にチェックが入っていて、`ModelSim - Intel FPGA Edition`にチェックが入っていないことを確認してください。`ModelSim - Intel FPGA Edition`は有料です。

![Quartus_Prime_注意1](https://user-images.githubusercontent.com/51772315/103536571-22475480-4ed6-11eb-8734-c8af101344ac.png)

Quartus Primeのインストールが終了すると書き込み用のUSB Blasterのインストールが始まります。これも同じようにインストールしてください。<br>
windowsでドライバー署名の強制が無効になっている場合はここで赤い警告画面が出ますので、ソフトウェアをインストールするを選択します。<br>
> <補足>もし赤い警告が出ずに、インストールが失敗したという表示が出たら、もう一度上にあるwindowsの起動オプションを試してみてください。


その後、`ModelSimSetup`と`QuartusProgrammerSetup`も同じように実行してインストールします。ModelSimをインストールする際も`ModelSim - Intel FPGA Starter Edition`を選択することに注意してください。

![Quartus_Prime_注意2](https://user-images.githubusercontent.com/51772315/103537259-743caa00-4ed7-11eb-88c9-d36eac88078e.png)

## 2.2. Nios II Software Build Tools for eclipseの構築
Nios II Software Build Tools for eclipse(以下、SBTと略す)はNios2を開発するために必要なソフトウェアです。おおもとはeclipseですが、これをNios2用にカスタムしたものを使用します。ここからの作業はQuartus Primeのバージョン19.1以降を対象としています。

このSBT自体は実はQuartus Primeと同時にインストールされています。しかし、このままでは起動することができません。というのも、19.1以前ではそれを起動するためのCygwinとEclipseが付属していないためです。また、19.1以降cygwinは廃止され、Windows Subsystem for linux(以下、WSLと略す)を使用する必要があります。<br>

### 2.2.1 WSLのインストール
[ここ](https://www.macnica.co.jp/business/semiconductor/articles/intel/133717/)を参考にしています。
未検証ですが、Linux環境では必要ないと思います。Windows環境に限定して説明します(Macは自分で頑張って調べてください)。<br>
また、この作業は筆者が昔に作業したため、バージョン19.1での作業の写真が出てきます。ご了承ください。

Microsoft StoreからUbuntu 20.04 LTSをダウンロードします。その他バージョンでも構いません。(下の写真はミスで18.04になっています)

![image](https://user-images.githubusercontent.com/51772315/103538229-2c1e8700-4ed9-11eb-906e-dfb688d289dc.png)

次にWindowsの設定を開き「アプリ」→「アプリと機能」の順に選択し、右側にある「プログラムと機能」を選択します。するとコントロールパネルの「プログラムと機能」が開きます。そこで、左側にある「Windowsの機能の有効化または無効化」を選択し、下の写真のようにWindows Subsysmtem for linuxにチェックを入れます。

![image](https://user-images.githubusercontent.com/51772315/103538462-96cfc280-4ed9-11eb-8b03-eae8209df9ba.png)

そして、WindowsのプログラムからWSLを起動します。すると`Installing, this may take a few minutes...`と表示されるのでしばらく待った後、User設定を行います。ここは普通にLinuxを入れるときに行うものと同じです。

その後以下のコマンドを実行します。
```
$sudo apt update
$sudo apt upgrade
$sudo apt install -y wsl dos2unix make
```
これがエラーなしで成功したのちに`Nios II Command Shell`をWindosのプログラムから起動します(このソフトウェアはデバック時によく使います)。その後、以下の写真のように正しく認識できていればOKです。

![無題の画像](https://user-images.githubusercontent.com/51772315/103539173-ecf13580-4eda-11eb-9483-1d83cadf9355.png)

### 2.2.2. Eclipseのインストール
[ここ](https://www.macnica.co.jp/business/semiconductor/articles/intel/133716/)を参考にしています。写真もここから使用しています。<br>
面倒な作業が続きます。 **写真を見ながら作業をしてください。**

[ここ](https://www.intel.com/content/www/us/en/support/programmable/articles/000086893.html)のダウンロード・リンクから CDT 8.8.1 をダウンロードします。<br>
![133716_img4](https://user-images.githubusercontent.com/87122408/197523737-d222c76c-0b41-4f09-a306-8e73497ef4f6.png)

ダウンロードしたファイル を <Quartus Prime インストール・ディレクトリー>/nios2eds/bin に展開します。<br>
展開後、eclipse フォルダーが表示されます。

![image](https://user-images.githubusercontent.com/51772315/103539546-a3551a80-4edb-11eb-9179-687ddfb9e0e9.png)

eclipse フォルダーの名前を eclipse_nios2 に変更します。<br>
eclipse_nios2_plugins.zipを bin フォルダー内に展開します。eclipse_nios2 のフォルダーが上書きされます。

![image](https://user-images.githubusercontent.com/51772315/103539664-c8e22400-4edb-11eb-867d-d109b0e74f4b.png)

eclipse_nios2 フォルダー内を確認します。plugin_customization.ini が生成され、以下の写真と同じであることを確認します。違う場合は正しく起動しません。上の作業でどこか間違えている可能性があります。

![image](https://user-images.githubusercontent.com/51772315/103539773-f8912c00-4edb-11eb-85af-a3f10ddd8fd5.png)

WindowsのプログラムからNios II SBT for eclipseを起動します。使用するプロジェクトは一先ずデフォルトで構いません。下の写真のようになっていたら成功です。

![image](https://user-images.githubusercontent.com/51772315/103539878-24acad00-4edc-11eb-887a-5381322a7193.png)

もし、上の写真のようにならない場合はどこかでミスしている可能性があります。

これでFPGAの開発に必要なソフトウェアはすべてインストールが終了しています。

この後の作業については[[FPGAの開発環境構築2]] (工事中)に続きます。