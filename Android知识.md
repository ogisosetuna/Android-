# Android-
## 1.四大组件是什么
    activity、service、contentprovider，broadcastReceive
## 2.四大组件的生命周期和简单用法
    Activity：屏幕中能够与用户交互的一些界面
    生命周期：oncreate、onstart、onresume、onpause、onstop、ondestroy
    Service：没有界面、生命周期比较长的程序，一般用来监控类程序（后台播放音乐）。Onstart：server独立运行在后台。Onbind：绑定activity。
    Contentprovider：用于数据共享，让一个应用程序指定数据集提供给其他应用程序。（通讯录数据被多个应用使用，多进程通信）
    BroadcastReceive广播接收器，主要应用在程序之间传输信息的机制。普通，有序，异步广播。静态：无论程序是否启动。动态：只有程序运行时才接受广播。
## 3.Activity之间的通信方式
    在Intent跳转时携带数据
    借助类的静态变量
    借助全局变量 Application
    借助Service服务
    借助外部存储来实现通讯
    * Activity和Fragment之间的通信：bundle传递，在Fragment和Actiivty关联后，调用Activity中的public修饰的方法，广播，定义接口，文件
    * Activity和Service之间的通信：通过bundle传递数据，startService(intent)和bindService（）方法启动服务，通过binder，通过messenger
    * 进程间通讯的方式：文件、AIDL、Binder、Messenger、ContentProvider、Socket
    * Linux 进程间通信方法： 匿名管道、有名管道、消息队列、共享内存、套接字、信号量、信号
        
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
## 21.wait 和 sleep的区别
    sleep是让线程休眠，wait是等待。
    * 使用上：从使用角度看，sleep是Thread线程类的方法，而wait是Object顶级类的方法。
    sleep可以在任何地方使用，而wait只能在同步方法或者同步块中使用。
    * CPU及资源锁释放: sleep,wait调用后都会暂停当前线程并让出cpu的执行时间。
    但sleep不会释放当前持有的对象的锁资源，到时间后会继续执行，而wait会放弃所有锁并需要notify/notifyAll后重新获取到对象锁资源后才能继续执行
## 22.Handler机制，looper相关
    一套 Android 消息传递机制 / 异步通信机制
    Handler封装了消息的发送：内部会跟Looper关联
    Looper（消息封装的载体）：内部包含一个消息队列（MessageQueue），所有Handler发送的消息都会走向这个消息队列；
    Looper.Looper方法是一个死循环，不断的从MessageQueue取消息，如果有消息就处理消息，没有消息就阻塞
    MessageQueue（消息队列）：可以添加消息，并处理消息

    子线程可以创建handler。一个线程可以有多个handler。只能有一个looper。
    调用looper.prepare时创建looper。Looper中有一个sThreadLocal变量，ThreadLocal用于存储上下文信息。Threadlocal用final static修饰。
    
    内存泄露原因：当Handler消息队列 还有未处理的消息 / 正在处理消息时，存在引用关系： “未被处理 / 正处理的消息 -> Handler实例 -> 外部类”
    若出现 Handler的生命周期 > 外部类的生命周期 时（即 Handler消息队列 还有未处理的消息 / 正在处理消息 而 外部类需销毁时），将使得外部类无法被垃圾回收器（GC）回收
    主线程不会因为Looper.loop()方法造成阻塞，ActivityThread的main方法主要就是做消息循环。
    ThreadLocal类提供了线程的局部变量。
## 23.多线程的方式有哪些
    继承Thread类（实现简单，局限性大）
    实现Runnable接口（适合资源共享，灵活）
    实现Callable重写call方法
## 24.Handler、Thread和HandlerThread的差别
    线程间通信的时候，比如Android中常见的更新UI，涉及到的是子线程和主线程之间的通信，实现方式就是Handler+Looper，但是要自己手动操作Looper，不推荐，所以谷歌封装了HandlerThread类.
## 25.view事件分发
    dispatchTouchEvent代表分发事件，onInterceptTouchEvent()代表拦截事件，onTouchEvent()代表消耗事件，由自己处理。
    默认状态下事件是按照从Activity到ViewGroup再到View的顺序进行分发的，分发下去处不处理是另一回事。
    分发完成后，不处理则向上一层回调，调用上一层的onTouchEvent进行处理事件，若onTouchEvent返回true，则表示在该层消耗了事件，若返回false，表示事件还没被处理，需要再向上回调一层，调用上一层的onTouchEvent方法。
    https://juejin.cn/post/6844903894103883789#comment
## 26.自定义View
    继承view方式，重写方法onMeasure、 onLayout、onDraw、onTouchEvent
## 27.EventBus和ottobus区别 基于观察者模式
    观察者模式：被观察者，产生事件；观察者，消费事件。
    在Android里面EventBus时常用来在各模块之间解耦。例子：密码输入框，不需要知道谁会处理这个事件、怎么处理，这符合设计模式的原则：单一职责、最小知道原则。
    两者基本用法类似。1.定义事件类 2.在观察者（处理者）类中定义@subscribe public方法，并注册此类的对象到EventBus中 3.在合适的地方post响应的事件 4.取消注册观察者。
    差别：
    *订阅方法不同。
    *观察者方法执行的线程不同。Otto，默认你创建的Bus，只能在UI线程使用，也通过ThreadEnforcer接口开放了自定义的能力，post方法在哪个线程被调用，观察者方法就在哪个线程中执行。EventBus现在可以支持在每一个事件上配置线程策略、sticky与否、事件优先级。
    *post方法执行不同。otto中post是同步操作。此方法返回的时候，所有感兴趣的观察者方法都已经被执行完毕。EventBus根据情况，可同步可异步。

## 28. okhttp架构相关
    *HTTP报文结构：
        请求报文：请求行，多个请求头，一个空行，实体内容。
        响应报文：状态行，多个响应头，一个空行，实体内容。
    *okhttp的优点：
        1)支持http2，对一台机器的所有请求共享同一个socket
        2)内置连接池，支持连接复用，减少延迟
        3)支持透明的gzip压缩响应体
        4)通过缓存避免重复的请求
        5)请求失败时自动重试主机的其他ip，自动重定向
        6)丰富的API，可扩展性好



# 多人白板
## 自定义view
    继承view，重写onDraw，重写时需要用到 API Canvas
    onMeasure()方法， onLayout()方法， onDraw()方法。
    * onMeasure()方法： 单一View， 一般重写此方法， 针对wrap_content情况， 规定View默认的大小值， 避免于match_parent情况一致。 ViewGroup， 若不重写， 就会执行和单子View中相同逻辑， 不会测量子View。 一般会重写onMeasure()方法， 循环测量子View。
    * onLayout()方法:单一View， 不需要实现该方法。 ViewGroup必须实现， 该方法是个抽象方法， 实现该方法， 来对子View进行布局。View测量、 布局及绘制原理
    * onDraw()方法： 无论单一View， 或者ViewGroup都需要实现该方法， 因其是个空方法

## socket（套接字）
    Socket是对TCP/IP协议的封装，是一个调用接口API。在TCP/IP协议中主要有Socket类型的流套接字StreamSocket和数据报套接字DatagramSocket。
    * 流套接字将TCP作为其端到端的协议，提供一个可信赖的字节流服务。
    * 数据报套接字使用UDP协议，提供数据打包发送服务。

    流程 创建serversocket -> accept（）等待接受请求 -> 接收到请求后链接socket。
## OSI 七层网络模型
    物理层、数据链路层、网络层、传输层、会话层、表示层、应用层。
    TCP/IP 模型在 OSI 模型的基础上进行了简化，变成了四层，从下到上分别为：网络接口层、网络层、传输层、应用层。
    Socket 作为应用层和传输层之间的桥梁，与之关系最大的两个协议就是传输层中的 TCP 和 UDP协议。
## TCP 三次握手，四次挥手
    3次握手完成两个重要的功能，既要双方做好发送数据的准备工作(双方都知道彼此已准备好)，也要允许双方就初始序列号进行协商，这个序列号在握手过程中被发送和确认。
    https://res-static.hc-cdn.cn/fms/img/9cb42dacc00e1313bee6141b41db1e341603777814954.png
## popupwindow和alertDialog区别
    AlertDialog 是非阻塞式对话框；而PopupWindow 是阻塞式对话框。AlertDialog 弹出时，后台还可以做事情；PopupWindow 弹出时，程序会等待，在PopupWindow 退出前，程序一直等待，只有当我们调用了 dismiss() 方法的后，PopupWindow 退出，程序才会向下执行。我们在写程序的过程中可以根据自己的需要选择使用 Popupwindow 或者是 Dialog.
    两者最根本的区别在于有没有新建一个window，PopupWindow 没有新建，而是通过 WMS 将 View 加到 DecorView；Dialog 是新建了一个 window (PhoneWindow)，相当于走了一遍 Activity 中创建 window 的流程。
## TCP协议-如何保证传输可靠性
    * 校验和 计算方式：在数据传输的过程中，将发送的数据段都当做一个16位的整数。将这些整数加起来。并且前面的进位不能丢弃，补在后面，最后取反，得到校验和。
    * 序列号 确认应答和序列号 ACK
    * 超时重传 最大超时时间（也就是等待的时间）是动态计算的。
    * 连接管理 三次握手四次挥手
    * 流量控制 TCP根据接收端对数据的处理能力，决定发送端的发送速度
    * 拥塞控制 慢启动 拥塞窗口
## HTTP状态码
    * 200 - 请求成功
    * 301 - 资源（网页等）被永久转移到其它URL
    * 404 - 请求的资源（网页等）不存在
    * 500 - 内部服务器错误
    * 1xx：指示信息--表示请求已接收，继续处理
    * 2xx：成功--表示请求已被成功接收、理解、接受
    * 3xx：重定向--要完成请求必须进行更进一步的操作
    * 4xx：客户端错误--请求有语法错误或请求无法实现
    * 5xx：服务器端错误--服务器未能实现合法的请求
## HTTP和HTTPS区别
    HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，要比http协议安全。
    HTTP（超文本传输协议）是一个基于请求与响应模式的、无状态的、应用层的协议，常基于TCP的连接方式
## 常用的HTTP方法有哪些？
    GET： 用于请求访问已经被URI（统一资源标识符）识别的资源，可以通过URL传参给服务器。
    POST：用于传输信息给服务器，主要功能与GET方法类似，但一般推荐使用POST方式。
    PUT： 传输文件，报文主体中包含文件内容，保存到对应URI位置。
    get和post的区别：
    * get重点在从服务器上获取资源，post重点在向服务器发送数据
    * Get传输的数据量小，因为受URL长度限制，但效率较高；Post可以传输大量数据，所以上传文件时只能用Post方式
    * get是不安全的，因为URL是可见的，可能会泄露私密信息，如密码等；post较get安全性较高
    * get方式只能支持ASCII字符，向服务器传的中文字符可能会乱码。post支持标准字符集，可以正确传递中文字符。
    
    get把请求的数据放在url上即HTTP协议头,而post会把数据放在HTTP的包体内。
    对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）；
    而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok（返回数据）。
## HTTP优化
    利用负载均衡优化和加速HTTP应用
    利用HTTP Cache来优化网站
## HTTP请求报文与响应报文格式
    请求报文包含三部分：
    a、请求行：包含请求方法、URI、HTTP版本信息
    b、请求首部字段
    c、请求内容实体
    响应报文包含三部分：
    a、状态行：包含HTTP版本、状态码、状态码的原因短语
    b、响应首部字段
    c、响应内容实体
## HTTP1.1版本新特性
    a、默认持久连接节省通信量，只要客户端服务端任意一端没有明确提出断开TCP连接，就一直保持连接，可以发送多次HTTP请求
    b、管线化，客户端可以同时发出多个HTTP请求，而不用一个个等待响应
    c、断点续传原理
    HTTP1.1协议（RFC2616）中定义了断点续传相关的HTTP头 Range 和 Content-Range 字段
## B+树
    Mysql索引使用B+树。
    事务
    4个原则：原子性，隔离性，一致性，持久性
    4个隔离级别：读取非提交，读取已提交，可重复读，可串行性


