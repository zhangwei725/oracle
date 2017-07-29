## 一、 数据库安装

1. [下载地址](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/112010-win64soft-094461.html)

2. 需要下载两个文件，同时解压两个文件到同一个目录中。

   ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/51859779-file_1489061650112_17c69.png)

   ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/22932287-file_1489061680470_17cb5.png)

## 二、安装：

1. 双击”setup.exe”，软件会加载并初步校验系统是否可以达到了数据库安装的最低配置，检查监视器参数，如果达到要求，就会直接加载程序并进行下一步的安装；（根据电脑的配置不同，这个界面之后可能会卡一会儿）

   ![](http://ojx4zwltq.bkt.clouddn.com/17-3-7/14040680-file_1488889929447_15c0c.png)

2. 耐心等等该界面的出现。正在加载...

   ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/26450709-file_1489064319640_10324.png)

3. 如果有提示电脑配置不够也继续安装

   ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/56842167-file_1489064336059_1109c.png)

4. 配置安全更新：在出现的“配置安全更新”窗口中，取消“我希望通过MyOracleSupport接受安全更新”，单击“下一步”：注：电子邮件（可选）我希望通过MyOracleSupport接收安全更新（W）（可选）一般情况下，这两项不必要勾选。

   ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/13461401-file_1489064370891_d799.png)

5. 如果电子邮件没有输入：则提示直接点击—&gt;【是\(Y\)】

   ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/94213563-file_1489064611290_144e.png)

6. 我们现在需要创建和配置数据库，所以选择【创建和配置数据库】，继续下一步；

   ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/79334529-file_1489064671734_103ca.png)

   注：【创建和配置数据库】（C）安装数据库软件并创建一个数据库实例（初学者）

   ​ 【仅安装数据库软件】（D）安装数据库软件，不会创建数据库实例（非初学者）

   ​ 【升级现有的数据库】（U）升级低版本的Oracle数据库

7. 系统类：默认为“桌面类”；如果是安装在服务器上，选择“服务器类”，如WindowsServer系列，UbuntuServe等。服务器类可以设置高级选项。这里我们选择【服务器类】，下一步；

   ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/74043905-file_1489064863731_74c.png)

8. Real Application Clusters 数据库 是Oracle比较高级的话题了。简称 RAC，数据库集群。就是多台机器\(数据库\)都在工作。也叫分布式数据库管理。这里是Oracle管理部分，相对麻烦。我们现在是开发部分，所以我们选择 【单实例数据库安装】。然后继续下一步：

   ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/5789938-file_1489065033250_12119.png)

9. 为了能够方便进行我们的配置，所以安装的时候尽量选择【高级安装】；然后下一步

   ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/32631176-file_1489065424875_cfba.png)

10. 支持语言不用说了，默认简体中文、英语即可。然后下一步

    ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/66758725-file_1489065650665_9db7.png)

11. 版本选择【企业版本】，这里的东西功能是最全的。然后再点下一步。

    ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/57719596-file_1489065723697_1b1b.png)

12. 选择Oracle的基目录。软件位置会跟随基本目录自动分配，默认就好。然后下一步。

    ![](http://opzv089nq.bkt.clouddn.com/17-7-29/12008380.jpg)

13. 这里选择【一般用途/事务处理】，然后下一步（此处可能会卡顿一下）

    ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/31802428-file_1489066214819_15000.png)

14. 将数据库名称定义为 yztc。之前选择创建新的数据库，这个就是我们要创建的新数据库的名称。然后下一步

    ![](http://opzv089nq.bkt.clouddn.com/17-7-29/79376724.jpg)

15. 在【示例方案】页面把【创建具有示例方案的数据库】打钩。这个钩一定要打上，后面测试语句的时候需要用到，否则就无法练习测试语句。然后选择下一步。

    1. ![](http://opzv089nq.bkt.clouddn.com/17-7-29/14998165.jpg)

    2. 

16. 将【字符集】设置为 UTF-8编码。如果此处设置的不是UTF-8编码，则等到开发的时候就有可能出现中文乱码的问题。如果安装错了也没关系，大不了先不存中文先。 然后选择【示例方案】

    ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/68764636-file_1489066607411_2a93.png)

17. 默认即可，不用管它，直接下一步

18. ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/90784129-file_1489066974392_6373.png)

19. 默认即可，不用管它，直接下一步

    ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/81995712-file_1489067008814_14fba.png)

20. 这里也默认，不用管它，直接下一步

    ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/59212612-file_1489067054234_4f4c.png)

21. 这里注意！！！Oracle 对于管理员口令有建议的标准，如果密码不满足标准它会做相应的提示。

    ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/21766130-file_1489067298305_1d35.png)

    虽然点击确定也可以继续使用不标准的口令。为了规范，也为了方便管理，我建议完美所有的用户名的密码都统一设置为【Oracle11gadmin】。然后继续下一步

    ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/60859601-file_1489067620924_cc05.png)

22. 注意！！！在此之前会先执行条件检查，如果有出现检查出现错误提示，则直接忽略即可。然后直接点击完成开始安装Oracle产品。

    ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/28998267-file_1489067669579_ad6.png)

23. 正常安装流程

    ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/80568958-file_1489067817308_92f3.png)

    ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/96610650-file_1489067837433_16a74.png)

    ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/27350151-file_1489067920776_4619.png)

    ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/25780898-file_1489067935924_3206.png)

    此过程会比较慢，快则10分钟左右，慢则30分钟左右。请耐心等待。

    ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/718077-file_1489067956024_122e7.png)

    ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/12427805-file_1489068061426_15fcb.png)

    ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/78660205-file_1489068087296_25.png)

24. 安装完成，会生成以下界面。

    ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/34093966-file_1489069198147_14c0d.png)

    本次安装是自动实现数据库的创建，每个数据库都需要进行一些额外的配置。这里选择【口令管理】。

    ​

随后进入口令管理程序，主要操作\(设置\)以下几个用户：

* 超级管理员：sys ，密码设置为：change\_on\_install；

* 普通管理员：system ，密码设置为：manager；

  * 普通用户：scott ，密码设置为：tiger；\(scott创始人有一只猫，猫名字叫tiger\)（需要解锁）

  * 大数据用户\(用本数据库才有\)：sh ，密码设置为：sh；（需要解锁）

  **注意！！！以上设置都是普遍习惯性的密码设置，而实际生产环境我们的密码肯定不是这些习惯密码。为了方便以后的学习，也以防忘记密码，我们统一用这些习惯性密码来设置。**

  ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/61238026-file_1489069783969_525b.png)

  设置完成后直接点【确定】

  这里会提示密码设置的不标准，是否继续？选择【是】

  ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/15522943-file_1489069915462_e438.png)

* 最后数据库配置完成后，再点击【确定】

  ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/99056283-file_1489069998545_5e69.png)

* 于是，到这里 Oracle 数据库软件已经安装完成，我们的数据也创建配置完成！直接点击【关闭】，完成数据库产品的安装！！！

  ![](http://ojx4zwltq.bkt.clouddn.com/17-3-9/46550508-file_1489070082972_10339.png)

  ​

* 那么此时数据库可以说是安装完成了。但是，安装完成之后，Oracle 软件系统会在Windows系统服务中自动启动一些服务。

​

​

1. ​

2. ​



