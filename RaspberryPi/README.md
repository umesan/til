
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




# 初期設定

### 初期設定の参考URL
  
初期設定系  
http://karaage.hatenadiary.jp/entry/2015/10/20/073000  
http://www.tapun.net/raspi/raspi-os-setup  
http://qiita.com/JO3QMA/items/b0892ce38f9220abea29  
  
ssh設定  
http://okuzawats.com/ssh-20140917  
  
wifi設定  
http://denshikousaku.net/raspberry-pi-wifi-lan-usb  
  
セキュリティ設定  
http://masatolan.com/raspberry-pi/raspberry-pi-security/  
  
文字化け  
http://start-now.link/100/archives/1930  
  

### 初期設定でやったこと概要

 0. 機器の設置
 1. ファイルシステムの拡張
 2. タイムゾーン設定
 3. キーボードの設定
 4. SSHの有効化
 5. wifi設定
 6. ソフトウェアのアップデート
 7. root パスワードの設定
 8. デフォルトユーザ名以外のユーザに変更
 9. SSH設定
10. 日本語インストール





### 0. 機器の設置


#### 0-1. 機器の設置

Raspberry Piに下記を接続  
  
- USBキーボード
- USBマウス
- USB無線LANアダプタ
- ディスプレイ(HDMIケーブル)
- インストールした micro SD

最後に、電源（マイクロUSB）を接続。
OSのインストールに成功していると、
電源を接続するだけで起動するはず。


#### 0-2. OS起動後の初期設定の仕方について
GUIから設定する方法と、Terminalから設定する方法の2通りがある。  
  
GUIからやる場合は、画面左上から  
Menu -> Preferences -> Raspberry pi Configuration  
で立ち上がる画面から設定。  
  
Terminalから設定する場合は、  
画面上メニューからTerminalを選択。  
  
Terminal 起動後、下記コマンドを実行  
  
```
sudo raspi-config
```
  
  
設定用画面が立ち上がる。


### 1. ファイルシステムの拡張 
「1 Expand Filesystem」を選択。

選択することで、SDカードの使われていない領域が開放され、
すべての領域を使えるようになる。

### 2. タイムゾーン設定

##### 2-1 「4 Internationalsation Options」を選択。

##### 2-2 「I1 Change Locale」を選択
下記を選択。
-「en_GB.ISO-8859-15 ISO 8859-15」
-「ja_JP.EUC-JP EUC-JP」
-「ja_JP.UTF-8 UTF-8」

デフォルトロケールを「ja_JP.UTF-8」で設定。


##### 2-3 I2 「Change Timezone」を選択
タイムゾーンを AsiaのTokyoに設定


### 3. キーボードの設定

##### 3-1 「4 Internationalsation Options」を選択。


##### 3-2 I3 Change Keyboard Layout
- 自分のキーボードを選択
- キーボードレイアウトを聞かれるので、[Japanese – Japanese (OADG 109A)]を選択
- [The default for the keyboard layout]を選択
- [No compose key]を選択
- Ctrl + Alt + Backspace キーの処理は[No]を選択


### 4. SSHの有効化

##### 「9 Advanced Options」を選択
##### 「A4 SSH」を選択
enabled に。


### 5. wifi設定

##### 5-1. wifi設定
無線LANアダプタを挿入している場合は下記コマンド実行。

```
wpa_passphrase WifiのSSID Wifiのパスワード
```

実行すると下記のようなテキストが出力される
```
network={
  ssid="WifiのSSID"
  #psk="Wifiのパスワード"
  psk=hogehogehogehogehogehogehogehogehoge
}
```

上記をコピー後、下記コマンドを実行

```
sudo vi /etc/wpa_supplicant/wpa_supplicant.conf
```

先ほどのコピーを含め下記にの形にして記入

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
network={
    ssid="WifiのSSID"
    #psk="Wifiのパスワード"
    psk=hogehogehogehogehogehogehogehogehoge
}
```

最後にRaspberry Pi を再起動
```
sudo reboot
```

これでWifiがつながる。  


##### 5-2. 無線LANのUSBアダプタのパワーマネジメント機能をOFFにする
無線LANの場合、しばらく放置するとネットワークが切れることがある。  
  
Raspberry Piで無線LANの反応が悪くなる原因のひとつに、  
無線LANのUSBアダプタのパワーマネジメント機能がONになっている可能性があるため。  
  
下記サイトを参考に設定する。  
http://denshikousaku.net/fix-sluggish-response-of-raspberry-pi-wifi-adaptor
 
```
sudo vim /etc/modprobe.d/8192cu.conf

# Disable power saving
options 8192cu rtw_power_mgnt=0 rtw_enusbss=1 rtw_ips_mode=1

//保存して終了後 Raspberry Piを再起動
sudo reboot
```

##### 5-3. ServerAliveIntervalを1以上に設定する

5-2 の対応で無線LANの反応はよくなると思いますが、
それでもSSHのセッションが切れる場合は、
ServerAliveIntervalを1以上にしてみましょう。

下記を参考に設定  
http://stkay.hateblo.jp/entry/2014/09/11/162214

```
# 新規作成
sudo vi ~/.ssh/config

# 記入
ServerAliveInterval 20

# 保存して終了
```

### 6. ソフトウェアのアップデート

Wifiが繋がったので各種ツールをアップデートする。  
結構時間かかる。  
  
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade

＃ 完了したら再起動
sudo reboot
```

### 7. root パスワードの設定
デフォルトでは設定されていないので、セキュリティ向上のため設定。

```
sudo passwd root
```
新しいパスワードを２回入力して完了。


### 8. デフォルトユーザ名以外のユーザを追加

>ラズパイは、最初から「pi」ユーザーが設定されているのですが、
>「ユーザー名」も「パスワード」も公開されているので、できれば使わない方が無難です。
>そこで、新しいユーザーを追加しておきましょう！
>http://masatolan.com/raspberry-pi/raspberry-pi-security/

下記コマンドを実行
```
sudo adduser xxxxxx
```
新しいパスワードを２回入力。
途中で色々聞かれるが、そのまま Enter 押してけばOK。


「pi」ユーザーと同じgroup(権限?)にしておきたいので、  
まず「pi」ユーザーのgroupを確認
```
groups pi
```

ズラッと、いろいろ表示されると思いますが、  
新規ユーザーxxxxxxに全部このgroupを追加。
```
sudo usermod -G pi,adm,dialout,cdrom,sudo,audio,video,plugdev,games,users,netdev,input,spi,gpio xxxxxx
```

groupが追加されたことを確認
```
groups xxxxxx
```

一旦Raspberry Piを終了させて、
新規ユーザーxxxxxxでログインできるか確認
```
ssh xxxxxx@xxx.xxx.x.x
```
入れたら新規ユーザ作成完了。  
  

新規ユーザーでログイン出来たら、
「pi」ユーザーは使うことが無いので以下のコマンドで削除。

```
userdel -r pi
```

削除しない場合はそのまま残しておくか、ユーザー名を変更
```
sudo usermod -l newpi pi
```


### 9. SSH設定
>ラズパイの初期設定では、SSHのポート番号がデフォルトの「22」となっています。
>このままだと攻撃されやすいので、自分の好きなポート番号に変更しましょう！
>
>http://masatsolan.com/raspberry-pi/raspberry-pi-security/

Raspberry Piにssh接続後、下記を実行。
```
sudo vim /etc/ssh/sshd_config
```

viでSSHの設定ファイルが開いたら、5行目くらいにあるポートの設定を変更
```
#Port 22
Port 51234
```
ポート番号は、「49152〜65535」の中から好きに選択可  
  
SSHを再起動
```
sudo /etc/init.d/ssh restart
```

ログアウトしてから、以下のようにSSH接続
```
ssh xxxxxx@192.168.3.3 -p 51234
```

## 終了

電源をひっこ抜くと死ぬことがあるので、  
ちゃんとシャットダウンしてから電源を抜くこと。  
  
シャットダウンのコマンド
```
sudo shutdown -h now
```








# Homekit
>HomeKitとはApple製品とホームセキュリティや家電を連携する開発システムの事です。  
>siriで指示すること電気をつけたり、現在は対応家電も発売されています。



# Homebridge
>Homekitをエミュレートするnode.jsサーバ。  
>https://datahotel.io/archives/725


### 永続化

＜参考URL＞  
https://github.com/nfarina/homebridge/wiki/Running-HomeBridge-on-a-Raspberry-Pi

Raspberry-Pi を再起動した際に、Homebridgeを起動するため、
init スクリプトを作成する。

##### 1. Raspberry-Piにssh接続
```
ssh ume@192.168.x.xx -p xxxxx
```

##### 2. init スクリプトを作成
```
sudo vi /etc/init.d/homebridge
```

##### 3. 下記URLの内容をコピー＆編集して書き込み。
https://raw.githubusercontent.com/fhd/init-script-template/master/template

```
#!/bin/sh
### BEGIN INIT INFO
# Provides:
# Required-Start:    $remote_fs $syslog
# Required-Stop:     $remote_fs $syslog
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Start daemon at boot time
# Description:       Enable service provided by daemon.
### END INIT INFO


# XXX は自分で新たに作ったユーザ名
dir="/home/XXX"
cmd="DEBUG=* /usr/local/bin/homebridge"
user="XXX"


name=`basename $0`
pid_file="/var/run/$name.pid"
stdout_log="/var/log/$name.log"
stderr_log="/var/log/$name.err"

get_pid() {
    cat "$pid_file"
}

is_running() {
    [ -f "$pid_file" ] && ps `get_pid` > /dev/null 2>&1
}

case "$1" in
    start)
    if is_running; then
        echo "Already started"
    else
        echo "Starting $name"
        cd "$dir"
        if [ -z "$user" ]; then
            sudo $cmd >> "$stdout_log" 2>> "$stderr_log" &
        else
            sudo -u "$user" $cmd >> "$stdout_log" 2>> "$stderr_log" &
        fi
        echo $! > "$pid_file"
        if ! is_running; then
            echo "Unable to start, see $stdout_log and $stderr_log"
            exit 1
        fi
    fi
    ;;
    stop)
    if is_running; then
        echo -n "Stopping $name.."
        kill `get_pid`
        for i in {1..10}
        do
            if ! is_running; then
                break
            fi

            echo -n "."
            sleep 1
        done
        echo

        if is_running; then
            echo "Not stopped; may still be shutting down or shutdown may have failed"
            exit 1
        else
            echo "Stopped"
            if [ -f "$pid_file" ]; then
                rm "$pid_file"
            fi
        fi
    else
        echo "Not running"
    fi
    ;;
    restart)
    $0 stop
    if is_running; then
        echo "Unable to stop, will not attempt to start"
        exit 1
    fi
    $0 start
    ;;
    status)
    if is_running; then
        echo "Running"
    else
        echo "Stopped"
        exit 1
    fi
    ;;
    *)
    echo "Usage: $0 {start|stop|restart|status}"
    exit 1
    ;;
esac

exit 0
```

##### 4. パーミッション変更とインストール
```
sudo chmod 755 /etc/init.d/homebridge
sudo update-rc.d homebridge defaults
```

##### 5. 手動実行する
```
sudo /etc/init.d/homebridge start
```
  
これで、ログアウト（ターミナルを落と）してもHomebridgeは実行されつづける。


#### 6. ログとエラー確認

Homebridge経由で実行した処理は、
ログ＆エラーとしてたまっていく。

下記を実行すると現在のログが確認できる。

```
tail -f /var/log/homebridge.log
tail -f /var/log/homebridge.err
```
