---
layout: manual
title: Jetsonの開発(vscode)
category: phoenix-wiki
---
## 1. vscodeのセットアップ

## 1.1. vscodeのinstall
[ここ](https://code.visualstudio.com/download)からvscodeをinstallします。
設定はデフォルトで大丈夫です。

## 1.2. 拡張機能のインストール
左側のアイコンの下のほうにある拡張機能(extension)から
japanese
c++
remote-ssh
をインストールします。

## 1.3. リモート設定
左側のアイコンからリモート エクスプローラーを選択して，SSHターゲットタブにカーソルを合わせ設定ボタンを押します。  
画面上部に入力画面が出るので，C:ProgramData\ssh\ssh_configのような名前のものを選択します。  
選択したファイルが開かれるので，以下のように入力します。

入力例(10号機の場合)

Host kiks10  
&nbsp;&nbsp;HostName 192.168.11.110  
&nbsp;&nbsp;User kiks10  

入力できたら保存をします。

([参考](https://qiita.com/nlog2n2/items/1d1358f6913249f3e186))

## 2. JetsonNanoへ接続
sshターゲットタブの中から接続するJetsonを選択します。  
画面上部に入力画面が出るので，選択もしくは入力します。  
入力が終わったら左側のエクスプローラーアイコンからフォルダを開くを選択し，フォルダを開きます。  