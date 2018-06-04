# Gradle for Android
  读完本文我可以了解到：
* [Android应用的构建过程](#应用的构建过程)
* [注意点](#注意点)
* [Android早期的构建工具](#早期的构建工具)
* [Android构建神器：Gradle](#构建神器)
****
### 应用的构建过程
Android应用的构建过程十分复杂，如图所示：<br/>
![image](https://github.com/MrRobotter/GradleForAndroid/raw/develop/images/Android应用构建过程示意图.png "Android应用构建过程示意图") <br/>
主要有以下几个步骤：
1. 主要的资源文件（layout，values等）都被 aapt 编译，并且在一个R文件中引用
2. Java代码被Java编译器编译成JVM字节码(.class文件)
3. JVM字节码再被dex工具转换成dalvik字节码（.dex文件）
4. 然后这些.dex文件、编译过的资源文件和其他资源文件（比如图片）会被打包成一个apk
5. apk文件在安装前会被debug/release的key文件签名
6. 安装到设备
****
### 注意点
1. 上面步骤中第一步是主要的资源文件，有些特别的资源文件就不会被编译，比如assets目录下的文件，raw目录下的文件以及图片，都不会被编译。只不过raw下的文件会在R文件里生成id
2. 如果对apk正式签名，还需要使用zipalign工具对apk进行对齐操作，这样做的好处是当应用运行时会减少内存的开销 <br/>
通过上面的了解，Android的构建过程十分复杂，如果每一步都要人工手动去完成的话，是费时费力的，效率太低，于是有了各种各样的构建工具。
****
### 早期的构建工具
在Gradle之前被广泛使用的是Apache Ant和 Maven。
#### Ant
Ant 发布于2000年，很快成为Java项目最流行的构建工具。<br/>
![image](https://github.com/MrRobotter/GradleForAndroid/raw/develop/images/Ant.png "Ant") <br/>
* Ant的优点：
  * 简单、易学，不需要什么特殊的准备就能上手
  * 基于过程式编程思想使得构建非常灵活
  * 后来还能支持插件。<br/>
* 不足之处:
  * 使用XML作为脚本配置格式，除非是很小的项目，否则它的XML文件就很快大得无法管理。
#### Maven
Maven 发布于2004年，目前是解决使用Ant所带来的一些问题。<br/>
![image](https://github.com/MrRobotter/GradleForAndroid/raw/develop/images/Maven.jpeg "Maven") <br/>
Maven也是使用XML作为构建配置的文件格式，不过文件结构却有了巨大的变化：
* Ant 需要开发者将执行task所需的全部命令都列出来
* Maven 依靠约定并提供现成可调用的目标<br/>

不仅如此，Maven更重要的一个进步是具备从网络上自动下载依赖的能力（后来Ant通过lvy也具备了这个功能），这革命性地改变了我们开发软件的方式。<br/>
Maven的缺点：
* 依赖管理不能很好地处理相同库文件不同版本之间的冲突（lvyz在这方面更好一些）
* XML作为配置文件的格式有严格的结构层次和标准，定制化目标很困难<br/>

Maven 主要解决了依赖管理的问题，然而使用XML的错误使他重蹈覆辙，实际上Maven很难写出复杂、定制化的构建脚本，在大型项目中，他经常什么“特别的事儿”还没干就有几百行代码，甚至不如Ant。<br/>

前辈们在反省前面两种构建工具的错误之后，提出了DSL（Domain Special Language，领域专用语言）的概念，目标是设计一套能够解决特定领域问题的语言。在构建方面，DSL的一个成功案例就是Gradle。
****
### 构建神器
![image](https://github.com/MrRobotter/GradleForAndroid/raw/develop/images/Gradle.gif "Gradle")

2012发布的Gradle深刻吸取前面几位前辈的经验和教训，集大家之所成，做了很多的改进。

Gradle有约定优于配置的原则，即为设置和属性提供默认值，这使得他比Ant或者Maven更容易上手。

Gradle构建脚本不再使用XML，而是使用Groovy的DSL进行书写（Groovy是一种基于Java虚拟机的动态语言，因此Android开发者看着会很熟悉），从而使得构建脚本比用Ant和Maven写的更清晰、简洁，下面是实现同样目标的Maven代码和Gradle代码对比:

![image](https://github.com/MrRobotter/GradleForAndroid/raw/develop/images/代码比较.gif "代码比较")

Gradle 既有Ant的强大和灵活性，又有Maven的依赖管理，支持插件且易于使用。因此一推出就获得广泛关注，2013年被Google选为Android Studio的默认构建工具。

Gradle设计的方式使他可以很容易地扩展构建和插入到现有的构建过程中，Google推广Gradle金额Android Studio时，目标是让代码复用、构建variant、配置和定制构建过程变得更加简单。

有人可能担心是否需要专门去学习新语言Groovy，《Gradle for Android》书中专门提到：

 >Groovy代码非常易读，如果你学习过Java，那么学习Groovy的曲线不会陡峭
  如果你想构建自己的任务和插件，那么对Groovy有更深次的理解尤为重要
  不过因为Groovy是基于Java虚拟机的，所以你完全可以用Java或者其他基于Java虚拟机的语言来编写你的自定义插件。

