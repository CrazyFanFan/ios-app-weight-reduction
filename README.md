# weight-reduction-ios

# iOS .ipa 文件大小优化

![](http://www.zoomfeng.com/images/2016/10/12/4.png)

****************************
实际开发中，使得 .ipa 安装包文件增大的一大元凶就是工程中导入积累了越来越多无用的图片，使得导出来的安装包大小剧增。    

解决办法：    </br>          
  在准备打包时，先用工具检测下工程中是否存在未使用的图片资源。 
      
工具使用教程和下载地址：    
[http://blog.lessfun.com/blog/2015/09/02/find-unused-resources-in-xcode-project/](http://blog.lessfun.com/blog/2015/09/02/find-unused-resources-in-xcode-project/)   

写了个简易的demo测试了下，总结优缺点如下：      
####优点：   

* 1.安装方便，下载后直接打开即可，无需多余步骤。   
* 2.使用方便，直接点击```browse```打开工程所在根目录，然后点击search即可获取项目中未使用的图片。   
* 3.匹配过滤更精确，例如：icon_tag_x.png 是不应该被当做未使用的资源的，只是以一种比较隐晦的方式间接引用了，所以不应该出现在结果列表中。    
* 4.删除方便，在获取到未使用图片列表后，可直接在工具上选中要删除图片即可，无需回到工程中逐个删除。   
* 5.查找速度开，一般几秒之内即可完成。

####缺点：   

* 1.若代码中使用了该图片，但是该代码被注释掉，或者没有被调用，工具无法检测出来。   
  例如：   
  
  ```
  // UIImage *image = [UIImage imageNamed:@"77.png"];
  
   /*
        UIImage *image = [UIImage imageNamed:[NSString stringWithFormat:@"aa_%d.png",1]];
    */
     
   ```
####总结：   
  经过简单demo测试后，发现这个工具还是非常好用的，方便快捷，强烈推荐使用。
  
  
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
