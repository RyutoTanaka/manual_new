---
layout: manual
title: FPGAの環境開発構築2
category: phoenix-wiki
---
# 1. はじめに
"FPGAを開発する"とは大きく分けて以下の2つになります。
 1. ハードウェア記述言語(HDL)でFPGA内のディジタル回路を開発する
 2. C++でFPGA内のソフトコア・プロセッサ(今回の場合はNios® Ⅱ)を開発する

現在使用しているMain回路にはIntel社のcyclone 10 lpが搭載されているため、これらの開発にはIntel社の[Quartus Prime](https://fpgasoftware.intel.com/?edition=lite)およびそれの周辺ソフトウェアが必要となります。

このページでは開発環境を構築の方法について説明しています。install作業については [[FPGAの開発環境構築1]]を参照してください。書き込み作業については ~[[FPGAの開発環境構築3]]~ (工事中)を参照してください。

# 3. FPGAの開発環境構築2
## 3.0. clone
このリポジトリをcloneしてください。
```
git clone https://github.com/kiksworks/phoenix-program.git
```

## 3.1. PlatformDesignerでの作業
まずは2.1でいれたQuartusを起動します。

![スクリーンショット 2021-02-17 155948](https://user-images.githubusercontent.com/51772315/108167621-4b940b00-7139-11eb-9f4a-2a647abebf30.jpg)

開いた後、画面中央のOpen Projectまたは、File→Open Project... (Ctrl + J)で3.0でクローンしたなかにある **Phoenix_FPGA.qpf** を開きます。

![2021-02-13 (2)](https://user-images.githubusercontent.com/51772315/108167677-623a6200-7139-11eb-9d1f-b74f60b1f838.png)

次にTools→Platform Designerまたは、画面上部のタブで同じマークのものをクリックしてPlatform Designerを開きます。
Platform Designerを開くと、どのファイルを開くか聞かれるので、control.qsys、motor.qsys、imu.qsysのいずれかを選択します(順番は関係ありません)。

![2021-02-13 (4)](https://user-images.githubusercontent.com/51772315/108167760-8ac25c00-7139-11eb-9bed-832c6986b87d.png)

![2021-02-13 (5)](https://user-images.githubusercontent.com/51772315/108167979-e2f95e00-7139-11eb-8cd5-695fc5d715d2.png)

その後、右下にあるGenerate HDL...を選択し、保存先をクローン先からphoenix-program/FPGA/**hdl**/ファイル名(拡張子なし)にします(以後、アドレスを説明する際はphoenix-programからのパスを基準とします)。Generationのウインドウではbsfファイルの出力や言語の設定ができますが、関係ないのでデフォルトで大丈夫です(ただし、今後変更した際には、そのほかのHDLファイルがSystem Verilogのため、Verilogで出力したほうのが無難なはず)。

この作業をすべての.qsysに対して行います。そして、phoenix-program/FPGAのディレクトリ内に、control.sopcinfoが生成されていることを確認してください。このファイルがNios II Software Build Tools for eclipseでの作業に必要となります。

## 3.2. Nios II Software Build Tools for eclipseでの作業
以下、SBTと略します。
### 3.2.1. プロジェクトの作成
SBTを起動します。起動した際に、ワークスペースのアドレスを聞かれますが、phoenix-program/FPGA/softwareを選択してください。クローンしたばかりの場合、中身はcontrol_niosおよびimu_niosしかないはずです。

![2021-02-13 (8)](https://user-images.githubusercontent.com/51772315/108169292-dc6be600-713b-11eb-9340-5aa70e463571.png)

![2021-02-13 (10)](https://user-images.githubusercontent.com/51772315/108169471-1b9a3700-713c-11eb-867a-f7fc77499820.png)

次にプロジェクトを作成します。File→New (Alt+Shift+N)→Nios II Application and BSP Templateを選択します。
そして、SOPC Information File nameは3.1で生成したcontrol.sopcinfoを選択します。CPU nameはniosとimu_niosの2種類ありますが、niosを選択した場合はProject nameをcontrol_niosに、imu_niosを選択した場合はimu_niosにしてください(下の図はimu_niosを選択した場合)。また、TemplatesはBlank Projectを選択してください。問題なければ、Finishして、もう片方のCPUについても生成してください。

![image](https://user-images.githubusercontent.com/51772315/108169860-a418d780-713c-11eb-85fe-017489bbf3b9.png)

生成すると、ディレクトリはcontrol_nios、control_nios_bsp、imu_nios、imu_nios_bspの4つになるはずです。このときに、4つのプロジェクトのプロパティを開き、Text file encodingをUTF-8にしておきます。

### 3.2.2. control_niosのビルド
control_nios_bspを右クリックし、Nios II→BSP Editor...を選択します。
MainタブのSettingでは下の画像の部分を修正します。

![2021-01-03 (3)](https://user-images.githubusercontent.com/51772315/108171354-c01d7880-713e-11eb-87a6-96ac1fa96f0f.png)
(1)

また、Advancedでは下のように修正します。

![2021-01-03 (4)](https://user-images.githubusercontent.com/51772315/108171469-e5aa8200-713e-11eb-815d-afcb855cc33c.png)
(2)

Driversのタブでは、enable_small_driverにチェックを入れます。
![2021-01-03 (5)](https://user-images.githubusercontent.com/51772315/108171534-01158d00-713f-11eb-833f-c616b2b27922.png)
(3)

Linker Scriptのタブでは、共有メモリの設定をします。まず、中段のLinker Memory Regionsで、shared_ramを追加します。Add...をクリックするとポップアップで出てくるウインドウ内のRegion Nameにshared_ramを、Region Sizeに256、Memory Deviceにdata_ram、Memory Offsetに0を入力します。その後、data_ramのSizeとOffsetをそれぞれ3840、256に編集します。
<br>
つぎに、Linker Section MappingsのAdd...をクリックし、Section Nameを.shared、Memory Regionをshared_ramと入力し、Addします。ここまでの作業をすると下の写真のようになるはずです。最後にGenerateを押したあと、Exitを押して終了します。

![2021-01-03 (7)](https://user-images.githubusercontent.com/51772315/108171612-1d192e80-713f-11eb-91b2-0b4f32951429.png)
(4)

その後、control_nios_bspのディレクトリを右クリック→Build Projectを選択し、ビルドします。その後、control_nios内のsourceを展開し、拡張子がcppのファイルのみ右クリック→Add to Nios II Buildを選択します。

また、control_nios内のMakefileの329行目付近にある
```
ifneq ($(SYS_LIB),)
APP_LDFLAGS += -msys-lib=$(call adjust-path-mixed,$(SYS_LIB))
endif
```
を
```
ifneq ($(SYS_LIB),)
APP_LDFLAGS += -msys-lib=$(SYS_LIB)
endif
```
に変更します(やらなくてもいいはずだけど...一応)。

その後、control_niosについても右クリック→Build Projectを選択し、ビルドします。エラーが出なければ成功です。
### 3.2.3. imu_niosのビルド
やることは3.2.2とほとんど一緒です。

imu_nios_bspを右クリックし、Nios II→BSP Editor...を選択します。
MainタブのSettingでは3.2.2の図(1)のように修正します。また、Advancedでは3.2.2の図(2)ように修正します。

Linker Scriptのタブでも同様に、中段のLinker Memory Regionsで、shared_ramを追加します。しかし、3.2.2とは違い、Region Sizeは44と入力します。その後、data_ramのSizeとOffsetをそれぞれ980、44に編集します。
<br>
つぎに、3.2.2と同様にLinker Section Mappingsに.sharedを追加します。最後にGenerateを押したあと、Exitを押して終了します。

その後、imu_nios_bspのディレクトリを右クリック→Build Projectを選択し、ビルドします。その後、imu_nios内のsourceを展開し、拡張子がcppのファイルのみ右クリック→Add to Nios II Buildを選択します。

また、imu_nios内のMakefileの114行目付近にある
```
APP_CFLAGS_OPTIMIZATION := -Oo
```
を
```
APP_CFLAGS_OPTIMIZATION := '-Os'
```
に変更します。

その後、imu_niosについても右クリック→Build Projectを選択し、ビルドします。エラーが出なければ成功です。
### 3.2.4. mem_initの生成
それぞれ、_bspがついていない方のディレクトリを右クリックし、Make Targets→Build... (Shift+F9)を選択します。その後、ポップアップしたウインドウのmem_init_generateを選択します。

## 3.3. Quartus Primeでの作業
Quartus Primeに戻って、Processing→Start Compilation (Ctrl+L)または、上のタブで、青色の▷を選択します。少し待った後、←下のTaskのCompile Designに緑色のチェックがつくはずです。


この後の作業については ~[[FPGAの開発環境構築3]]~ (工事中)に続きます。