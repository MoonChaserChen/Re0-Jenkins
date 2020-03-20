# yum安装后启动报错
## 问题
```
Starting jenkins (via systemctl):  Job for jenkins.service failed because the control process exited with error code. See "systemctl status jenkins.service" and "journalctl -xe" for details.
                                                           [失败]
```

## 原因分析
`journalctl -xe` 查看到如下信息
```
Starting Jenkins bash: /usr/bin/java: 没有那个文件或目录
```
已安装jdk，但并未在 `/usr/bin/java`

## 解决办法
```
yum install java-1.8.0-openjdk-devel
```