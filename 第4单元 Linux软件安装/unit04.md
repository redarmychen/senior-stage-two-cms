# 第四单元 Linux软件安装

# 【授课重点】

1. 了解Linux应用程序的组成部分
2. 掌握应用程序安装的方法
3. 在Linux中安装JDK
4. 在Linux中安装mysql
5. 在Linux中安装tomcat

# 【考核要求】

1. 掌握应用程序安装方法（tar、rpm、yum）
2. 掌握mysql安装
3. 掌握tomcat安装
4. 掌握本地WEB项目发布到Linux中

# 【教学内容】

## 1.1 课程导入

以后我们开发的项目都需要部署到Linux系统下，都是基于java的，那么就必须安装JDK等相关的软件，配置相关的环境。我们一起来学习下，如何在Linux系统中安装软件。

## 1.2 在Linux中安装JDK

在linux 下安装JDK，可正常使用java和javac等命令

#### **1.2.1 准备工作**

将资料文件夹中的软件目录下的jdk文件上传到Linux系统的用户主目录ee

![1570847194047](unit04.assets/1570847194047.png)   

#### **1.2.2 相关知识点：rpm命令**

l 格式：rpm [参数] [软件]

​		-v 　显示指令执行过程。

​		-h或--hash 　套件安装时列出标记。

​		-q 　使用询问模式，当遇到任何问题时，rpm指令会先询问用户。 

​		-a 　查询所有套件。

​		-i<套件档>或--install<套件档> 　安装指定的套件档。

​		-U<套件档>或--upgrade<套件档>   升级指定的套件档。 

​		-e<套件档>或--erase<套件档> 　删除指定的套件。

​		--nodeps 　不验证套件档的相互关联性

​		常用

​			安装：rpm -ivh rpm文件【安装】 

​			升级：rpm -Uvh rpm文件【更新】

​			删除：rpm -e --nodeps 软件名  【卸载软件命令】

​			查看：rpm –qa(配置管道使用)

#### **1.2.3 JDK安装步骤**

l 第一步：查看现有安装的JDK版本

rpm -qa | grep -i java 

![1570847241661](unit04.assets/1570847241661.png) 

openJDK它是Linux系统安装时自带的JDK，我们是不需要的，直接卸载即可！

 

l 第二步：卸载已有的软件

rpm -e --nodeps java-1.7.0-openjdk-1.7.0.79-2.5.5.4.el6.x86_64

rpm -e --nodeps java-1.6.0-openjdk-1.6.0.35-1.13.7.1.el6_6.x86_64



![1570847270331](unit04.assets/1570847270331.png) 

l 第三步：将之前上传好的JDK解压

需要先创建目录 mkdir /usr/local/src/java

tar –zxvf jdk1.8.0_65.tar.gz

 ![1570847291740](unit04.assets/1570847291740.png)

l 第四步：将加压好的JDK移动到java目录中

mv jdk1.8.0_65 /usr/local/src/java

![1570847308445](unit04.assets/1570847308445.png) 



l 第五步：配置系统环境变量

vim /etc/profile   #进入根目录

在这个配置文件的末尾(先i，可以移动光标，通过上下左右箭头)，添加如下2行代码(建议复制)

export JAVA_HOME=/usr/local/src/java/jdk1.8.0_65

export PATH=$JAVA_HOME/bin:$PATH

然后使用Esc  :wq保存退出

![1570847328853](unit04.assets/1570847328853.png) 

l 第六步：使配置文件生效并测试

先进入JDK的bin目录，然后复制以下2行代码即可：

source /etc/profile

java –version  #测试是否安装成功



![1570847599027](unit04.assets/1570847599027.png) 

如果配置成功，只需要在bin目录下面运行 java –version,在任意目录均可！(上图是bin目录，下图是根目录)

![1570847615113](unit04.assets/1570847615113.png) 

## 1.3 在Linux中安装mysql

l 第一步：查看之前版本

rpm -qa | grep -i mysql --color

![1570847641157](unit04.assets/1570847641157.png) 

l 第二步：卸载之前的版本

rpm -e --nodeps mysql-libs-5.1.73-5.el6_6.x86_64 

![1570847663143](unit04.assets/1570847663143.png) 

l 第三步：上传2个rpm的MySQL文件(5.5.49)

![1570847679243](unit04.assets/1570847679243.png) 

MySQL安装系统会自动指定目录。

l 第四步：安装server

rpm -ivh MySQL-server-5.6.25-1.el6.x86_64.rpm

![1570847740832](unit04.assets/1570847740832.png) 

l 第五步：安装client

rpm -ivh MySQL-client-5.6.25-1.el6.x86_64.rpm

![1570847757835](unit04.assets/1570847757835.png) 

l 第六步：查询MySQL服务运行状态

service mysql status

![1570847824571](unit04.assets/1570847824571.png) 

l 第七步：启动MySQL服务

service mysql start

![1570847940605](unit04.assets/1570847940605.png) 

l 第八步：使用root账户登录MySQL

[root@gao~]# cat /root/.mysql_secret

![1570847960031](unit04.assets/1570847960031.png)

使用此密码登录:

mysql -uroot -p

登录成功后

l 第九步：修改密码

SET PASSWORD = PASSWORD('root');

![1570847985285](unit04.assets/1570847985285.png) 

l 第十步：虚拟机内部登录MySQL

mysql -u root –p   密码为之前设置的root

![1570848007490](unit04.assets/1570848007490.png) 

l 第十一步：远程主机访问，设置防火墙

n 打开防火墙配置

vim /etc/sysconfig/iptables

n 设置内容

-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT

![1570848029750](unit04.assets/1570848029750.png) 

n 重启服务  

service iptables restart

![1570848046094](unit04.assets/1570848046094.png) 

n 使用mysql语句创建远程登录用户(**登录MySQL**)

create user 'root'@'%' identified by 'root';		#创建用户，并设置密码



grant all on *.* to 'root'@'%' with grant option;	#给指定的用户授权

flush privileges;									#刷新权限

![1570848130702](unit04.assets/1570848130702.png) 

n 远程访问成功

mysql -h192.168.59.128 -uroot –proot

​	打开DOS窗口，输入上面的命令

![1570848158356](unit04.assets/1570848158356.png) 



## 1.4 在Linux中安装tomcat

l 第一步：上传Tomcat

rz 上传

![1570848174355](unit04.assets/1570848174355.png) 

l 第二步：解压Tomcat

tar -zxvf apache-tomcat-7.0.57.tar.gz



![1570848196434](unit04.assets/1570848196434.png) 

l 第三步：移动到指定目录java

mv apache-tomcat-7.0.57 /usr/local/src/java

![1570848214144](unit04.assets/1570848214144.png) 

l 第四步：配置防火墙(所有的配置都在etc目录下面,指定windows系统访问的端口号)

vim /etc/sysconfig/iptables

![1570848229141](unit04.assets/1570848229141.png) 

n 运行8080端口远程访问

-A INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT

![1570848246710](unit04.assets/1570848246710.png) 

n 重启启动防火墙服务

先进入tomcat的安装目录

![1570848262902](unit04.assets/1570848262902.png) 

service iptables restart     #重启防火墙命令

![1570848280308](unit04.assets/1570848280308.png) 

l 第五步：启动并访问Tomcat

**进入tomcat所在的bin目录**

cd /usr/local/src/java/apache-tomcat-7.0.57/bin

![1570848297516](unit04.assets/1570848297516.png) 

启动tomcat:  ./startup.sh

![1570848315365](unit04.assets/1570848315365.png) 



浏览器访问：

![1570848347719](unit04.assets/1570848347719.png) 

第六步：关闭Tomcat结束测试

![1570848364484](unit04.assets/1570848364484.png) 

依旧需要先进入tomcat的bin目录，然后执行 ./shutdown.sh命令

![1570848383868](unit04.assets/1570848383868.png) 