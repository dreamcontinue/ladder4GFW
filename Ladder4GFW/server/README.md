## [Caddy 反向代理](https://doubibackup.com/l-en8vwt-2.html)

域名反向代理http 

``` shell script
echo "http://DOMAIN.COM {
 gzip
 proxy / http://xxx.xx
}" >> /usr/local/caddy/Caddyfile
```
    
or

 ```shell script
:80 {
    root PATH/TO/SITE   // 改 样例 /var/www/site
    gzip
    browse
    proxy /PATH/ localhost:YOUR_PORT {  // 改两个
        websocket
        header_upstream -Origin
    }
    proxy / http://xxx.xx
}
 ```