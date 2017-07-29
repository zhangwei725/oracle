### 4.1、安装\(见附件\)

### 4.2、 卸载

1. 关闭oracle所有的服务。可以在windows的服务管理器中关闭；

2. 打开注册表：regedit打开路径：HKEY\_LOCAL\_MACHINE\SYSTEM\CurrentControlSet\Services\删除该路径下的所有以oracle开始的服务名称，这个键是标识Oracle在windows下注册的各种服务！

3. 打开注册表，找到路径：HKEY\_LOCAL\_MACHINE\SOFTWARE\ORACLE删除该oracle目录，该目录下注册着Oracle数据库的软件安装信息。

4. 删除注册的oracle事件日志，打开注册表HKEY\_LOCAL\_MACHINE\SYSTEM\CurrentControlSet\Services\Eventlog\Application删除注册表的以oracle开头的所有项目。

5. 删除环境变量path中关于oracle的内容。

6. 鼠标右键右单击“我的电脑&gt;属性&gt;高级&gt;环境变量 PATH变量。删除Oracle在该值中的内容。

   1. 注意:path中记录着一堆操作系统的目录，在windows中各个目录之间使用分号（;）隔开的，删除时注意。

   2. 建议：删除PATH环境变量中关于Oracle的值时，将该值全部拷贝到文本器中，找到对应的Oracle的值，删除后，再拷贝修改的串，粘贴到PATH环境变量中，这样相对而言比较安全。

7. 重新启动操作系统。以上1~5个步骤操作完毕后，重新启动操作系统。

8. 重启操作系统后各种Oracle相关的进程都不会加载了。这时删除Oracle\_Home下的所有数据。（Oracle\_Home指Oracle程序的安装目录）

9. 删除C:\ProgramFiles下oracle目录。

10. 删除C:\ProgramData下oracle目录。\(该目录一般隐藏\)（该目录视Oracle安装所在路径而定）

11. 删除开始菜单下oracle项，如：C:\DocumentsandSettings\AllUsers\「开始」菜单\程序\Oracle-Ora10g。

12. 最后在 命令行输入 lusrmgr.msc 进入用户/组 管理界面。删掉Oracle创建的用户。

13. 大功告成！！！！

### 4.3 、服务介绍\(了解\)

1. OracleOraDB10\_home1TNSListener：Oracle监听服务，如果有客户端需要连接到数据库，此服务必须打开。

2. OracleServiceORCL：Oracle数据库的主服务，此服务的必须启动才能使用Oracle

3. OrcaleDBConsoleorcl：Oracle数据库控制台，如果你需要用浏览器来使用oracle企业管理器，那么就启动这个服务。一般不需要开启。

4. OracleJobSchedulerORCL：Oracle job定时器的功能，一般不需要开启。

5. OracleOraDB10\_home1iSQLPlus：Oracle iSQLPlus服务，只有在Web页面中要使用iSQLPlus时候才需要启动。一般不需要开启。

  


