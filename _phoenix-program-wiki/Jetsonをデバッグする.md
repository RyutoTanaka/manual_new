---
layout: manual
title: Jetsonをデバッグする
category: phoenix-wiki
---

## 作業手順

### 1. ディレクトリの移動
phoenix-programがあるディレクトリに移動し、更にJetsonに移動する。

### 2. デバッグするノードを設定する
以下のコマンドを実行して、指示に従って入力を行います。

`bash scripts/debug_set.bash`

ノードのデバッグを終了する際にももう一度実行して、最後に"n"を入力してください

### 3. ログデータの表示&保存

以下のコマンドを実行すると1で設定したノードのデバッグログがコンソールに表示されます。


`bash scripts/print_debug.bash`

表示されるデータを保存するには以下のコマンドを実行する。

`bash scripts/print_debug.bash > savedir/savefile.txt`