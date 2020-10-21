

# Oracle 11g 卸载

参考博客：

https://blog.csdn.net/Devin_LiuYM/article/details/59539020?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.add_param_isCf&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.add_param_isCf

## 步骤1：停服务

<img src="./image/1停服务.png" align='left'/>





## 步骤2： 删除注册表

1. 打开注册表：regedit 打开路径： <找注册表 ：开始->运行->regedit>

 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\ 

 删除该路径下的所有以oracle开始的服务名称，这个键是标识Oracle在windows下注册的各种服务。 

1. <img src="./image/2删除注册表.png" align='left'/>





2. 打开注册表，找到路径： 

 HKEY_LOCAL_MACHINE\SOFTWARE\ORACLE

 删除该oracle目录，该目录下注册着Oracle数据库的软件安装信息。

<img src="./image/3删除注册表.png" align='left'/>





3.删除注册的oracle事件日志，打开注册表

 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Eventlog\Application

 删除注册表的以oracle开头的所有项目。

<img src="./image/4删除注册表.png" align='left'/>





## 步骤3：删除系统环境变量的Oracel路径

删除系统环境变量Path: 中的Oracle内容

删除前的内容

E:\me2\oracle\app\lifehrx\product\11.1.0\db_1\bin;C:\Program Files (x86)\Common Files\Oracle\Java\javapath;C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;E:\me2\SVN\bin;E:\me2\TortoiseSVN\bin;E:\me2\node\;E:\me2\maven\apache-maven-3.6.1\bin;E:\me2\gradle\gradle-5.4.1-all\gradle-5.4.1\bin;C:\Program Files (x86)\NVIDIA Corporation\PhysX\Common;E:\me2\Git\Git\cmd



删除后的内容

C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;E:\me2\SVN\bin;E:\me2\TortoiseSVN\bin;E:\me2\node\;E:\me2\maven\apache-maven-3.6.1\bin;E:\me2\gradle\gradle-5.4.1-all\gradle-5.4.1\bin;C:\Program Files (x86)\NVIDIA Corporation\PhysX\Common;E:\me2\Git\Git\cmd

 

# Oracle 12C 安装

## 步骤1：双击setup

<img src="./image/1双击setup.png" align='left'/>



## 步骤2：顺序安装

<img src="./image/6oracle安装.png" align='left'/>



<img src="./image/7oracle安装.png" align='left'/>



<img src="./image/8oracle安装.png" align='left'/>



<img src="./image/9oracle安装.png" align='left'/>



<img src="./image/10oracel安装.png" align='left'/>



<img src="./image/11oracle安装.png" align='left'/>



<img src="./image/12oracle安装.png" align='left'/>



<img src="./image/13oracle安装.png" align='left'/>



<img src="./image/14oracle安装.png" align='left'/>



<img src="./image/15oracle安装.png" align='left'/>



<img src="./image/16oracle安装.png" align='left'/>