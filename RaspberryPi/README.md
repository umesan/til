
# Raspberry Pi

# 参考URL

http://www.tapun.net/raspi/raspi-os-setup


>Raspberry Pi（ラズベリー パイ）は、ARMプロセッサを搭載したシングルボードコンピュータ。
>イギリスのラズベリーパイ財団 (Raspberry Pi Foundation) によって開発されている。

>単体でも汎用コンピュータとして動作するスペックを持っており、
>インターネットを介して様々な情報の伝播も可能なため、
>センサーが拾った情報を遠隔地で取得したり、
>接続したディスプレイに表示させる内容をスマートフォンから制御したりといったことも可能な、
>「モノのインターネット(Internet of Things、以下IoT)」の基となる製品。


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










