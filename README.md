# iOS-SWIFT-OLD-DRIVER-Summary

### About

2020阅读笔记  

人无恒产则无恒心，所以总是要学会去经营点什么。最近受挫很多，有时候觉得川建国那种脸皮比地壳厚的精神d挺值得学习的  

文档内容纵向为时间，横向为分类  

## 日期

### 4-12
* [iOS启动时间优化](http://www.zoomfeng.com/blog/launch-time.html)   

 摘要：通常针对一个技术点做优化的时候，都要先了解清楚这个技术点有哪些流程，优化的方向往往是减少流程的数量，以及减少每个流程的消耗。

一 app启动流程  
1.解析Info.plist  
加载相关信息，例如闪屏  
沙箱建立、权限检查  
2.Mach-O加载   
如果是胖二进制文件，寻找合适当前CPU架构的部分  
加载所有依赖的Mach-O文件（递归调用Mach-O加载的方法）  
定位内部、外部指针引用，例如字符串、函数等  
加载类扩展（Category）中的方法  
C++静态对象加载、调用ObjC的 +load 函数  
执行声明为__attribute__((constructor))的C函数  
3.程序执行  
调用main()  
调用UIApplicationMain()  
调用applicationWillFinishLaunching  

二 优化策略  
1.减少非系统库的依赖  
2.检查下 framework应当设为optional和required  
3.减少Objc类数量， 减少selector数量，把未使用的类和函数都可以删掉   
4.减少C++虚函数数量  
5.使用 +initialize 来替代 +load  
6.少用C++构造函数和静态变量     

7.减少启动初始化的流程，能懒加载的就懒加载，能放后台初始化的就放后台  
能够延时初始化的就延时，不要卡主线程的启动时间，已经下线的业务直接删掉  
8.优化代码逻辑，去除一些非必要的逻辑和代码，减少每个流程所消耗的时间  
9.启动阶段使用合理的多线程来进行初始化，把CPU的性能尽量发挥出来  
10.使用纯代码而不是xib或者storyboard来进行UI框架的搭建  

* [iOS 界面性能优化浅析](https://coderzsq.github.io/2018/07/iOS-%E7%95%8C%E9%9D%A2%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96%E6%B5%85%E6%9E%90/)   
对于界面的性能优化, 简单的说就是保持界面流畅不掉帧, 当然原理这种网上一搜一大把   有空的话看看YYKit也就能够知晓个大概. 硬是要说原理的话   就是当Vsync信号来临的16.67ms内CPU做完排版, 绘制, 解码, GPU避免离屏渲染之类的   就会在Runloop下一次来临的时候渲染到屏幕上.  

* [《Effective Objective-C》干货三部曲（一）：概念篇](https://juejin.im/post/5a4f34226fb9a01cb0492016)   

一.概念篇  
1.isEqual比较值，==比较内存地址 
2.深度等同性判定 比较两个数组是否相等的话可以使用深度同等性判断方法：  3.先比较数组的个数  
4.再比较两个数组对应位置上的对象均相等。 
5.子类EOCSubClass并没有覆写initialize方法，那么自然会调用其父类EOCBaseClass的方法  
6.使用“类族”(class cluster)这一设计模式，通过“抽象基类”来实例化不同的实体子类，隐藏实现细节，抽象工厂
等，比较基础  
其他两篇不怎么深入，不概括了

### 4-13  

* [BLE传输大数据](https://www.jianshu.com/p/b71e9394a60a)  
蓝牙相关  
绑定方式2种，连接上了不需要配对和连接上了需要配对才能传输数据  
蓝牙开发遇到的问题：  
1.重连问题，添加标记位，在断开后3-5秒开始重连   
2.大数据传输的问题，分包，蓝牙是小端传输，最大传输单元是20字节  
3.断开方式 用户主动断开，其它原因断开，主动断开不需要自动重连  
4.蓝牙数据结构，传输头，包含传输的总数据大小等信息。每段数据增加排列位置标记，最后一个是e结束包
5.蓝牙断开大概需要5秒左右  
6.CBCentralManager对象不可以同时存在多个，设备最多连接7个
7.设备UUID在不同的设备上不一样，在同一个设备上一样

* [iOS绘图框架CoreGraphics分析](http://www.cocoachina.com/articles/20187)   
UIKit依赖CoreGraphics，CoreGraphics会用到Quart 2D的api  
绘图基本就是对API的理解和灵活搭配，主要非OC对象创建后需要自己去释放

### 4-14  
* [基于LLVM开发属于自己Xcode的Clang插件](https://www.cnblogs.com/guwudao/p/9492022.html)  
LLVM插件开发的质量依赖于开发者对编译器编译原理和编译器各种API，语法书等的了解熟悉程度，其基本开发流程如下：
1.下载LLVM，clang,编译工具使用ninja和cmake
2.xcode编译生产新的LLVM
3.修改LLVM的一些函数等
4.替换原有的Xcode的LLVM

### 4-23
* [iOS Memory Deep Dive](https://www.cnblogs.com/guwudao/p/9492022.html)  
内存管理两个开发框架  
1. MLeaksFinder 腾讯  
2. FBRetainCycleDetector FB  
高效使用内存：   
1.避免内存泄漏  
2.复用机制  
3.非OC对象创建后需要手动释放  
4.懒加载  
5.使用自动释放池避免提前释放  
6.内存警告的时候主动释放   

* [巧妙实现 debugOnly 函数](https://kemchenj.github.io/2018-09-24/)   
condition 由于 @autoclosure 的标记会把传入的值自动装到闭包里，然后只有在 debug 模式下才会执行并且求值，通过这种方式就可以很完美地实现一个 debugOnly 函数。

* [Hook所有+load方法（包括Category）](https://mp.weixin.qq.com/s/kL__CM3CfP_7i8Obg8qzWQ) 
在做启动优化的时候可以用到



## 分类
- [优化@](#优化)
- [基础@](#基础)
- [硬件相关@](#硬件相关)
- [工具开发@](#工具开发)
- [内存管理@](#内存管理)
- [小技巧@](#小技巧)

#### 优化@
* [iOS启动时间优化](http://www.zoomfeng.com/blog/launch-time.html) 
* [iOS 界面性能优化浅析](https://coderzsq.github.io/2018/07/iOS-%E7%95%8C%E9%9D%A2%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96%E6%B5%85%E6%9E%90/) 

#### 基础@
* [《Effective Objective-C》干货三部曲（一）：概念篇](https://juejin.im/post/5a4f34226fb9a01cb0492016) 
* [iOS绘图框架CoreGraphics分析](http://www.cocoachina.com/articles/20187)   

#### 硬件相关@
* [BLE传输大数据](https://www.jianshu.com/p/b71e9394a60a)  

#### 工具开发@
* [基于LLVM开发属于自己Xcode的Clang插件](https://www.cnblogs.com/guwudao/p/9492022.html) 

#### 内存管理@
* [iOS Memory Deep Dive](https://www.cnblogs.com/guwudao/p/9492022.html)  

#### 小技巧@
* [巧妙实现 debugOnly 函数](https://kemchenj.github.io/2018-09-24/)   
* [Hook所有+load方法（包括Category）](https://mp.weixin.qq.com/s/kL__CM3CfP_7i8Obg8qzWQ)
