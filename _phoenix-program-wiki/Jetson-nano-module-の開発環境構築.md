---
layout: manual
title: Jetson-nano-moduleの環境開発構築
category: phoenix-wiki
---

# 1. はじめに
Jetson nano module は通常のjetsonと違いSDカードスロットがありません。
代わりに、16 GB eMMC 5.1が基板上に実装されています。
今回は、SDK Managerというソフトを用いてeMMcにOSをインストールし、Jetson nano moduleを使えるようにします。
[参考動画](https://www.youtube.com/watch?v=xE1Jk8hHjNE)
# 2. SDK Manager
注意点
*Jetson nano module を書き込むためには、仮想環境ではないUbuntu18.04の環境が必要です。
現在windowsを使用している場合はデュアルブートなどによりUbuntu18.04の環境を整える必要があります。
*ディスクの空き容量が少なくとも13GB以上必要です。
私の環境では100GB以上あったので、13GB以上あっても失敗する可能性があります。
*初回の書き込みには時間がかかります。
初めて書き込む場合はダウンロード、インストールなどに時間がかかると思われます。十分時間があるときに書き込みましょう。
## 2.1 SDK Managerのインストール
Ubuntuの環境が整ったら[このサイト](https://developer.nvidia.com/drive/sdk-manager)から .deb Ubuntuを押してファイルをダウンロードします。
![Screenshot from 2023-03-13 15-32-28](https://user-images.githubusercontent.com/87122408/224629048-a5e66b9f-42ca-4430-9744-01265cf1fc87.png)
ここで、NVIDIAのアカウント登録が必要です。指示に従ってアカウントを作成してください。
その後、```sudo apt install <ダンロードしたファイル.deb>```を実行しSDK Managerをインストールします。
## 2.2 JETPACKのダウンロード
先程インストールしたSDK Managerを起動します。
![Screenshot from 2023-03-13 15-53-59](https://user-images.githubusercontent.com/87122408/224629152-9699b99c-177b-470f-8d47-def7b1608884.png)
NVIDIA DEVELOPERのLOGINをクリックするとNVIDIAのWebサイトに飛ばされるので先ほと登録したアカウントでLOGINします。
その後、SDK managerの画面を以下のように変更します。(DeepStreamのチェックは外しておきます。)
![Screenshot from 2023-03-13 16-03-21](https://user-images.githubusercontent.com/87122408/224630311-965385a9-0ad5-48a8-acc6-26580c8fd0c6.png)
設定が完了したらCONTINUEを押します。
![Screenshot from 2023-03-13 16-06-37](https://user-images.githubusercontent.com/87122408/224638612-76246edb-2acc-4dc3-ac13-1253dc7ab2bd.png)
次に、左下の```I accept ...```にチェックを入れてCONTINUEを押します。
このときに、ディスクの空き容量が足りているかどうかを確認してください。
CONTINUEを押すとダウンロードとインストールが始まります。
## 2.3 Jetson Nano module の組み立て
ダウンロードとインストールを待っている間に、moduleの組み立てを行います。
使用していないdeveloperキットの土台とJetson nano module、ヒートシンク、(CPUグリス、Wifiカード＆アンテナ)を用意してください。
[この動画](https://www.youtube.com/watch?v=xE1Jk8hHjNE)の2:30以降を参考にして組み立てを行ってください。
CPUグリスを持っているならヒートシンクとCPUの間に塗りましょう。
また、ここでWifiカードをつけておくと、あとで分解せずにwifi設定ができます。
## 2.4 Jetson Nano Module に書き込み
ダウンロード開始からしばらく立つと、以下のようなwindowが開きます。
![Screenshot from 2023-03-13 16-30-18](https://user-images.githubusercontent.com/87122408/224635534-6bb6afb0-bfb3-4319-96c7-ca11e6ad295d.png)
このような状態になったらリカバリーモードのjetsonを接続します。
### 2.4.1 jetson をリカバリーモードで起動
Jetsonをリカバリーモードで起動する方法を説明します。
文字だけではわかりにくいので、[例の動画](https://www.youtube.com/watch?v=xE1Jk8hHjNE)の7:10以降を参考にするといいと思います。

必要なもの
* 組み立てたjetson nano module
* Jetson developer kit 付属のACアダプター（5V 4A）
* USB micro-Bケーブル
* ジャンパピン

はじめに、dev-kitの土台の印の部分がジャンパーピンで短絡されていることを確認してください。
次に、dev-kitの印の部分(FC-RECとGND)をジャンパーピンで短絡させてください。
それができたら、PCとJetsonをUSB micro-Bで繋いで、ACアダプターを繋いで電源を入れてください。
このときにUSBのPORT近くにあるLEDが光ることを確認してください。
電源を入れたあと、FC-RECにつけたジャンパーピンは抜きます。

この状態になると、PCの画面に下のようなウィンドウが出て来るので、Jetson nanoを選択します。
![Screenshot from 2023-03-13 17-16-44](https://user-images.githubusercontent.com/87122408/224660120-eed987b4-9089-42d9-bf6b-1161eecd66eb.png)
その後、```Automatic Setup - Jetson Nano```を```Manual Setup - Jetson Nano```に変更します。
そしてnew username に ```kiksロボット番号```を、new passwordに```soccer```を設定します。
![Screenshot from 2023-03-13 17-17-36](https://user-images.githubusercontent.com/87122408/224660127-9be01a6e-7c26-4419-bf5f-41947e710363.png)
あとはインストールが終了するまで放置します。この画面になれば書き込みは完了です。
![Screenshot from 2023-03-13 18-35-40](https://user-images.githubusercontent.com/87122408/224662958-a44f9892-844a-47a9-8b90-103585131ce0.png)

# 3. Jetsonの設定
Micro-Bケーブルを抜いて、モニター、キーボード、マウスを接続して、Jetsonが正常に起動できるかどうか確かめます。
正常に起動しなかった場合は、設定などを確認し、やり直してみてください。
## 3.1 wifi設定
正常に起動したならばWifi設定を行います。
インターネットが使えるお好みのwifi(有線でも可)に接続してください。
## 3.2 HostNameの変更
jetson-nano-moduleのHostNameをjetson-nanoのデフォルトに合わせて変更します。(以下は2番機の例)
```
sudo hostnamectl set-hostname kiks2-desktop
```
## 3.3 容量の確保
Jetson-nano-moduleには16 GB eMMC 5.1が基板上に実装されています。しかし、初期設定のままだと容量が足らずにROSがインストールできません。そこで、不必要なものを削除し空き容量を確保します。
[このページ](https://collabnix.com/easy-way-to-free-up-jetson-nano-sd-card-disk-space-by-40%ef%bf%bc%ef%bf%bc/)を参考に作業します。

以下のコードを順番に実行してください。
```
sudo apt update

sudo apt upgrade

sudo apt autoremove

sudo apt clean

sudo apt remove thunderbird libreoffice-* -y

sudo rm -rf /usr/local/cuda/samples \
/usr/src/cudnn_samples_* \
/usr/src/tensorrt/data \
/usr/src/tensorrt/samples \
/usr/share/visionworks* ~/VisionWorks-SFM*Samples \
/opt/nvidia/deepstream/deepstream*/samples

sudo apt purge cuda-repo-l4t-local libvisionworks-repo -y

sudo rm /etc/apt/sources.list.d/cuda*local /etc/apt/sources.list.d/visionworks*repo*

sudo rm -rf /usr/src/linux-headers-*
```
これで、開発を行うのに十分な空き容量を確保できます。

## 3.4　spiの設定
Jetson-nano-moduleは通常の方法でSPIの設定を行えません。
そこで、SPIの.dtbファイルを直接rootディレクトリにコピーして設定します。\
[このページ](https://forums.developer.nvidia.com/t/how-can-i-enable-spi-communication-on-jetson-nano-module/229228)
を参考に作業します。

[ここ](https://forums.developer.nvidia.com/uploads/short-url/pHU1lykUAqfxJqx3MMhPfxFY9hu.dtb)から```nano-spi.dtb```をダウンロードします。

ファイルをインストールしたディレクトリに移動し、次のコマンドを実行して、`nano-spi.dtb`をbootフォルダーにコピーします。
```
sudo cp nano-spi.dtb /boot/
```
次に、extlinux.confというファイルを変更します。
お好みのエディターでファイルを開きます。(ここではvim)
```
sudo vim /boot/extlinux/extlinux.conf
```
3行目の`DEFAULT primary`を`DEFAULT JetsonIO`に書き換えます。
そして以下の文をファイルに貼り付けます。
```
LABEL JetsonIO
MENU LABEL Custom Header Config: <HDR40 User Custom [2022-09-29-210441]>
LINUX /boot/Image
FDT /boot/nano-spi.dtb
INITRD /boot/initrd
APPEND ${cbootargs} Quiet root=/ dev/mmcblk0p1 rw rootwait rootfstype=ext4 console=ttyS0,115200n8 console=tty0 fbcon=map:0 net.ifnames=0
```
最終的な.confファイルの中身が以下のようになればOKです。

以下のコマンドで再起動して設定を反映させましょう。
```
sudo reboot now
```

# 4. おわりに
これでJetson-nano-module固有のセットアップは終了です。
お疲れさまでした。

次はこのページでjetsonの環境開発の続きを行ってください。