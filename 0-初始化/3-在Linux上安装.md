# 在RedHat安装
## 安装
```
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
yum install jenkins
```

### 国内源
上面的源在国外，国内安装很慢，可以使用[国内源](https://mirrors.tuna.tsinghua.edu.cn/jenkins/)：
```
wget https://mirrors.tuna.tsinghua.edu.cn/jenkins/redhat-stable/jenkins-2.204.5-1.1.noarch.rpm
rpm -ivh jenkins-2.204.5-1.1.noarch.rpm
```

## 启动
```
service jenkins start/stop/restart
```

## 开机启动
```
chkconfig jenkins on
```

## 修改端口
```
vim /etc/sysconfig/jenkins
JENKINS_PORT="8080"
```

## 修改context-path
```
vim /etc/sysconfig/jenkins
JENKINS_ARGS="--prefix=/jenkins"
```