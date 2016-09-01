#### Android studio环境搭建中遇到的问题

  * 一、svn配置相关
    * 1、安装TortoiseSvn<br>File -> Other Settings -> Default Settings -> Version Control -> Subversion -> Use command line client:[path\svn.ext]<br>安装svn的时候要注意勾选安装命令行，否则安装目录中不会出现svn.exe
    * 2、Settings -> Version Control 选中项目的条目将VCS选到Subversion即可
    * 3、VCS -> Import Into Version Control -> Import Into Subversion -> 选择“+”添加svn路径 -> OK -> 选择本地版本路径 -> OK
  
  * 二、Android Studio Gradle Configuration with name 'default' not found
    * 1、第一次遇到，是因为所引用的dependence中没有build.gradle导致的
    * 2、第二次遇到是settings.gradle中没有include该项目

   * 三 UNEXPECTED TOP-LEVEL ERROR: finished with none-zero exit value 1/2/3
   value 1:被编译的代码或资源有问题
   value 2:jar包冲突
   value 3:编译的代码过多导致内存不足
   UNEXPECTED TOP-LEVEL ERROR: finished with none-zero exit value 3
   android {
    // ...
     dexOptions{
     incremental true
     javaMaxHeapSize "4g"
     }
    }
    或者修改android studio的安装目录下的bin目录中的studio.vmoptions文件中的Xms和Xmx两项，将其值改大，如下所示：
    -Xms256m
    -Xmx1280m
