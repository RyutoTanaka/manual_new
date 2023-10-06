---
layout: manual
title: Jetsonの環境開発構築1
category: phoenix-wiki
---
# 1.  JetsonNanoの開発環境構築1(本体の準備)
JetsonNano自体の準備をします。  
JetsonNanoが使用できる状態であれば飛ばしてかまいません。  
[参考ページ](https://ja.osdn.net/projects/ttssh2/)
## 1.1.  OSのインストール
JetsonNanoにUbuntu18.04をインストールして，設定を行います。  
### 1.1.1. microSDカードのフォーマット
[ここ](https://www.sdcard.org/ja/downloads-2/formatter-2/)からSDカードフォーマッターをダウンロードしてインストールをします。  
インストールができたらPCにmicroSDカードを差し込み，フォーマットを行います。  
### 1.1.2. OSの書き込み  
[このサイト](https://developer.nvidia.com/embedded/downloads)からOSイメージファイル(約6GB)をダウンロードして解凍します([通常版ダウンロードリンク](https://developer.nvidia.com/embedded/l4t/r32_release_v7.1/jp_4.6.1_b110_sd_card/jeston_nano/jetson-nano-jp461-sd-card-image.zip))。  
次に[ここ](https://www.balena.io/etcher/)から書き込みソフトをダウンロード・インストールして，先ほどのOSイメージファイルをmicroSDカードに書き込みます。  
  
以上でOSの書き込みは完了です。


## 1.2. JetsonNanoセットアップ
### 1.2.1. 起動方法
イメージを書き込んだmicroSDカードをJetsonNano本体の裏面に差し込みます。  
次にデバイスを接続して，電源を入れます。

***

* 開発キットを使用する場合  
USBポートにマウス，キーボードを差し，HDMIポートにモニターを繋げます。  
付属のACアダプターを差し込むと電源が入ります。

* phoenix基板を使用する場合  
端のほうについているUSBtypeCポートにUSBハブを取り付け，USBポートにマウス，キーボードを差し，HDMIポートにモニターを繋げます。  
電源スイッチがオフなのを確認した後，USB-PDの機能を持った電源とUSBハブをUSBケーブルでつなぎ，電源スイッチをオンにします。(電源スイッチはオフにしてもJetsonNanoは起動したままです)

***

### 1.2.2.初回設定
画面にセットアップ設定が出るので手順に沿って進めます。  
この時  
>ユーザー名 : kiks<ロボット番号> (例:kiks9)   
>パスワード : soccer  
>言語　　　 : English  

にしてください。その後変更したければ言語を日本語にしてrebootします(注:フォルダ名は英語のままにしてください)。

### 1.2.4. SSH接続(TeraTerm)
使用しているPCからJetsonNanoにSSH接続を行います。  
[ここ](https://ja.osdn.net/projects/ttssh2/)からTeraTermをダウンロードしてインストールします。  
TeraTermを起動したら以下のように設定しOKを押します。

> TCP/IP : 選択する  
> ホスト : <JetsonNanoのIPアドレス>  
> ヒストリ : 基本的に選択する(選択すると次回起動時に設定が引き継がれる）  
> TCPポート : 22  
> サービス : SSH  
> SSHバージョン : SSH2  
> IPバージョン : AUTO か IPv4  
> シリアル : 選択しない  

接続が成功すればユーザー名等を入力する画面が出るので，入力してOKを押します。
もしその後警告画面が出ても問題ないのでOK等で次へ進んでください。
コマンドが入力できれば問題ありません。

### 1.2.5. SSH接続(VSCode)
これは必須ではありませんが，設定しておくとその後何かと楽です。  

VSCodeを開き，一番左のアイコンからリモートエクスプローラーを選択し，その中のSSH Targetsを選択します。
SSH TARGETSのタブが出たら，タブにカーソル合わせ，出てきた設定マークをクリックしssh_configを開きます。
開いたファイルに以下のように書き込みます

> Host <VSCodeに表示されるデバイス名>  
> &emsp;HostName <JetsonNanoのIPアドレス>  
> &emsp;User <JetsonNanoのユーザー名>  

9番機の例

> Host kiks9  
> &emsp;HostName 192.168.11.109 
> &emsp;User kiks9

管理者権限で保存すると，左側のタブにHostに設定した名前が表示されるので，カーソルを合わせてフォルダマークを選択するとSSH接続を開始する。  
パスワードの入力等手順道理進めれば接続完了です。

