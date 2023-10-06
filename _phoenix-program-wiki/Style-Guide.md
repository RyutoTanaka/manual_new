---
layout: manual
title: Style-Guide
category: phoenix-wiki
---

[ROS2 foxyのC++スタイルガイド](https://docs.ros.org/en/foxy/The-ROS2-Project/Contributing/Code-Style-Language-Versions.html#id1)を基にしています。
ROS2のスタイルガイドは[GoogleのC++スタイルガイド](https://google.github.io/styleguide/cppguide.html)を参考にしているので，合わせて確認してください。
万が一，下記スタイルガイドで触れられてないことがありましたら，上記リンクを参照してください。

## ClangFormat
コードの整形にClangFormatを導入しています。

## 省略表記
省略表記は以下の通りです。
- CamelCased: 各単語毎にその先頭の一文字を大文字にし，アンダースコアなしで接続したもの。
- snake_case: 全て小文字の単語をアンダースコアで接続したもの。

- ALL_CAPITALS: 全て大文字の単語をアンダースコアで接続したもの。

⚽ のマークをつけているものはこちらの独自ルールのため気をつけてください。

## 詳細
### クラス，関数，変数
<命名>
- クラス名，自作の型は **CamelCased** で，その名前で何を表すのかわかるようにします。
- `public:`，`private:`，`protected:`の前にスペースを入れません。
- 関数/メンバー関数は **snake_case** で，その名前はそれが何を行うかを明確にする。
- ⚽ [推奨]クラス名は名前，メンバ関数/関数名は動作になるようにします。
- 関数の定義，呼び出しで1行に収まりきらない場合は，`(`や`{`の後で改行し，スペース二つ分のインデントをいれます。
```
call_func(
  foo, bar, foo, bar, foo, bar, foo, bar, foo, bar, foo, bar, foo, bar, foo, bar, foo, bar,
  foo, bar, foo, bar, foo, bar, foo, bar, foo, bar, foo, bar, foo, bar, foo, bar, foo, bar);
```
- 変数/メンバー変数は **snake_case** で無理なく説明的な名前をつけます。
  - for文などに使用する反復用の変数は例外的に`i`,`j`,`k`を使っても大丈夫です。使う場合，それらは外側から順番に使用し，その変数のスコープはその反復処理内部にとどめてください。
  - 大域変数(グローバル変数）は基本的には使わないでください。どうしても使用する場合には接頭語`g_`を加えます。
  - メンバー変数には末尾にアンダースコアをつけます。
- 定数は **ALL_CAPITALS** にします。
```
for(int i=0;i<num;i++)
{
  for(int j=0;i<num;i++)
  {
     // something
  }
}
```

### 名前空間
- 名前空間は**snake_case**で命名します。
- ⚽ すべてのコードはkiks名前空間の中に書きます。
- ⚽ 名前空間とディレクトリは一致させる

### インクルードガード
- ⚽ インクルードガードはすべてのヘッダーファイルに使用し，`NAMESPACE_CLASS_HPP`のようにする。
```
#ifndef KIKS_HOGE_FUGA_HPP
// 例外(自動生成ファイルなど)
// #ifndef PACKAGE_PATH_FILE_H_
// #define PACKAGE_PATH_FILE_H_

// 空行を入れてから処理を書く。

namespace kiks::hoge
{

class fuga{
public:
  fuga();
  //something
}

} // namespace kiks::hoge

#endif
```

### 条件文，繰り返し文など
- if，else，do，while，forに続けて{}を常に使用します。
- if文内の条件で1行に収まりきらない場合は，`(`や`{`の後で改行し，スペース二つ分のインデントをいれます。
```
if(a < b){
 // something
}
else{
 // other
}
if (
  this && that || both && this && that || both && this && that || both && this && that)
{
  // something
}
// NG
// if(a < b)
//   x = 2*a
// if (this &&
//     that ||
//     both) {
//   // something
// }
```
### include順
- #includeの順番は以下の通り。ただし，グループごとにアルファベット順にソートします。
    1. ソースコードの場合，ソースコードに対応する同名のヘッダーファイル
    2. 空行
    3. `#include < >`を必要とするCシステムヘッダ(extern "C"が必要)
    4. `#include < >`を必要とするC++システムヘッダ
    5. 空行
    6. `#include " "`でincludeする他のライブラリ
    7. `#include " "`でincludeするライブラリ
- ⚽ C++のファイルはCファイル，Cライブラリのincludeをなるべく避ける。どうしてもincludeする必要がある場合は`extern "C"`で囲みます。

### その他
- コードの1行の長さは **100文字**
- コメントを入力する際は`//`や`/*`から1マス開けてから入力する。ファイルの説明などが目的の場合は`///`や`/** */`を使用する。
- using指令をヘッダーファイルで使用しない。また，ソースファイルで使用する際も意図した名前のみ引き出せるように使用します。
```
using namespace std; // NG
using std::list;  // OK, std::list を list として参照したい
using std::vector;  // OK, std::vector を vector として参照したい
```
- ⚽ ファイルの名前は **snake_case**，ソースファイルの拡張子は`.cpp`，ヘッダーファイルは`.hpp`にします。
 - ⚽ ただし例外として，STM32のソースファイルの拡張子は`.c`，ヘッダファイルは`.h`にします。（理由はSTM32のみ #27 の都合でCで開発するため。）
 - ⚽ ファイル名にスペースは含めません。
- ⚽ プリプロセッサマクロはどんな時も可能な限り使用を避ける。ただし自動生成のものは除く。
- ⚽ コンソール出力は以下の通りとする。
  - ⚽ ROS2を適応しているJetson側のプログラムは`RCLCPP_DEBUG`関数，`RCLCPP_INFO`関数，`RCLCPP_WARN`関数，`RCLCPP_ERROR`関数，`RCLCPP_FATAL`関数を使用する。
  - ⚽ [推奨]ROS2を適応しているJetson側のプログラムは`RCLCPP_DEBUG`を使用し，適宜`RCLCPP_WARN`，`RCLCPP_ERROR`，`RCLCPP_FATAL`を使用する。`RCLCPP_INFO`はできる限り使わない。(試合中に使用するデフォルトのレベルはinfo)
  - ⚽ それ以外のものについてはprintfまたはstd::coutを使用します。
- ⚽ インデントは空白2マスを使用します。