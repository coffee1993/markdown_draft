## 手机网络CMWAP 和 CMNET 主要区别与适用范围


CMWAP 和 CMNET 只是中国移动人为划分的两个GPRS接入方式。
前者是为手机WAP上网而设立的，后者则主要是为PC、笔记本电脑、PDA等利用GPRS上网服务。


### CMWAP
CMWAP有了部分限制，资费上也存在差别,据网上相关资料所知，中国移动GPRS网络目前只有唯一的一个WAP网关：`10.0.0.172，`有中国移动提供，用于WAP浏览（HTTP）服务
所以CMWAP接入时只能访问GPRS网络内的`IP（10.*.*.*）`，而无法通过路由访问Internet。我们用CMWAP浏览Internet上的网页就是通过WAP网关协议或它提供的HTTP代理服务实现的。


目前，中国移动的WAP网关对外只提供HTTP代理协议（80和8080端口）和WAP网关协议（9201端口）。
因此，只有满足以下两个条件的应用才能在中国移动的CMWAP接入方式下正常工作：
1. 应用程序的网络请求基于HTTP协议。
2. 应用程序支持HTTP代理协议或WAP网关协议。

### CMNET:
CMNET国家骨干网部分由北京、上海、广州、南京、武汉、成都、西安、沈阳八大省会城市节点构成。
通过CMNET接入点可以接入中国移动CMNET网络，获得完全的Internet访问权。






## 现在的手机接入点：
QQBrower_x5/Subproject/CommonBaseModule/src/com/tencent/common/http/Apn.java
现在移动4G和联通4G的接入点分别是CMNET 和 3GNET,电影应该是全面变成ctnet和ctlte
移动WAP
联通3G-wap
联通wap
电信net
联通net
联通3G-net
移动net
电信4G
联通4G
移动4G


## 项目中手机网络接入类型
NET WAP WIFI
