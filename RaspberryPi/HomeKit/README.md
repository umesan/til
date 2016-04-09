# HomeKit

>HomeKitとはApple製品とホームセキュリティや家電を連携する開発システムの事です。  
>siriで指示すること電気をつけたり、現在は対応家電も発売されています。
>http://www.apple.com/jp/ios/homekit/


Siriをコントローラーとして、家電を操作できるような仕組みを開発できる開発システムのこと。


# Homebridge
>HomeKitをエミュレートするnode.jsサーバ。  
>https://datahotel.io/archives/725
  
日本ではHomeKit対応した家電が少ないため、Siriから操作できる対象の家電が少ない。  
  
Homebridgeを利用することで、Homekit未対応の家電に対しても、  
Siriの命令を受け取り、そこから、Homkit対応家電のように操作命令を擬似的に与えることができます。  
  
例えば、Homebridge + IRKitのようなAPIリモコンと組み合わせることで、  
Homkit対応家電ではないテレビなどにも、Siri経由で電気のON・OFFが可能になります。

### インストール

avahiのライブラリをインストール
```
sudo apt-get install libavahi-compat-libdnssd-dev
```

node.jsのインストール
```
wget https://nodejs.org/dist/v4.0.0/node-v4.0.0-linux-armv6l.tar.gz
```

解答
```
tar -xvf node-v4.0.0-linux-armv6l.tar.gz
```

移動
```
cd node-v4.0.0-linux-armv6l
```

コピー
```
sudo cp -R * /usr/local/
```

再起動
```
sudo reboot
```

インストール確認
```
pi@raspberrypi:~/ $ node -v
# v4.0.0

pi@raspberrypi:~/ $ npm -v
#2.14.2
```


### 持っているデバイス用のパッケージをインストール
philipshueの場合
```
sudo npm install -g  homebridge-philipshue
```


### homebridge 設定ファイルを編集

インストールしたパッケージを homebridgeに登録して、利用できるようにする。

```
# xxx は自分で追加したユーザ名
sudo vi /home/xxx/.homebridge/config.json
```

下記のフォーマットで記入
```
{
  "bridge": {
    "name": "Homebridge",
    # Macアドレスを記入。フォーマットがMacであればOKとのこと
    "username": "xx:xx:xx:xx:xx:xx",
    # portは ssh で設定したポート番号以外「49152〜65535」の中から好きに選択
    "port": xxxxxx,
    # ここも任意で好きな値をいれてよし
    "pin": "031-45-154"
  },
  "platforms": [
    {
      "platform": "PhilipsHue",
      "name": "Philips Hue",
      "ip_address": "xxx.xxx.x.xx",
      "username": "xxxxxx",
      "excludephilips": false
    }
  ]
}
```

保存後、コマンド実行
```
homebridge
```

pinコードが表示されれば成功。


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


##### 6. ログとエラー確認

Homebridge経由で実行した処理は、ログ＆エラーとしてたまっていく。  
下記を実行すると現在のログが確認できる。

```
tail -f /var/log/homebridge.log
tail -f /var/log/homebridge.err
```



### Homekitを利用するための、iOSアプリのインストール

Homekitを利用するための、iOSアプリはいろいろある。  
App Storeに登録されている下記のアプリから好きなのをインストール。  
  
[Insteon+](https://itunes.apple.com/jp/app/insteon+/id919270334)  
 - 無料
 - メニュー英語

[Elgato Eve](https://itunes.apple.com/jp/app/elgato-eve/id917695792?mt=8)  
 - 無料
 - メニュー日本語

[Home - Smart Home Automation](https://itunes.apple.com/jp/app/home-smart-home-automation/id995994352?mt=8)  
 - 有料
 - AppleWatch対応
  
また、下記でAppleが「HomeKit Catalog」というアプリをソース形式で配布している。  
カスマイズしたい場合はこちらを利用。  
[HomeKit Catalog](https://developer.apple.com/library/ios/samplecode/HomeKitCatalog/Introduction/Intro.html)  
  
こちらはソース形式で配布されているため、Xcodeを使ってビルドする必要があります。
ダウンロードにはDevelopperアカウントが必要。

