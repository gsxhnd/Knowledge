# Promtail 介绍



Promtail 将本地日志内容传送到 Loki 实例。需要监控的应用程序的机器上都需要部署该组件。

它的主要工作流程可以划分为：

-   使用 fsnotify 监听指定目录下（例如：/var/log/*.log）的文件创建与删除
-   对每个活跃的日志文件起一个 goroutine 进行类似 tail -f 的读取，读取到的内容发送给 channel
-   有一个单独的 goroutine 会读取 channel 中的日志行，分批并附加上标签后推送给 Loki

![promtail原理](https://ask.qcloudimg.com/http-save/yehe-3322396/c783d414bf108d794d59ef6ab72ee7c9.jpeg?imageView2/2/w/2560/h/7000)