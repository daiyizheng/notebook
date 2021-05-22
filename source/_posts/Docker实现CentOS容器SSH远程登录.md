---
title: Docker实现CentOS容器SSH远程登录
date: 2021-05-19 13:03:00
tags:
---

这里根据Dockerfile方式构建一个CentOS的可远程SSH的镜像。

## Dockerfile文件
在/data/test/sshd_centos/目录下新建Dockerfile文件。注意：目录可以自行设定，但目录下除了Dockerfile文件外建议不要放置别的文件和目录。

`vim Dockerfile`

```shell
# 生成的新镜像以centos镜像为基础
FROM centos
# 指定作者信息
MAINTAINER by dyz
# 安装openssh-server
RUN yum -y install openssh-server

RUN mkdir /var/run/sshd
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
RUN ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key

# 指定root密码
RUN /bin/echo 'root:dyz123456'|chpasswd
RUN /bin/sed -i 's/.*session.*required.*pam_loginuid.so.*/session optional pam_loginuid.so/g' /etc/pam.d/sshd
RUN /bin/echo -e "LANG=\"en_US.UTF-8\"" > /etc/default/local
EXPOSE 22
CMD /usr/sbin/sshd -D
```



## build镜像
在Dockerfile当前目录执行下面语句，开始构建镜像。注意最后面的点不要忘了，表明是读取当前目录的Dockerfile文件。

`docker build -t dyz/centos-ssh:v1.0.0 .`
bigdata/centos-ssh:v1.0.0：新生成的镜像名称及版本号

打包成功的话会出现下面的提示，可能时间会有点长，耐心等待。

Successfully built 2d548392b205
## 查看镜像
`docker images`

## 启动容器
`docker run -itd -p 11122:22 --name centos dyz/centos-ssh:v1.0.0`

后台启动一个容器，将该容器名称设置为：test_centos_1，将容器端口22映射到宿主机端口10022。

## 远程访问
远程通过 宿主机IP、映射端口10022进行访问容器。
