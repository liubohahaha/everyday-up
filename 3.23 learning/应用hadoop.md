### 1.	安装hadoop系统  
#### (1)	创建hadoop用户
```
1.	$ sudo useradd -m hadoop -s /bin/bash     #创建hadoop用户，并使用       /bin/bash作为shell
2.	$ sudo passwd hadoop    #为hadoop用户设置密码，之后需要连续输入两次密码
3.	$ sudo adduser hadoop sudo             #为hadoop用户增加管理员权限
4.	$ su - hadoop                          #切换当前用户为用户hadoop
5.	$ sudo apt-get update           #更新hadoop用户的apt,方便后面的安装
```
![image](https://user-images.githubusercontent.com/49645739/112183231-0338a300-8c39-11eb-8b06-2818a2e25002.png)

#### (2)	安装SSH服务  
免密码SSH访问配置  
![image](https://user-images.githubusercontent.com/49645739/112183254-092e8400-8c39-11eb-9060-9d6b2232e218.png)
* i.	执行命令产生认证文件  
执行后连续三次回车，第一次回车是让KEY存于默认位置，以方便后续的命令输入，第二次和第三次是确定passphrase，相关性不大。将在/home/hadoop/.ssh目录下生成id_rsa认证文件。  
* ii.	将该文件复制到名为authorized_keys的文件  
![image](https://user-images.githubusercontent.com/49645739/112183277-0e8bce80-8c39-11eb-8f38-4edd5b21b5a6.png)  
* iii.	测试登录  
![image](https://user-images.githubusercontent.com/49645739/112183313-16e40980-8c39-11eb-9878-70fef94a0005.png)
此时登录不再需要输入密码，完成免密访问。  
#### (3)	解压安装hadoop  
* i.	官网下载hadoop-2.10.1   
* ii.	解压安装包并修改文件权限    
```
1.	sudo tar -zxvf  /home/liubo/Downloads/hadoop-2.10.1.tar.gz -C /usr/local                                #安装位置
2.	cd /usr/local/
3.	sudo mv ./hadoop-2.10.1/ ./hadoop          # 将文件夹名改为hadoop
4.	sudo chown -R hadoop ./hadoop           # 修改文件权限
```
#### (4)	安装jdk  
使用命令行安装jdk
![image](https://user-images.githubusercontent.com/49645739/112183369-26635280-8c39-11eb-9edc-02798769ba88.png)
#### (5)	配置环境变量
进入.brashrc文件，添加相关信息。  
* i.配置java环境变量  
查看jdk的安装路径  
![image](https://user-images.githubusercontent.com/49645739/112183480-40049a00-8c39-11eb-8f03-28a07aee764a.png)
因此JAVA_HOME应为 /usr/lib/jvm/java-8-openjdk-amd64/jre,  
添加如下信息：  
 ![image](https://user-images.githubusercontent.com/49645739/112183520-4bf05c00-8c39-11eb-86ab-e200a32f1346.png)
* ii.	配置hadoop环境变量  
![image](https://user-images.githubusercontent.com/49645739/112183539-51e63d00-8c39-11eb-8b6c-dcc308239f09.png)
    更新配置  
    ![image](https://user-images.githubusercontent.com/49645739/112183574-59a5e180-8c39-11eb-8aa3-c6bf0c4dcb8c.png)
    配置完成后查看hadoop是否安装成功  
    ![image](https://user-images.githubusercontent.com/49645739/112183595-5f032c00-8c39-11eb-8001-bebeccd6715c.png)
##### (6)	伪分布式配置  
* i.	配置core-site.xml
```
1.	<configuration>
2.	        <property>
3.	           <name>hadoop.tmp.dir</name>
4.	           <value>file:/usr/local/hadoop/tmp</value>
5.	           <description>Abase for other temporary directories.</description>
6.	        </property>
7.	        <property>
8.	           <name>fs.defaultFS</name>
9.	           <value>hdfs://localhost:9000</value>
10.	        </property>
11.	</configuration>
```
ii.	配置hadoop-env.sh  
  将jdk1.8的路径添加到hadoop-env.sh  
  ```
  1.	export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre
  ```
iii.	配置hdfs-site.xml  
```
1.	<configuration>
2.	        <property>
3.	             <name>dfs.replication</name>
4.	             <value>1</value>
5.	        </property>
6.	        <property>
7.	             <name>dfs.namenode.name.dir</name>
8.	             <value>file:/usr/local/hadoop/tmp/dfs/name</value>
9.	        </property>
10.	        <property>
11.	             <name>dfs.datanode.data.dir</name>
12.	             <value>file:/usr/local/hadoop/tmp/dfs/data</value>
13.	        </property>
14.	</configuration>
```
iv.	配置mapred-site.xml  
```
1.	<configuration>
2.	  <property>
3.	  <name>mapreduce.framework.name</name>
4.	  <value>yarn</value>
5.	  </property>
6.	</configuration>
```
v.	配置yarn-site.xml  
```
1.	<configuration>
2.	  <property>
3.	  <name>yarn.nodemanager.aux-services</name>
4.	  <value>mapreduce_shuffle</value>
5.	  </property>
6.	  <property>
7.	  <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
8.	  <value>org.apache.hadoop.mapred.ShuffleHandler</value>
9.	  </property>
10.	</configuration>
```
#### (7)	格式化Namenode   
![image](https://user-images.githubusercontent.com/49645739/112183756-84903580-8c39-11eb-9640-2711dcfb2f81.png)

成功格式化结果：
![image](https://user-images.githubusercontent.com/49645739/112183769-878b2600-8c39-11eb-8fd4-4805bc89b621.png)
#### (8)	启动HDFS和MapReduce 
![image](https://user-images.githubusercontent.com/49645739/112183789-8c4fda00-8c39-11eb-9485-be7a3f1455cd.png)

查看是否启动成功
![image](https://user-images.githubusercontent.com/49645739/112183825-9540ab80-8c39-11eb-997f-78f092702dd3.png)

web端查看HDFS文件系统，localhost:50070  
![image](https://user-images.githubusercontent.com/49645739/112183845-9a055f80-8c39-11eb-9412-6051edac9db2.png)

#### (9)	启动yarn集群  
使用脚本一键启动是启动集群最常使用的方式。  
 ![image](https://user-images.githubusercontent.com/49645739/112183858-9d005000-8c39-11eb-80ef-da72fc4c2f99.png)

查看是否启动成功：  
 ![image](https://user-images.githubusercontent.com/49645739/112183876-a12c6d80-8c39-11eb-938a-0fa62a94ced8.png)

web网页端查看，localhost:8088  
![image](https://user-images.githubusercontent.com/49645739/112183896-a5588b00-8c39-11eb-99b7-5f81bb498b7f.png)

#### (10)	yarn资源管理  
当运行wordcount程序时，在yarn中查看作业运行过程。  
作业运行时：  
![image](https://user-images.githubusercontent.com/49645739/112183907-a984a880-8c39-11eb-9bc9-48d46f7d7d07.png)

运行结束后:
![image](https://user-images.githubusercontent.com/49645739/112183930-ae495c80-8c39-11eb-9fa4-5fec7f756b59.png)

 

