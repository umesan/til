
# RaspberryPi

>Raspberry Pi（ラズベリー パイ）は、ARMプロセッサを搭載したシングルボードコンピュータ。  
>イギリスのラズベリーパイ財団 (Raspberry Pi Foundation) によって開発されている。

>単体でも汎用コンピュータとして動作するスペックを持っており、
>インターネットを介して様々な情報の伝播も可能なため、
>センサーが拾った情報を遠隔地で取得したり、
>接続したディスプレイに表示させる内容をスマートフォンから制御したりといったことも可能な、
>「モノのインターネット(Internet of Things、以下IoT)」の基となる製品。


# 参考URL
http://okuzawats.com/raspberry-pi-os-install-20140904  
http://okuzawats.com/noobs-20150603  
http://www.tapun.net/raspi/raspi-os-setup  
  
コマンドでのインストール参考  
http://karaage.hatenadiary.jp/entry/2015/10/20/073000

SD書き込み失敗の原因参考
http://divide-et-impera.org/archives/1092


## OSインストールの方法

RaspberryPiはコンピュータなので、  
RaspberryPiで何かするにはRaspberryPi用のOSインストールが必要。  
  
OSをインストールするには2通りある。  
  
1. Noobsというインストーラ経由で、OSをインストールする
2. OSを直接インストールする


#### Noobs
NoobsはRaspberry Pi用のOS"インストーラ"。   
https://www.raspberrypi.org/downloads/noobs/  
  
Noobsは“New Out Of the Box Software”の略です。“Out of thebox”は  
「枠を超える」という意味、（OSの）枠を超えたソフトウェアという意味合い。  
  
RaspbianをはじめとするいろいろなOSを、  
クリックだけでRaspberry Piにインストールすることができるようになります。  

容量が重いが、GUIがあるのでOSのインストールが簡単。

#### Raspbian
Raspberry Piの最も一般的なOS。
https://www.raspberrypi.org/downloads/raspbian/

直接インストールも可能だし、Noobs経由でのインストールも可能。

#### その他のOS
Pidora,RISC OS,RaspBMC,Arch,OpenELEC


# OS インストール

Noobs経由で作業したところ、HDMIを認識してくれずGUIでのOSインストール作業ができなかったため、  
コマンドで直接OSをインストールするようにした。


### 1. 公式サイトからOSをダウンロード。

1-1. 「Raspbian Jessie」のZIPファイルを選択  
https://www.raspberrypi.org/downloads/raspbian/  

1-2. ダウンロード後、zipを解凍、imgファイルがでてくるのでファイルの配置先を認識しておく  
     うちの環境では Downloads の中に出力されている状態。  

### 2. microSDカードをフォーマット
  
2-1. SD Card Formatter(for Mac) をダウンロード＆インストール  
     https://www.sdcard.org/downloads/formatter_4/  
2-2. SDカードをMacに挿入  
2-3. SD Card Formatterをアプリケーションから起動し、挿入したSDカードを指定  
2-4. 上書きフォーマットを選び、フォーマットボタンを押す。  
2-5. 時間がかかるが完了まで待つ  
2-6. 完了後、フォーマット確認作業をする  
　　　　2-6-1. appleメニュー  
　　　　2-6-2. このMacについて  
　　　　2-6-3. システムレポート  
　　　　2-6-4. ハードウェア -> USB -> 挿入したSDカードを選択  
　　　　2-6-5. ボリューム：ファイルシステムが MS-DOS FAT32になっているか確認。  
　　　　2-6-6. ボリューム：BSD名(diskX)のX部分をメモ(うちの環境ではdisk2s1)  

### 3. imgファイルをmicroSDに書き込み

3-1. ディスクのデバイスファイルの確認。
  
ターミナルで下記コマンド実行。
```
df -h
```
  
2-6-6で確認した BSD名が表示されているのがデバイスファイル。  
うちの環境では下記。  
```
/dev/disk2s1
```
  
  
3-2. 書き込みしたいSDカードをアンマウント

```
diskutil umountDisk /dev/disk2s1
```
  

3-3. imgファイルの配置先まで移動

1-2の imgファイルの場所までターミナルで移動
```
cd Downloads
```
  
  
3-4. imgファイルの書き込み  
  
ターミナルで下記実行
```
sudo dd if=※imgファイルのファイル名※ of=※デバイスファイル(diskの前にrをつける)※ bs=1m

# うちの環境ではこんな感じ
sudo dd if=2016-03-18-raspbian-jessie.img of=/dev/rdisk2 bs=1m
```

/dev/disk2s1 に対して /dev/rdisk2 のようにrをつけるのとs1を外すのがポイント。


rをつけるとアンバッファモードというのになって書き込みが速くなるらしい。
bs=1mは一度に書き込む容量を表していて1m(1MB)くらいにしないとこれまた速度が遅くなるとのこと。

また、s1を外す理由は下記。
>先ほどdfコマンドで表示したものは/dev/sdc1だけれども、ここでは/dev/sdcと入力。
>1はパーティションの番号になるので、それではなくてSDカードそのもののデバイスファイル名を入力する必要がある。
>fdisk -lで表示される名前だ。
>うっかり間違って1つけて最初導入したのだけれども、
>そうするとRaspberry piは起動しない。
>PWRランプがずっとついたままでACTランプはまったく変化無しだ。
>ちなみにbsは一回の書き込みのデータサイズ。変更せずにそのままで良い。
>このとおりにインストールすれば、ちゃんと起動するSDカードを作成できると思う。
>
>http://divide-et-impera.org/archives/1092


書き込みが終わると下記のようなメッセージが表示される。
```
3847+0 records in
3847+0 records out
4033871872 bytes transferred in 231.269470 secs (17442302 bytes/sec)
```
  
  
3-5. SDカードの取り出し  
  
以下のコマンドでアンマウントしてからSDカードを取り出す。  
```
diskutil eject /dev/disk2s1
```


これで img ファイルのmicroSDへの書き込みは完了。  
MacからmicroSDを抜いて、Raspberry Piに挿入する。





