## 遇到的问题
用户打开百度新闻显示一直加载中，加载不出页面。

1 对比正常抓取日志中发现，主资源正常加载，但是主资源response的长度不一致。

2 发现有一个子资源没有从网络获取，而是被服务器304。

## 验证问题：
这个子资源的加载会不会是百度新闻一致加载不出的关键。


## 日志对比：


### 主资源请求：

- 打不开主资源请求：
```
2016-08-22 16:09:22.998 Stream   	HttpNetworkTransaction::DoLoop this:-1407290208 result:0 next_state_:[STATE_SEND_REQUEST]  url:http://m.news.baidu.com/news?fr=mohome&ssid=0&from=1000953b&uid=&pu=sz%401320_1002%2Cta%40iphone_2_5.1_2_6.9&bd_page_type=1
2016-08-22 16:09:22.999 Spdy     	### Basic Request Headers: ###
2016-08-22 16:09:22.999 Spdy     	    GET /news HTTP/1.1
2016-08-22 16:09:22.999 Spdy     	    Host: m.news.baidu.com
2016-08-22 16:09:23.000 Spdy     	    Connection: keep-alive
2016-08-22 16:09:23.000 Spdy     	    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
2016-08-22 16:09:23.000 Spdy     	    User-Agent: Mozilla/5.0 (Linux; U; Android 5.1.1; zh-cn; vivo X7 Build/LMY47V) AppleWebKit/537.36 (KHTML, like Gecko)Version/4.0 Chrome/37.0.0.0 MQQBrowser/6.9 Mobile Safari/537.36
2016-08-22 16:09:23.000 Spdy     	    Accept-Encoding: gzip, deflate
2016-08-22 16:09:23.001 Spdy     	    Accept-Language: zh-CN,en-US;q=0.8
2016-08-22 16:09:23.001 Spdy     	    Cookie: BAIDUID=F7119774F411FA05F71CF8C185552323:FG=1; BAIDULOC=12530931.173965799_4534128.674197617_30_176_1471849961749; H5LOC=1; H_WISE_SIDS=104687_102571_104492_100273_102435_103569_104952_104341_106323_108825_107516_108459_108411_108335_107694_107790_108290_108085_109505_107918_108073_108121_108341_107805_107788_108297_107630_107312_107300_107616; plus_cv=1::m:691fc2a7; Hm_lvt_2b39067e768e666f600180167f68927f=1471851381,1471851394; Hm_lpvt_2b39067e768e666f600180167f68927f=1471853304
2016-08-22 16:09:23.001 Spdy     	### Request Headers End ###


回复：

2016-08-22 16:09:23.142 Stream   	### Response Headers: 200 ### url:http://m.news.baidu.com/news?fr=mohome&ssid=0&from=1000953b&uid=&pu=sz%401320_1002%2Cta%40iphone_2_5.1_2_6.9&bd_page_type=1
2016-08-22 16:09:23.143 Stream   	 Cache-Control: no-cache
2016-08-22 16:09:23.143 Stream   	 Content-Length: 776
2016-08-22 16:09:23.144 Stream   	 Content-Type: text/html;charset=utf-8
2016-08-22 16:09:23.144 Stream   	 Date: Mon, 22 Aug 2016 08:09:23 GMT
2016-08-22 16:09:23.144 Stream   	 Server: apache
2016-08-22 16:09:23.145 Stream   	### Response Headers end ###
```

- 正常请求：

```
2016-08-22 21:53:06.595 Stream   	HttpNetworkTransaction::DoLoop this:2082680352 result:0 next_state_:[STATE_SEND_REQUEST]  url:http://m.news.baidu.com/news?fr=mohome&ssid=0&from=1000953b&uid=&pu=sz%401320_1002%2Cta%40iphone_2_5.1_2_6.9&bd_page_type=1
2016-08-22 21:53:06.596 Spdy     	### Basic Request Headers: ###
2016-08-22 21:53:06.596 Spdy     	    GET /news HTTP/1.1
2016-08-22 21:53:06.596 Spdy     	    Host: m.news.baidu.com
2016-08-22 21:53:06.597 Spdy     	    Connection: keep-alive
2016-08-22 21:53:06.597 Spdy     	    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
2016-08-22 21:53:06.598 Spdy     	    User-Agent: Mozilla/5.0 (Linux; U; Android 4.4.2; zh-cn; HUAWEI MT7-CL00 Build/HuaweiMT7-CL00) AppleWebKit/537.36 (KHTML, like Gecko)Version/4.0 Chrome/37.0.0.0 MQQBrowser/6.9 Mobile Safari/537.36
2016-08-22 21:53:06.598 Spdy     	    Accept-Encoding: gzip, deflate
2016-08-22 21:53:06.599 Spdy     	    Accept-Language: zh-CN,en-US;q=0.8
2016-08-22 21:53:06.599 Spdy     	    Cookie: BAIDUID=665BAD9397330B0AF617518A71851194:FG=1; plus_cv=1::m:1; Hm_lvt_2b39067e768e666f600180167f68927f=1471855328,1471855883,1471860170,1471873856; Hm_lpvt_2b39067e768e666f600180167f68927f=1471873962
2016-08-22 21:53:06.600 Spdy     	### Request Headers End ###


回复：

2016-08-22 21:53:06.710 Stream   	### Response Headers: 200 ### url:http://m.news.baidu.com/news?fr=mohome&ssid=0&from=1000953b&uid=&pu=sz%401320_1002%2Cta%40iphone_2_5.1_2_6.9&bd_page_type=1
2016-08-22 21:53:06.711 Stream   	 Cache-Control: no-cache
2016-08-22 21:53:06.711 Stream   	 Content-Length: 776
2016-08-22 21:53:06.712 Stream   	 Content-Type: text/html;charset=utf-8
2016-08-22 21:53:06.712 Stream   	 Date: Mon, 22 Aug 2016 13:53:00 GMT
2016-08-22 21:53:06.712 Stream   	 Server: apache
2016-08-22 21:53:06.713 Stream   	### Response Headers end ###

```

主资源对比完全一致。



### 关键子资源请求:

- 打不开请求日志：
```
2016-08-22 16:09:23.282 Stream   	HttpNetworkTransaction::DoLoop this:-1402350080 result:0 next_state_:[STATE_SEND_REQUEST]  url:http://hm.baidu.com/hm.js?2b39067e768e666f600180167f68927f
2016-08-22 16:09:23.283 Spdy     	### Basic Request Headers: ###
2016-08-22 16:09:23.283 Spdy     	    GET /hm.js HTTP/1.1
2016-08-22 16:09:23.283 Spdy     	    Host: hm.baidu.com
2016-08-22 16:09:23.284 Spdy     	    Connection: keep-alive
2016-08-22 16:09:23.284 Spdy     	    Accept: */*
2016-08-22 16:09:23.284 Spdy     	    If-None-Match: 2499e4228f3a1b98a13a87cfdc935f4f
2016-08-22 16:09:23.284 Spdy     	    User-Agent: Mozilla/5.0 (Linux; U; Android 5.1.1; zh-cn; vivo X7 Build/LMY47V) AppleWebKit/537.36 (KHTML, like Gecko)Version/4.0 Chrome/37.0.0.0 MQQBrowser/6.9 Mobile Safari/537.36
2016-08-22 16:09:23.285 Spdy     	    Referer: http://m.news.baidu.com/news?fr=mohome&ssid=0&from=1000953b&uid=&pu=sz%401320_1002%2Cta%40iphone_2_5.1_2_6.9&bd_page_type=1
2016-08-22 16:09:23.285 Spdy     	    Accept-Encoding: gzip, deflate
2016-08-22 16:09:23.285 Spdy     	    Accept-Language: zh-CN,en-US;q=0.8
2016-08-22 16:09:23.285 Spdy     	    Cookie: BAIDUID=F7119774F411FA05F71CF8C185552323:FG=1; BAIDULOC=12530931.173965799_4534128.674197617_30_176_1471849961749; H5LOC=1; HMACCOUNT=8E2656CBC4CE93F8; H_WISE_SIDS=104687_102571_104492_100273_102435_103569_104952_104341_106323_108825_107516_108459_108411_108335_107694_107790_108290_108085_109505_107918_108073_108121_108341_107805_107788_108297_107630_107312_107300_107616; plus_cv=1::m:691fc2a7
2016-08-22 16:09:23.286 Spdy     	### Request Headers End ###
```

- 正常请求：

```
2016-08-22 21:53:06.965 Stream   	HttpNetworkTransaction::DoLoop this:-2118327848 result:0 next_state_:[STATE_SEND_REQUEST]  url:http://hm.baidu.com/hm.js?2b39067e768e666f600180167f68927f
2016-08-22 21:53:06.965 Spdy     	### Basic Request Headers: ###
2016-08-22 21:53:06.968 Spdy     	    GET /hm.js HTTP/1.1
2016-08-22 21:53:06.969 Spdy     	    Host: hm.baidu.com
2016-08-22 21:53:06.969 Spdy     	    Connection: keep-alive
2016-08-22 21:53:06.970 Spdy     	    Accept: */*
2016-08-22 21:53:06.970 Spdy     	    If-None-Match: 2499e4228f3a1b98a13a87cfdc935f4f
2016-08-22 21:53:06.971 Spdy     	    User-Agent: Mozilla/5.0 (Linux; U; Android 4.4.2; zh-cn; HUAWEI MT7-CL00 Build/HuaweiMT7-CL00) AppleWebKit/537.36 (KHTML, like Gecko)Version/4.0 Chrome/37.0.0.0 MQQBrowser/6.9 Mobile Safari/537.36
2016-08-22 21:53:06.971 Spdy     	    Referer: http://m.news.baidu.com/news?fr=mohome&ssid=0&from=1000953b&uid=&pu=sz%401320_1002%2Cta%40iphone_2_5.1_2_6.9&bd_page_type=1
2016-08-22 21:53:06.972 Spdy     	    Accept-Encoding: gzip, deflate
2016-08-22 21:53:06.972 Spdy     	    Accept-Language: zh-CN,en-US;q=0.8
2016-08-22 21:53:06.973 Spdy     	    Cookie: BAIDUID=665BAD9397330B0AF617518A71851194:FG=1; HMACCOUNT=78522EFACF565907; plus_cv=1::m:1
2016-08-22 21:53:06.973 Spdy     	### Request Headers End ###
```

请求中携带了If-None-Match字段：
大约不到100ms内response 304


```
2016-08-22 16:09:23.362 Stream   	### Response Headers: 304 ### url:http://hm.baidu.com/hm.js?2b39067e768e666f600180167f68927f
2016-08-22 16:09:23.362 Stream   	 Cache-Control: max-age=0, must-revalidate
2016-08-22 16:09:23.363 Stream   	 Date: Mon, 22 Aug 2016 08:09:23 GMT
2016-08-22 16:09:23.364 Stream   	 Etag: 2499e4228f3a1b98a13a87cfdc935f4f
2016-08-22 16:09:23.365 Stream   	 Server: apache
2016-08-22 16:09:23.366 Stream   	### Response Headers end ###
```



- 正常响应：

```
2016-08-22 21:53:07.031 Stream   	### Response Headers: 304 ### url:http://hm.baidu.com/hm.js?2b39067e768e666f600180167f68927f
2016-08-22 21:53:07.032 Stream   	 Cache-Control: max-age=0, must-revalidate
2016-08-22 21:53:07.032 Stream   	 Date: Mon, 22 Aug 2016 13:53:00 GMT
2016-08-22 21:53:07.033 Stream   	 Etag: 2499e4228f3a1b98a13a87cfdc935f4f
2016-08-22 21:53:07.033 Stream   	 Server: apache
2016-08-22 21:53:07.033 Stream   	### Response Headers end ###
```
