# weight-reduction-ios

# iOS .ipa 文件大小优化

![](http://www.zoomfeng.com/images/2016/10/12/4.png)

****************************
减少图片数量：减少图片生成，只采用 @3x。

减少不必要的系统和架构的适配。

****************************
资源优化：

去除无用资源：  
相同的资源但名字不同的；  
未使用的图片；    
一倍图资源；      
代码里面一些无用的文件，例如RENAME等。  

资源压缩：     
图片无损/有损压缩；[官网]（https://imageoptim.com/howto.html）  

音频压缩；         

使用ASSETS.XCASSETS来管理图片；         

h5页面远程化；  

动态下载资源；（字体、图片、配置数据等非必须资源） 

****************************

编译选项优化（Build Settings编译选项）： 

Optimization Level设置为Fastest,Smallest;(默认release是此选项)   
（Optimization Level 应该是编译器的优化程度。比较早期的时候，硬件资源是比较缺乏的。为了提高性能，开发编译器的大师们，都会对编译器(从c到汇编的编译过程)加上一定的优化策略。优化后的代码效率比较高，但是可读性比较差，且编译时间更长。优化是指编译器一级的措施，与机器指令比较接近，所以很可能会导致硬件不兼容，进而产生了目前遇到的软件装不上的问题。 他是编译器的行为，与代码理论上不相关。）    

把Enable bitcode 修改为YES;   
(bit code是什么：说的是bitcode是被编译程序的一种中间形式的代码。包含bitcode配置的程序将会在App store上被编译和链接。bitcode允许苹果在后期重新优化程序的二进制文件，而不需要重新提交一个新的版本到App store上。另外的解释：当提交程序到App store上时，Xcode会将程序编译为一个中间表现形式(bitcode)。然后App store会再将这个botcode编译为可执行的64位或32位程序。总之就是和瘦身有关。)    

Strip Debug Symbols During Copy设置为YES;    
新创建版本会默认YES。旧版本未知。作用还需研究下。    

Strip Linked Product设置成YES；   
作用是去掉调试的信息。（目前还要确认会否影响到错误日志的收集）   

Symbols hidden by default 选项设置为YES。   
作用:release时去除不必要的调试符号。      
参考：[symbolicate工具环境设置](http://blog.csdn.net/dnj630/article/details/7321101)   

Make Strings Read-Only设为YES。    
字面意思就是变为只读属性。详细作用需要调研。   

附录： 
[配置参数的理解](http://blog.csdn.net/iitvip/article/details/9118499)

****************************

文件优化：

去掉文件未使用的第三方库和框架；   

去掉未使用的类。

去掉未使用的代码。       

****************************

代码优化：   

重构代码，提高代码的重用性。    

如果要用第三方库里面一个属性或者方法。建议自己重写。    

****************************

将工程 BuildSetting 的 Levels 选项中的 Generate Debug Symbols 这个设置为 NO:   
当 Generate Debug Symbols 设置为 YES 时，编译产生的 .o 文件会大一些，当然最终生成的可执行文件也大一些。
当 Generate Debug Symbols 设置为 NO 的时候，在 Xcode 中设置的断点不会中断，同样生成的ipa安装包也会小一些。

****************************
适当舍弃架构 armv7:   
因为 armv7 用于支持 4s 和 3gs，4s是2011年11月正式上线，虽然还有小部分人在使用，如果是是追求包体大小的完全可以舍弃了。

****************************
删除无用的图片音频和视频文件:   
ipa 包的体积增大很大程度上取决于资源文件的大小。包括 Images.xcassets 中无用的图片， bundle 中的音频、视频、图片 和字体文件等。

****************************
代码及代码文件的优化:   
通过 AppCode 打开对应的工程文件 选择 Code -> inspect Code 分析代码，去掉无用的引用及代码。查找内部使用到的第三方库，一方面可以进行删减代码，用不到的类，可以直接删除，还有把第三方库中的图片资源删除掉

****************************
Optimization Level 等编译项优化:   
Build Settings->Optimization Level有几个编译优化选项，release版应该选择 Fastest, Smalllest ，这个选项会开启那些不增加代码大小的全部优化，并让可执行文件尽可能小。
Strip Linked Product / Deployment Postprocessing / Symbols Hidden by Default在release版本应该设为yes，可以去除不必要的调试符号。Symbols Hidden by Default会把所有符号都定义成”private extern”。。

****************************
如何查看 ipa 包中的大文件?   
  1. 找到自己打包后的 ipa ，然后右键，打开方式选择归档实用工具，就会解压出来一个名为 Payload 文件夹。
  2. 在Payload文件夹中找到当前ipa的app文件（基本就是和这个ipa名字一样的文件，app后缀系统默认隐藏），右键显示包内容。
  3. 进入到文件夹内，按照大小进行排序，你会发现所有的资源。

****************************
查找 iOS 工程无用图片资源工具:   
[LSUnusedResources](https://github.com/tinymind/LSUnusedResources)
  1. 点击 Browse，选择一个文件夹。
  2. 点击 Search 开始搜索。
  3. 等待片刻即可看到结果，可直接对搜索结果进行操作。

****************************
注意:   
针对减小 ipa 包体积的操作，我们必须考虑相关影响，以确保做出正确的决定。如果不做权衡的话，我们无法知道需要对程序做出什么样的改变。

****************************
