---
layout: manual
title: controllerデバッグの方法
category: phoenix-wiki
---

# controllerを使用してデバッグをする方法

1. ロボットとssh接続します。

2. ドングルをロボットの外側のUSBポートに差します。


// todo 写真

3. 以下のコマンドを実行します
```
cd ~/Documents/phoenix-program/Jetson
. install/local_setup.bash
ros2 run kiks_tools old_controller ロボット番号
```
(同一ネットワーク上にいる場合は他のPCにドングルを差しても操作できます。)

4. 操作方法は以下のようになります。

// todo 写真
