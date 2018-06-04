# Gradle for Android
  读完本文我可以了解到：
* [Android应用的构建过程](#Android应用的构建过程)
* [注意点](#注意点)

****
### Android应用的构建过程
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
2. 如果对apk正式签名，还需要使用zipalign工具对apk进行对齐操作，这样做的好处是当应用运行时会减少内存的开销
