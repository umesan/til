## RaspberryPi内でnpmパッケージをグローバルインストールした際の配置先

独自のnpmパッケージを配置したい場合はこちらに配置。

```
cd /usr/local/lib/node_modules/
```


## USB無線LANアダプタがものすごい発熱する場合
USB無線LANアダプタの熱にやられてRaspberryPiがシャットダウンする。  
その場合は、USB延長ケーブルをつけて、物理的に本体とUSB無線LANアダプタを離してやると効果がある。  
https://x1japan.wordpress.com/2015/04/18/rpi-wifi-down/



## それでもだめなら有線LAN
有線で接続する場合は、固定IPにしないと再起動の度にIPが変わるとのことなので注意。
有線接続するだけなら、
RaspberryPiにキーボード・マウス・ディスプレイ・有線LANをつなげた状態で起動し、
Menu -> Accessories -> Terminal にて `ifconfig` するとIPが `192.xxx.x.xx`のように表示されるので
そちらを参考にしてにssh接続する。

```
ssh ユーザ名@192.xxx.x.xx -p xxxx
```

＜固定IP参考＞
http://qiita.com/ykog/items/a6dbba1c09e870f8f702  
http://qiita.com/MarieKawasuji/items/b088ffb252a92eee8f5d  
http://hattotech.hatenablog.com/entry/2016/04/13/000244  
http://swiftlife.hatenablog.jp/entry/2016/01/10/170249

```
sudo vi /etc/dhcpcd.conf
```
で対象ファイルを編集し、
```
interface eth0
static ip_address=192.168.x.xx/24
static routers=192.168.x.xx
static domain_name_servers=192.168.x.xx
```
を末尾に追記する
