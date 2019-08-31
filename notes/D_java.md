修改JVM启动参数


用java命令查看。
用java -option进行修改参数。
还有tomcat，eclipse启动时通过配置文件加载的。

详细如下：
安装Java开发软件时，默认安装包含两个文件夹，一个JDK(Java开发工具箱)，一个JRE(Java运行环境，内含JVM)，其中JDK内另含一个JRE。如果只是运行Java程序，则JRE已足够；而JDK则只有开发人员才用到。
一、配置JVM内存

1.配置JVM内存的参数
-XmxJavaHeap最大值，默认值为物理内存的1/4，最佳设值应该视物理内存大小及计算机内其他内存开销而定；
-XmsJavaHeap初始值，Server端JVM最好将-Xms和-Xmx设为相同值，开发测试机JVM可以保留默认值；
2.如何配置JVM内存分配：

（1）当在命令提示符下启动并使用JVM时（只对当前运行的类生效）
java-Xmx128m-Xms64m-Xmn32m-Xss16mTest

（2）当在集成开发环境下（如eclipse）启动并使用JVM时：
第一种设置方法：
在eclipse根目录下打开eclipse.ini，默认内容为（这里设置的是运行当前开发工具的JVM内存分配）：
-vmargs-Xms40m-Xmx256m-vmargs表示以下为虚拟机设置参数，可修改其中的参数值，，另外，eclipse.ini内还可以设置非堆内存，如：-XX:PermSize=56m，-XX:MaxPermSize=128m。

此处设置的参数值可以通过以下配置在开发工具的状态栏显示：
在eclipse根目录下创建文件options，文件内容为：org.eclipse.ui/perf/showHeapStatus=true
修改eclipse根目录下的eclipse.ini文件，在开头处添加如下内容：
-debugoptions-vmjavaw.exe重新启动eclipse，就可以看到下方状态条多了JVM信息。

第二种设置方法：
打开eclipse－窗口－首选项－Java－已安装的JRE（对在当前开发环境中运行的java程序皆生效）
编辑当前使用的JRE，在缺省VM参数中输入：-Xmx128m-Xms64m-Xmn32m-Xss16m

第三种设置方法：
打开eclipse－运行－运行－Java应用程序（只对所设置的java类生效）
选定需设置内存分配的类－自变量，在VM自变量中输入：-Xmx128m-Xms64m-Xmn32m-Xss16m

注：如果在同一开发环境中同时进行了第二种和第三种设置，则第二种设置生效，第三种设置无效，如：
开发环境的设置为：-Xmx256m，而类Test的设置为：-Xmx128m-Xms64m，则运行Test时生效的设置为：
-Xmx256m-Xms64m

（3）当在服务器环境下（如Tomcat）启动并使用JVM时（对当前服务器环境下所以Java程序生效）：
a.设置环境变量：
变量名：CATALINA_OPTS
变量值：-Xmx128m-Xms64m-Xmn32m-Xss16m

b.打开Tomcat根目录下的bin文件夹，编辑catalina.bat，将其中的%CATALINA_OPTS%（共有四处）替换为：-Xmx128m-Xms64m-Xmn32m-Xss16m

二、查看配置JVM内存信息
Runtime.getRuntime().maxMemory();//最大可用内存，对应-Xmx
Runtime.getRuntime().freeMemory();//当前JVM空闲内存
Runtime.getRuntime().totalMemory();//当前JVM占用的内存总数，其值相当于当前JVM已使用的内存及freeMemory()的总和

关于maxMemory()，freeMemory()和totalMemory()：
maxMemory()为JVM的最大可用内存，可通过-Xmx设置，默认值为物理内存的1/4，设值不能高于计算机物理内存；

totalMemory()为当前JVM占用的内存总数，其值相当于当前JVM已使用的内存及freeMemory()的总和，会随着JVM使用内存的增加而增加；

freeMemory()为当前JVM空闲内存，因为JVM只有在需要内存时才占用物理内存使用，所以freeMemory()的值一般情况下都很小，而JVM实际可用内存并不等于freeMemory()，而应该等于maxMemory()-totalMemory()+freeMemory()。及其配置JVM内存分配。


java -Xmx256m -XX:MaxDirectMemorySize=200m DirectByteBufferTest
-XX:ReservedCodeCacheSize