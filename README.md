# TeacherAssiatant详解：教你如何一步一步的实现功能
## 1. 登录功能（如何使用云服务实现联网登录）</br>
    源码:[loginDemo点击下载源码](https://github.com/weiyashuai123/TeacherAssiatant-detailed/raw/master/LoginDemo.zip)</br>
    首先我们需要使用移动云服务来储存数据.打开Bmob官网：[http://www.bmob.cn/](http://www.bmob.cn/) 注册一个账号并登陆</br>
    在右上角点开我的控制台</br>
    ![](https://github.com/weiyashuai123/TeacherAssiatant-detailed/blob/master/image/0.png "bmob")</br>
    创建一个应用:</br>
    ![](https://github.com/weiyashuai123/TeacherAssiatant-detailed/blob/master/image/6.png "bmob")
    ![](https://github.com/weiyashuai123/TeacherAssiatant-detailed/blob/master/image/1.png "bmob")</br>
    在数据中可以看到我们的数据库表 系统一般会默认创建一个 User表 </br>
    我们现在先往这个表中添加一条数据 </br>
    点击添加行 添加一条用户名为teacher.密码为123456的用户（注意这个objectid 不要填写 是自动生成的）</br>
    ![](https://github.com/weiyashuai123/TeacherAssiatant-detailed/blob/master/image/5.png "bmob")</br>
    在左下角点开设置:</br>
    可以看到我们的application id 一会会用到这个![](https://github.com/weiyashuai123/TeacherAssiatant-detailed/blob/master/image/3.png "bmob")</br>
    现在我们建立一个工程，创建两个Activity LoginActivity和MainActivity 两个layout（布局文件）activity_main 和 activity_login.</br>
    并在login的布局中添加两个编辑框用于用户输入用户名和密码.一个登陆按钮用于登录操作</br>
    现在我们先来配置一下刚才云服务的SDK</br>在 Project 的 build.gradle 文件中添加 Bmob 的maven仓库地址：</br>
    `allprojects { `</br>
        `  repositories {`</br>
        `  jcenter()`</br>
        `  maven { url "https://raw.github.com/bmob/bmob-android-sdk/master" }`</br>
        `  }`</br>
        `  }`</br>
    在app的build.gradle文件中添加compile依赖文件</br>
    `android {`</br>
       `  compileSdkVersion 25`</br>
       `  buildToolsVersion '25.0.0'`</br>
       `  useLibrary 'org.apache.http.legacy'`</br>
        ` }`</br>
    `dependencies `{</br>
       `  compile fileTree(dir: 'libs', include: ['*.jar'])`</br>
       `  compile 'cn.bmob.android:bmob-sdk:3.5.0'`</br>
    `}`</br>
   在minifest中声明以下权限：</br>
`<uses-permission android:name="android.permission.INTERNET" /> `</br>
`<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" /> `</br>
`<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" /> `</br>
`<uses-permission android:name="android.permission.WAKE_LOCK" /> `</br>
`<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />`</br>
`<uses-permission android:name="android.permission.READ_PHONE_STATE" />` </br>
   好的 现在我们来实现登陆 在LoginActivity的oncreate中加入：Bmob.initialize(this, "Application ID");</br>
   这里的Application id 就是你刚才创建的应用的application id;</br>
   我们继续在 按钮的点击事件中写入登陆事件</br>
    ![s](https://github.com/weiyashuai123/TeacherAssiatant-detailed/blob/master/image/4.png "bmob") </br>
   好的 现在运行一下程序 可以看到已经实现了登陆功能 </br>
    ![s](https://github.com/weiyashuai123/TeacherAssiatant-detailed/blob/master/image/logindemo.gif "bmob")</br>
## 2.注册与签到（扩展User类以及往数据库中写入数据）（上）
  源码:[Demo2点击下载源码](https://github.com/weiyashuai123/TeacherAssiatant-detailed/raw/master/Demo2Register.zip)</br>
  首先我们来对User类来进行扩展，假设我们需要给教师用户添加一个“性别”属性.</br>
  建立一个teacher类继承自BmobUser：</br>
  `public class Teacher extends BmobUser{}`</br>
  在类中写入我们新建的属性：</br>
  `private String sex;`</br>
  重写构造器并设置set，get方法：</br>
  ![Teacher](https://github.com/weiyashuai123/TeacherAssiatant-detailed/blob/master/image/2.2.png "teacher")</br>
  我们简单的扩展已经完成，接下来我们实现注册功能：</br>
  注册界面不说了，两个edittext用于输入用户名与密码，radiobutton用于选择性别，一个button用于注册</br>
   ![Register](https://github.com/weiyashuai123/TeacherAssiatant-detailed/blob/master/image/2.4.png "register")</br>
  直接来介绍注册按钮的点击事件：</br>
  ![Register](https://github.com/weiyashuai123/TeacherAssiatant-detailed/blob/master/image/2.3.png "register")</br>
  new一个Teacher对象 设置其用户名密码性别，然后调用sign up方法，好的到这里注册功能就实现了，我们来运行一下：</br>
  ![Register](https://github.com/weiyashuai123/TeacherAssiatant-detailed/blob/master/image/regis.gif "register")</br>
  打开我们的Bmob后台数据库表可以看到刚才注册的用户：</br>
  ![Register](https://github.com/weiyashuai123/TeacherAssiatant-detailed/blob/master/image/2.5.png "register")</br>
## 3.注册与签到（扩展User类以及往数据库中写入数据）（下）
  源码:[Demo3点击下载源码]()</br>
  接下来我们来实现一个签到功能：</br>
  我们签到需要保存什么信息呢，假设我们签到信息要保存：签到时间、签到用户、签到地点这三个信息.</br>
  那么首先创建一个继承自BmobObject的JavaBean用于存放签到信息：</br>
  为什么没有定义签到时间这个属性呢，因为父类BmobObject中已经有createAt这个属性，用来保存数据的创建时间，我们可以直接拿来用。</br>
  接下来在MainActivity中添加一个按钮用于执行签到操作</br>
  好了，来看我们签到按钮的点击事件：</br>
  使用BmobUser类执行用户登录操作有一个好处是在登录后我们可以在程序中随时调出该用户的信息</br>
  在这个点击事件里：首先我们通过BmobUser的getCurrentUser方法获取到当前登录的用户</br>
  接下来我们new一个SignInfo对象用于保存签到信息</br>
  然后通过save方法保存这个信息.</br>
  好了 现在运行一下程序：</br>
  查看一下后端云数据库：</br>
  会发现多了一张表。表中存放着我们刚才的签到信息。</br>
  好了 到这里 签到的功能就实现了</br>
