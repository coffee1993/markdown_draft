## Chromium多进程架构及多进程架构之间的通信

### 前言

参见：
[多进程架构官方文档 Multi-process Architecture](https://www.chromium.org/developers/design-documents/multi-process-architecture)

## 首先知道Chromium的四个进程

- Browser进程、Render进程、GPU进程和Plugin进程
- 在一个Chromium实例下，1 Browser，GPU ，N Render，Plugin。
- 每一个进程除了自身的主线程，都有IO线程来进行进程之间的通信。
- 进程通信关系是：plugin进程不会和GPU进程直接通信需要Render进程转发OpenGL命令，其他都两两之间进行 **IPC通信**

![进程间通信图](F:\markdown文件夹\image\LayoutOnDisk_net.png)

### Browser进程到底干了些什
Browser进程负责合成浏览器的UI，包括标题栏、地址栏、工具栏以及各个TAB的网页内容。
Browser进程是直接将UI渲染在Frame Buffer上，也就是屏幕上。
也就是说：Render进程渲染好的网页UI要经过Browser进程合成之后，才能在屏幕上看到。

### Render进程
Render进程负责解析和渲染网页的内容，还有JS的执行。Render是一个Service进程。


### Plugin进程的
Plugin进程，需要将一些网页的事件发送给它处理，这样Render进程就需要与Plugin进程进行通信。总的来说就是用来运行第三方开发的Plugin，以便可以扩展浏览器的功能。
例如，Flash就是一个Plugin，它运行在独立的Plugin进程中。

### GPU进程
Browser 或者 Render启动了硬件加速 都需要GPU进程来渲染UI
