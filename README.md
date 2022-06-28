# 辅助控

声明：本系统为本人自娱自乐基于酷信开发的控制系统，不用于任何非法用途。下载用户，请勿用于非法获取，仅供大家娱乐。
1.测试环境使用的centos7.8 64位
2.在阿里云的端口管理，授权你自己的服务器ip或任何服务器都可以访问web服务器和im服务器所在的6379，9876端口。
3.在阿里云的云mongodb数据库申请开启外网访问（如果用的本地mongodb数据库，在端口开启28018端口）
4.修改imapi代码
使用文件【digit-1.0-SNAPSHOT.jar】替换maven下面的该文件【.m2/repository/com/dev/digit/1.0-SNAPSHOT/digit-1.0-SNAPSHOT.jar】
重新编译imapi的源码，打包出来【imapi-xx.war】文件
上传【imapi-xx.war】文件到web服务器的【/opt/spring-boot-imapi-jiqun】目录下替换掉原来的【imapi.war】
cd /opt/spring-boot-imapi-jiqun
vi application.properties 
指向控制系统的url地址，并保存(注意端口后面没有反斜线)
appConfig.randomKey=http://控制系统的外网ip:8080
再执行下面的命令停止和重启后台web系统
sh stop
sh start

5.在控服务器上安装jdk
使用FileZila上传文件【jdk1.8.0_131.zip】到控制服务器的/opt目录下面

使用ssh远程到服务器
ssh root@ip
输入密码进入系统
yum install zip unzip
cd  /opt
unzip jdk1.8.0_131.zip
mkdir  java
mv  jdk1.8.0_131  ./java


vi /etc/profile
将下面的内容输入到文件尾部
JAVA_HOME=/opt/java/jdk1.8.0_131
JRE_HOME=/opt/java/jdk1.8.0_131/jre
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
PATH=$JAVA_HOME/bin:$PATH
export PATH JAVA_HOME CLASSPATH

#更新环境变量
source /etc/profile

#查看是否安装成功
java -version
6.在控服务器部署redhelper
上传redhelper.zip 到/opt目录下
unzip redhelper.zip 
 cd redhelper

#编辑配置文件
 vi application.properties
 #端口可以自己随意改，改了要确保阿里云的该端口有开放
 server.port=8080
 #如果本地数据库，使用这个配置
 mongoconfig.uri=mongodb://聊天服务器的外网ip:28018
 如果是用的云数据库，配置类似这样：
 mongodb://root:数据库密码@主服务器,副服务器/admin?replicaSet=xxxx
 #redis也配置为对应外网服务器的ip
 redisson.address=redis://redis所在服务器的外网ip:6379
 #消息服务器和消息队列服务器配置对应外网ip
 im.mqConfig.nameAddr=127.0.0.1:9876
rocketmq.name-server=127.0.0.1:9876
保存配置文件

7.启动运行
sh start

登录管理后台进行配置
http://ip:8080/console/login
账号、密码跟web服务器的账号密码相同

8.在【卡密生成器-平台方V3.0】生成注册码的时候，在【其他（选填）】填写控服务器的访问地址，比如【http://控ip:8080】（端口后面不要带反斜线），生成的注册码发给群主使用即可。

ps：
1.本系统是通过修改原聊天系统的内存值，从而控制红包产生的数字。如果控制系统没有正常启动，web服务器也能正常发红包、抢红包，不影响。
2.【可选】通过配置redhelper下面的application.properties配置文件，修改端口【server.port=8080】，红包类型【appConfig.redType = 0】控制用户列表是否可以操控中不中。建议把系统部署2套，1套部署只能设置不中的，1套部署既可以中和不中的。修改上面的端口和红包类型即可。
3. 流水统计更新接口（通过配置application.properties中的allowUpdateWater项目设置是否允许匿名更新流水）
http://ip:8080/reddata/userRunningWater2?lostwin=100&steal=100&runwater=100&win=100&roomJid=xx
 @ApiImplicitParam(paramType = "query", name = "lostwin", value = "输赢", dataType = "String", required = true),
            @ApiImplicitParam(paramType = "query", name = "steal", value = "偷包", dataType = "String", required = true),
            @ApiImplicitParam(paramType = "query", name = "runwater", value = "流水", dataType = "String", required = true),
            @ApiImplicitParam(paramType = "query", name = "win", value = "中奖", dataType = "String", required = true) ,
            @ApiImplicitParam(paramType = "query", name = "roomJid", value = "群jid", dataType = "String", required = true)
            
            
  9.
  skype:jackmingli2029@outlook.com          
  添加时备注项目名称
  
