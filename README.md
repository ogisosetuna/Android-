# Android-
## 1.四大组件是什么
activity、server、contentprovider，broadcast<br> 
## 2.四大组件的生命周期和简单用法
Activity：屏幕中能够与用户交互的一些界面<br>
生命周期：oncreate、onstart、onresume、onpause、onstop、ondestroy<br>
Service：没有界面、生命周期比较长的程序，一般用来监控类程序（后台播放音乐）。Onstart：server独立运行在后台。Onbind：绑定activity。<br>
Contentprovider：用于数据共享，让一个应用程序指定数据集提供给其他应用程序。（通讯录数据被多个应用使用，多进程通信）<br>
BroadcastReceive广播接收器，主要应用在程序之间传输信息的机制。普通，有序，异步广播。静态：无论程序是否启动。动态：只有程序运行时才接受广播。<br>
## 3.Activity之间的通信方式
    在Intent跳转时携带数据
    借助类的静态变量
    借助全局变量 Application
    借助Service服务
    借助外部存储来实现通讯
## 4.Activity各种情况下的生命周期，加载（启动）模式，什么时候会用到singleTask？
    加载模式分为四种
    Standard：在Activity的栈中无论该活动有没有加入栈，活动就会被创建。（邮件）
    SingleTop：只要被创建的活动不位于栈的顶部，该活动就会被创建入栈。（登陆界面，推送通知）
    SingleTask：单任务模式。（微信主界面：分享功能）
    SingleInstance：堆内单例，手机系统内只有一个实例

Activity四种状态：活动（可见），暂停（可见无焦点），停止（不可见），待用<br>
正常home后重启 onPause -->onStop |||||| onRestart -->onStart -->onResume
横竖屏切换：onPause -->onSaveInstanceState -->onStop -->onDestroy -->onCreate-->onStart -->onRestoreInstanceState-->onResume -->onPause -->onStop -->onDestroy
如果有设置configChanges	，只调用onConfigurationChanged方法。 <br>
普通AlertDialog弹出，不会发生activity声明周期变化<br>
由于Fragment依附于Activity，所以启动的时候Activity的方法肯定在前面，Fragment的方法在后面，但是在要销毁的时候，Fragment的方法先执行，再执行Activity的方法。    
## 5.两个Activity 之间跳转时必然会执行的是哪几个方法？
    当在A里面激活B组件的时候, A会调用onPause()方法,然后B调用onCreate() ,onStart(), onResume()。
    这个时候B覆盖了A的窗体, A会调用onStop()方法。
    如果B是个透明的窗口,或者是对话框的样式, 就不会调用A的onStop()方法。
    如果B已经存在于Activity栈中，B就不会调用onCreate()方法。
## 6.Activity的状态保存和恢复
* 什么时候调用Activity的onSaveInstanceState()<br>
屏幕旋转重建会调用onSaveInstanceState()<br>
启动另一个activity: 当前activity在离开前会调用onSaveInstanceState()<br>
按Home键的情形和启动另一个activity一样, 当前activity在离开前会onSaveInstanceState()<br>
* 什么时候不调用Activity的onRestoreInstanceState()
屏幕旋转重建会调用onRestoreInstanceState()<br>
启动另一个activity，返回时如果因为被系统杀死需要重建, 则会从onCreate()重新开始生命周期, 调用onRestoreInstanceState()<br>
按Home键的情形和启动另一个activity一样，用户再次点击应用图标返回时, 如果重建发生, 则会调用onCreate()和onRestoreInstanceState()<br>
## 7.Fragment状态保存startActivityForResult是哪个类的方法，在什么情况下使用
    onActivityResult方法接受startActivityForResult方法发出的数据请求。
    Fragment和FragmentActivity都能接收到自己的发起的请求所返回的结果
    FragmentActivity发起的请求，Fragment完全接收不到结果
    Fragment发起的请求，虽然在FragmentActivity中能获取到结果，但是requestCode完全对应不上
## 8.如何实现Fragment的滑动，如何实现Fragment之间的数据传递
    Viewpager + Fragment
    方式一：是通过fragment共有的activity来实现数据传递
    方式二：可以使用bundle进行参数传递、这样在两个Fragment跳转的时候就可以带上参数了、同样也可以传递一个复杂的对象。通过fragment.setArguments(bundle)来实现
## 9.Service相关：怎么和activity绑定，怎么在activity中启动自己对应的service，怎么进行数据交互？
    使用service中的onbind()函数返回service实例给activity
    通过Binder对象具体实现两个组件之间的交互 或者 通过broadcast(广播)的形式
## 10.ContentProvider相关
    * 在Android中,为了把自己程序的数据(一般是数据库)提供给其他应用程序,就通过ContentProvider提供的方法.
    内容提供者可认为是程序间共享数据的接口,新建一个类继承ContentProvider.
    * ContentProvider:内容提供者,定义增删改查(方法)和数据库关联;
    ContentResolver:内容解析者,一个app里边用于获取另一个app的数据(进行增删查改的具体数据操作)
    ContentObserver:内容观察者,另外的一个app(可以是不同于上述两个app)可以监听数据改变的消息
## 11.BroadcastReceiver相关
    主要应用在程序之间传输信息的机制。按注册方式分类，静态：无论程序是否启动都接受。动态：只有程序运行时才接受广播。
    *分类
    普通广播（Normal Broadcast）
    系统广播（System Broadcast）
    有序广播（Ordered Broadcast）
    App应用内广播（Local Broadcast）
    *本地广播和全局广播的区别
     本地广播只能够在应用程序的内部进行传递 ，并且广播接收器也只能接收来自应用程序发出的广播，解决了全局广播的安全性问题。
## 12.AlertDialog、PopupWindow 与 Activity 之间区别
    AlertDialog 是非阻塞式对话框；而PopupWindow 是阻塞式对话框
    两者最根本的区别在于有没有新建一个window。PopupWindow 没有新建，而是通过 WMS 将 View 加到 DecorView；Dialog 是新建了一个 window (PhoneWindow)
## 13.Android属性动画总结
    Google官方把动画分为
    * View Animation
    * Drawable Animation
    * Property Animation 属性动画
    或者分为逐帧动画，补间动画，属性动画
    属性动画系统可以让你定义动画的以下属性：
    * 时长
    * 时间插值
    * 重复次数
    * 动画集Animator sets
    * 帧频率
    说Java里面说list我们想到了ArrayList，说map我们想到了HashMap，说属性动画要想到ObjectAnimator
## 14.接口和回调
    先创建一个接口，假如接口中有一个抽象方法showToast（），只要是实现了ToastListener的类都必须要重写showToast（）方法。
    新建一个类来设置回调，并写一个方法来回调接口的方法，可以用于其他类来实现接口的方法
    在主界面去实现接口中未实现的方法，创建接口回调的那个类
    https://www.jianshu.com/p/eba2c839db47 回调实例
## 15.SurfaceView相关
    View和SurfaceView的区别:
    * View适用于主动更新的情况，而SurfaceView则适用于被动更新的情况，比如频繁刷新界面。
    * View在主线程中对页面进行刷新，而SurfaceView则开启一个子线程来对页面进行刷新。
    * View在绘图时没有实现双缓冲机制，SurfaceView在底层机制中就实现了双缓冲机制。
    https://www.jianshu.com/p/b037249e6d31
## 16.序列化的作用
    序列化定义：将一个类对象转换成可存储、可传输状态的过程。序列化有两个过程：
    * 序列化：将对象编码成字节流（serializing）
    * 反序列化：从字节流编码中重新构建对象（deserializing)。对象序列化后，可以在进程内/进程间、网络间进行传输，也可以做本地持久化存储。
    为什么要序列化: 系统底层并不认识对象，数据传输是以字节序列形式传递。
        ！！以进程间通信为例，需要将对象转化为字节序列(字节序列中包括该对象的类型，成员信息等)，然后在目标进程里通过反序列化字节序列，将字节序列转换成对象。
    序列化方式:
    * Serializable(Java提供 后面简称为S)
    * Parcelable(Android特有 下面简称为P)
    P基于内存进行序列化和反序列化，效率高，代码复杂。
## 17.安卓中的数据存储方式
    * 文件存储
    * SharedPreferences
    * SQLite数据库存储
    * ContentProvider
    * 网络存储
## 18.差值器和估值器
    用来实现一些动画效果，例如做个抛物线运动。
## 19.各版本API区别
    * android6.0 代号M Marshmallow
    API23，软件权限管理，安卓支付，指纹支持，App关联，
    * android7.0 代号N Preview
    API24，多窗口支持(不影响Activity生命周期)，增加了JIT编译器，引入了新的应用签名方案APK Signature Scheme v2（缩短应用安装时间和更多未授权APK文件更改保护）,严格了权限访问
    * android8.0 代号O
    API26，取消静态广播注册，限制后台进程调用手机资源，桌面图标自适应
    * android9.0 代号P
    API27，加强电池管理，系统界面添加了Home虚拟键，提供人工智能API，支持免打扰模式
## 20.invalidate()和postInvalidate() 的区别及使用
    invalidate()得在UI线程中被调动，在工作者线程中可以通过Handler来通知UI线程进行界面更新。
    而postInvalidate()在工作者线程中被调用




