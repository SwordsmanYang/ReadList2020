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

![](http://www.zoomfeng.com/images/2018/07/3.png)  

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


## 分类
- [优化@](#优化)

#### 优化@
* [iOS启动时间优化](http://www.zoomfeng.com/blog/launch-time.html) 
