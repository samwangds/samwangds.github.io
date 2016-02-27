title: android-相关环境变量配制笔记
---
>* 在第一步建一个系统变量方便目录发生改变时，直接改相应的系统变量，不用都去改PATH，以免出错
* 在用户变量PATH里追加路径时，如果之前不是以';'结尾的，记得先加上';' 再加路径。
#jdk
1.新建一个系统变量 
   JAVA_HOME - JDK目录
2.在系统变量里面的PATH 后追加 %JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;

#sdk (adb)
1.新建一个系统变量 
   android - sdk目录\platform-tools;sdk目录\tools
2.在系统变量里面的PATH 后追加 ;%android% 


#gradle
1.新建一个系统变量 
   gradle- android studio目录\gradle\gradle-x.x\bin
2.在系统变量里面的PATH 后追加 ;%gradle% 
 
#git
1.新建一个系统变量 
   git- C:\Program Files (x86)\Git\bin （git目录\bin）
2.在系统变量里面的PATH 后追加 ;%git% 


未完待续~
