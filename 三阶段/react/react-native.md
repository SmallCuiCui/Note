## 配置

* 首先安装JDK，添加系统环境变量：安装路径下的bin，命令行输入javac正常表示成功。

* 安装android-studio-

  * 首次运行，设置代理，选择无代理，进行科学上网，，，翻墙---或者使用代理，Android的国内镜像

  * 将安装路径下面的/jre设置为新建用户变量的`JAVA_HOME`的值

  * 启动android-studio，找到file-settings .. -->Appearance& Behavior -->System Settings-->Android SDK --->Android SDK location的路径(..../Android/Sdk)，设置为`ANDROID_HOME`的值
  * 将上面一个步骤找到的路径下的platform-tools路径添加到系统环境变量，命令行输入adb正常表示成功

* 安装夜神模拟器

  安装之后命令行执行`adb connect 127.0.0.1:62001`