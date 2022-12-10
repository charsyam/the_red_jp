# the_red

 * 実行する前にinstall_program.shを実行してzookeeperとredisをインストールしてください。
 * dockerが実行可能な環境が必要です。

## Chapter 1

 * ステートレスなサービスの例を見せます。ステートレスなサーバーはロードバランサーに追加する方法で簡単に拡張できます。

### geoip
 * ipを入力すると、該当する国を教えてくれる例題です。公開されているmaxmindライブラリを利用します。
  * 独立して動作するステートレスなサービスの例です。

### scrap
 * 入力されたurlのOpengraphをパースして見せる例題です。
  * 独立して動作するステートレスなサービスの例です。

## Chapter 2

### loadbalancer
 * ロードバランサーの例題はnginxを利用して簡単にロードバランサーが動作することを見せます。
  * nginxのインストールが必要です。
```
   sudo apt install -y nginx
```
  * nginxは、「127.0.0.1:7001」と「127.0.0.1:7002」の２つに合わせて設定されています。scrapサーバーをポート7001、ポート7002を使用するように２つを実行します。
   * Dockerを利用しても構いません。
```
   docker build . -t ch1/scrap
```
   * Docker Run
```
   docker run -e ENDPOINTS=0.0.0.0:7001 --network host ch1/scrap
   docker run -e ENDPOINTS=0.0.0.0:7002 --network host ch1/scrap
```

  * 次の命令で実行することができます。

### service discovery
 * zookeeperの実行が必要です。
 * callerは、calleeが追加／削除されると、zookeeperを通してcalleeの変化について通知を受けるようになります。
  * callee
   * 実際のScrapを行うサービスです。
  * caller
   * calleeにScrapをリクエストするサービスです。
   * callerは、callリストでRound Robin方式によってリクエストをします。
