
# RaspberryPi

# 参考URL

http://www.tapun.net/raspi/raspi-os-setup


>Raspberry Pi（ラズベリー パイ）は、ARMプロセッサを搭載したシングルボードコンピュータ。
>イギリスのラズベリーパイ財団 (Raspberry Pi Foundation) によって開発されている。

>単体でも汎用コンピュータとして動作するスペックを持っており、
>インターネットを介して様々な情報の伝播も可能なため、
>センサーが拾った情報を遠隔地で取得したり、
>接続したディスプレイに表示させる内容をスマートフォンから制御したりといったことも可能な、
>「モノのインターネット(Internet of Things、以下IoT)」の基となる製品。


## OSインストールの方法

RaspberryPiはコンピュータなので、
RaspberryPiで何かするにはRaspberryPi用のOSインストールが必要。

OSをインストールするには2通りある。

1. Noobsというインストーラ経由で、OSをインストールする
2. OSを直接インストールする


#### NOOBS
NoobsはRaspberry Pi用のOS"インストーラ"。 
https://www.raspberrypi.org/downloads/noobs/

Noobsは“New Out Of the Box Software”の略です。“Out of thebox”は
「枠を超える」という意味、（OSの）枠を超えたソフトウェアという意味合い。

RaspbianをはじめとするいろいろなOSを、
クリックだけでRaspberry Piにインストールすることができるようになります。

#### Raspbian
Raspberry Piの最も一般的なOS。
https://www.raspberrypi.org/downloads/raspbian/

#### その他のOS
Pidora,RISC OS,RaspBMC,Arch,OpenELEC


# OS インストール

### 1. 公式サイトからOSをダウンロード。

1-1. 「Raspbian Jessie」のZIPファイルを選択  
https://www.raspberrypi.org/downloads/raspbian/  
1-2. ダウンロード後、zipを解凍、imgファイルがでてくる  

### 2. microSDカードをフォーマット
  
2-0. SDカードをMacに挿入  
2-1. アプリケーション  
2-2. ユーティリティ  
2-3. ディスクユーティリティ  
2-4. 挿入したSDカードを選択  
2-5. 消去  
2-6. フォーマットを MS-DOS(FAT)にして消去実行  

### 3. microSDカードのフォーマット確認とBSD名の確認
3-1. appleメニュー  
3-2. このMacについて  
3-3. システムレポート  
3-4. ハードウェア -> USB -> 挿入したSDカードを選択  
3-5. BSD名(diskX)のX部分をメモ  

### 4. img ファイルをmicroSDに配置

ターミナルで下記実行
```
sudo dd bs=1m if=path_of_your_image.img of=/dev/rdiskn
```

SDカードを取り出し
```
sudo diskutil eject /dev/rdisk3
```










