# Android-
## 1.四大组件是什么
activity、server、contentprovider，broadcast<br> 
## 2.四大组件的生命周期和简单用法
Activity：屏幕中能够与用户交互的一些界面<br>
生命周期：oncreate、onstart、onresume、onpause、onstop、ondestroy<br>
Server：没有界面、生命周期比较长的程序，一般用来监控类程序（后台播放音乐）。Onstart：server独立运行在后台。Onbind：绑定activity。<br>
Contentprovider：用于数据共享，让一个应用程序指定数据集提供给其他应用程序。（通讯录数据被多个应用使用，多进程通信）<br>
Broadcast：广播，主要应用在程序之间传输信息的机制。普通，有序，异步广播。静态：无论程序是否启动。动态：只有程序运行时才接受广播。<br>
## 3.Activity之间的通信方式
在Intent跳转时携带数据<br>
借助类的静态变量<br>
借助全局变量 Application<br>
借助Service服务<br>
借助外部存储来实现通讯<br>
