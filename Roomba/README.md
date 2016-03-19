
# ルンバ

>全自動で部屋を掃除してくれる「ルンバ」の最新モデルが「ルンバ 980」です。
>Wi-Fiを搭載してスマートフォンからの操作が可能になっただけでなく、
>カメラを搭載することで家具の配置や部屋の形を把握して自分でマップを作成、
>一度掃除した場所を何度も掃除しない効率性を身につけており、
>床の素材によって吸引力を自動的に10倍に上げることもできるという、かなり賢いお掃除ロボットへと進化したようです。
>
> お掃除ロボのルンバがWi-Fiとカメラを搭載、新型「ルンバ 980」は一体何がすごいのか？ - GIGAZINE
> http://gigazine.net/news/20151026-roomba-980-review/

# 参考URL
http://www.irobot-jp.com/roomba/980/  
http://gigazine.net/news/20150917-irobot-roomba-980/  
http://gigazine.net/news/20151026-roomba-980-review/  
http://sikmi.com/blog/smart-house/hacking-roomba-slackbot



## API 

### 掃除開始

コマンド
```
curl https://irobot.axeda.com/services/v1/rest/Scripto/execute/AspenApiRequest -X POST -d "blid=★ルンバのID★&robotpwd=★ルンバのパスワード★&method=multipleFieldSet&value=%7b%22remoteCommand%22%20%3a%20%22start%22%7d"
```

Request
```
blid: "★ルンバのID★"
robotpwd: "★ルンバのパスワード★"
method: "multipleFieldSet"
value: { "remoteCommand" : "start" }
```

Response
```
{"status":"OK","method":"multipleFieldSet"}
```


### ルンバの情報取得

コマンド
```
curl https://irobot.axeda.com/services/v1/rest/Scripto/execute/AspenApiRequest -X POST -d "blid=★ルンバのID★&robotpwd=★ルンバのパスワード★&method=getStatus"
```

Request
```
blid: "★ルンバのID★"
robotpwd: "★ルンバのパスワード★"
method: "getStatus"
```

Response
```
{
  "status":"OK",
  "method":"getStatus",
  "sku":"R980060",
  "country":"JP",
  "postalCode":"",
  "regDate":"2016-xx-xx",
  "hardwareVersion":"",
  "manualUpdate":"",
  "swUpdateAvailable":"",
  "schedHold":"",
  "autoEvacCount":"",
  "autoEvacFlags":"",
  "wifiDiagnostics":"",
  "autoEvacModel":"",
  "milestones":"",
  "robotName":"ルンバの名前",
  "robotLanguage":"",
  "tzName":"Asia/Tokyo",
  "twoPass":"",
  "noAutoPasses":"",
  "openOnly":"",
  "carpetBoost":"",
  "binPause":"",
  "vacHigh":"",
  "noPP":"",
  "ecoCharge":"",
  "mission":"{
    \"nMssn\":0,
    \"done\":\"cncl\",
    \"flags\":0,
    \"sqft\":0,
    \"runM\":0,
    \"chrgM\":0,
    \"pauseM\":0,
    \"doneM\":0,
    \"dirt\":0,
    \"chrgs\":0,
    \"saves\":0,
    \"evacs\":0,
    \"pauseId\":0,
    \"wlBars\":[0,0,0,0,0]
  }",
  "preventativeMaintenance":"[
    {
      \"partId\":\"bin\",
      \"date\":\"2016-xx-xx\",
      \"distance\":0,
      \"runtime\":0,
      \"months\":0,
      \"notified\":false
    },
    {
      \"partId\":\"core\",
      \"date\":\"2016-xx-xx\",
      \"distance\":0,
      \"runtime\":0,
      \"months\":0,
      \"notified\":false
    },
    {
      \"partId\":\"extractor\",
      \"date\":\"2016-xx-xx\",
      \"distance\":0,
      \"runtime\":0,
      \"months\":0,
      \"notified\":false
    }
  ]",
  "cleanSchedule":"{
    \"cycle\":[\"none\",\"start\",\"none\",\"start\",\"none\",\"start\",\"none\"],
    \"h\":[0,8,0,8,0,8,0],
    \"m\":[0,45,0,45,0,45,0]
  }",
  "robot_status":"{
    \"flags\":4,
    \"cycle\":\"none\",
    \"phase\":\"charge\",
    \"pos\":{
      \"theta\":-1,
      \"point\":{
        \"x\":90,
        \"y\":-124
      }
    },
    \"batPct\":0,
    \"expireM\":0,
    \"rechrgM\":0,
    \"error\":0,
    \"notReady\":0,
    \"mssnM\":0,
    \"sqft\":0
  }",
  "softwareVersion":"v1.2.3",
  "lastSwUpdate":"2016-xx-xx xx:xx:xx+0000",
  "engBuild":"",
  "bbrun":"{
    \"hr\":合計清掃時間の時,
    \"min\":合計清掃時間の分,
    \"sqft\":平方フィート,
    \"nStuck\":0,
    \"nScrubs\":0,
    \"nPicks\":0,
    \"nPanics\":0,
    \"nCliffsF\":0,
    \"nCliffsR\":0,
    \"nMBStll\":0,
    \"nWStll\":0,
    \"nCBump\":0
  }",
  "missing":false,
  "ota":"{
    \"st\":0,
    \"err\":0,
    \"lbl\":\"\"
  }"
}
```




### ルンバの行動履歴
```
curl https://irobot.axeda.com/services/v1/rest/Scripto/execute/AspenApiRequest -X POST -d "blid=★ルンバのID★&robotpwd=★ルンバのパスワード★&method=missionHistory"
```

Request
```
blid: "★ルンバのID★"
robotpwd: "★ルンバのパスワード★"
method: "missionHistory"
```

Response
```
{
  "status":"OK",
  "method":"missionHistory",
  "missions":[
    {
      "date":"2016xxxx xx:xx",
      "nMssn":0,
      "done":"ステータス",
      "flags":0,
      "sqft":清掃時間,
      "runM":0,
      "chrgM":0,
      "pauseM":0,
      "doneM":0,
      "dirt":0,
      "chrgs":0,
      "saves":0,
      "evacs":0,
      "pauseId":0,
      "wlBars":[0,0,0,0,0]
    },
    {
      "date":"2016xxxx xx:xx",
      "nMssn":0,
      "done":"ステータス",
      "flags":0,
      "sqft":0,
      "runM":清掃時間,
      "chrgM":0,
      "pauseM":0,
      "doneM":0,
      "dirt":0,
      "chrgs":0,
      "saves":0,
      "evacs":0,
      "pauseId":0,
      "wlBars":[0,0,0,0,0]
    }
  ]
}
```
