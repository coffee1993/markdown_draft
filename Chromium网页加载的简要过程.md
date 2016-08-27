## Chromium网页URL加载过程
对于新人来说，Chromium网页加载容易误会为网页的请求资源过程，其实广义上的网页加载表示

- Browser进程的请求网页资源内容
- Render进程的解析和渲染，排版。

小弟先从网的请求资源入手来探究URL是如何通过浏览器向服务器发送资源请求，然后下载原始的网页资源数据。

首先需要弄明白：Chromium的模块划分及其层次关系
参见：Chromium的模块划分及其层次关系.md

相关官方链接：
[**How Chromium Displays Web Pages**](https://www.chromium.org/developers/design-documents/displaying-a-web-page-in-chrome)


### URL加载过程需要两个进程Browser和Render

关于进程，参见多进程架构及多进程架构之间的通信

简单介绍：
Chromium在Browser进程中为网页创建了一个Frame Tree之后，会将网页的URL发送给Render进程进行加载。Render进程接收到网页URL加载请求之后，会做一些必要的初始化工作，然后请求Browser进程下载网页的内容。Browser进程一边下载网页内容，一边又通过共享内存将网页内容传递给Render进程解析，也就是创建DOM Tree。
Render进程出于安全的考虑，是没有网络访问权限的，需要请求Browser进程进行资源的请求下载。

#### 为什么Browser不会主动将网页资源下载好交给Render进程解析。

因为 **Webkit** 在Render进程中，Render进程中的Content模块需要使用Webkit中的特定接口访问网络，加载网页URL。为了使得Webkit可以在有网络权限的进程中和没有网络权限的进程中应用，Webkit不关心自己所在的进程是否有网络权限。


#### 进程中HTTP协议请求服务器是"何时何地"发送

1 Browser进程中为需要加载的网页创建FrameTree。
2 Browser进程向Render进程发送FrameMsg_Navigate IPC消息，Render进程执行相关初始化操作
3 Render进程执行完初始化操作向Browser进程发送ResourceHostMsg_RequestResource IPC消息
4 Browser进程收到消息后，进行HTTP请求得到Web服务器数据，然后创建内存共享块。
5 Browser进程发送一个ResourceMsg_SetDataBuffer将内存共享块传递给Render进程
6 Browser进程每次收到web服务器的数据就写入共享内存，然后发送ResourceMsg_DataReceived消息给render进程。
7 Render进程收到消息后读出共享内存内容进行解析构造DOM Tree。这是一个循环过程，直到网页内容下载完成为止。

Browser -> FrameMsg_Navigate ->Render
Browser <- ResourceHostMsg_RequestResource <- Render
Browser -> ResourceMsg_SetDataBuffer -> Render
Browser -> ResourceMsg_DataReceived -> Render


其中 总的流程为图：Chromium_URL加载1 ，第四部比较复杂，单独列为 Chromium_URL加载2
