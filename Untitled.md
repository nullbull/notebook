1. docker pull centos获取centos镜像

2. 后台运行centos镜像并开放8080端口，window端可通过访问80端口访问到8080端口

```bash
docker run -itd -p 80:8080 docker.io/centos
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



3. 从宿主机拷贝文件到容器

​        拷贝方式为：

​        docker cp 宿主机中要拷贝的文件名及其路径 容器名：要拷贝到容器里面对应的路径

​        例如，将宿主机中路径：

​        /opt/test/下的文件：jdk

​        拷贝到容器：

​        mycontainer的：/niu

​        路径下，同样还是在宿主机中执行命令如下：

```
docker cp /opt/doc mycontainer:/niu
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

4. 解压jdk

```
tar -xzvf jdk1.8.tar.gz
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

5. 安装vim

```
yum -y install vim
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

6. 设置环境变量 

```
        vim ~/.bashrc

        #set oracle jdk environment

        export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_121  ## 这里要注意目录要换成自己解压的jdk 目录

        export JRE_HOME=${JAVA_HOME}/jre  

        export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  

        export PATH=${JAVA_HOME}/bin:$PATH
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

7. 使环境变量生效

```
source ~/.bashrc
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

8. 验证jdk

```
java -version
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

9. 保存镜像

```
[root@localhost ~]# docker commit -m "tomcat & jdk" zwt
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

10. 开启tomcat，访问192.168.113.136:80如下便运行成功

```
/niu/tomcat/bin/startup.sh
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



![img](https://upload-images.jianshu.io/upload_images/13321385-0d5c54fa5ac7df93.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

11. 查看正在运行的docker

```
docker ps 
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://upload-images.jianshu.io/upload_images/13321385-cf7a2cbefa58a040.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

12. 安装docker pull mysql

```
docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d docker.io/mysql:5.7
命令解析：--name：容器名
         -p 端口设置 前面的是linux的端口，后面的是docker的端口
         -e 设置数据库密码
         -d 后台运行
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



13. 必须修改%的root密码，要不然非localhost无法连接该数据库

```
    ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'newpassword'
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

14. 查看mysql镜像的ip docker inspect  + 容器id

同时修改jdbc.properties。

![img](https://upload-images.jianshu.io/upload_images/13321385-4ac5c5130bae7b49.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

15. 将war包复制到tomcat下面的webapps

```
docker cp /root/SuperMarket.war cb5453f085b1:/niu/tomcat/webapps
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

16. 如果一切没有问题。可以访问localhost:80/项目名称



![img](https://upload-images.jianshu.io/upload_images/13321385-3a280a265335916b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)