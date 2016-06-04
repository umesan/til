## Mac OS X 10.11 El CapitanでSass,Compassなどが動かなくなった場合の対処方法

#### ＜参考URL＞
http://blog.h-wd.info/2015/10/10/installing-sass-on-os-x-10-11-el-capitan/

#### 対応
Macの仕様でインストール場所が変わったため、インストールし直す。

```
# Sass
sudo gem install -n /usr/local/bin sass
# Compass
sudo gem install -n /usr/local/bin compass
```
