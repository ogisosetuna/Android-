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
## 4.Activity各种情况下的生命周期，加载模式，什么时候会用到singleTask？
加载模式分为四种<br>
    Standard：在Activity的栈中无论该活动有没有加入栈，活动就会被创建。（邮件）
    SingleTop：只要被创建的活动不位于栈的顶部，该活动就会被创建入栈。（登陆界面，推送通知）
    SingleTask：单任务模式。（微信主界面：分享功能）
    SingleInstance：堆内单例，手机系统内只有一个实例

Activity四种状态：活动，暂停，停止，待用<br>
正常home后重启 onPause -->onStop |||||| onRestart -->onStart -->onResume
横竖屏切换：onPause -->onSaveInstanceState -->onStop -->onDestroy -->onCreate-->onStart -->onRestoreInstanceState-->onResume -->onPause -->onStop -->onDestroy
如果有设置configChanges	，只调用onConfigurationChanged方法。 <br>
普通AlertDialog弹出，不会发生activity声明周期变化<br>
由于Fragment依附于Activity，所以启动的时候Activity的方法肯定在前面，Fragment的方法在后面，但是在要销毁的时候，Fragment的方法先执行，再执行Activity的方法。    

