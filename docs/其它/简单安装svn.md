# 简单安装svn

1. yum安装软件
```
yum install subversion httpd mod_dav_svn
```

2. 创建repository
```
mkdir -p /data/svn/
cd /data/svn/
svnadmin create cka-repos
chown -R apache.apache cka-repos
```

3. 创建密码文件
```
cd /data/svn
htpasswd -cm passwdfile cka
```

4. 配置仓库密码权限
```
vim /etc/httpd/conf.d/subversion.conf
<Location /svn>
    DAV svn
    SVNParentPath /data/svn
    AuthType Basic
    AuthName "Authorization Realm"
    AuthBasicProvider file
    AuthUserFile /data/svn/passwdfile
    Require valid-user
</Location>
```

5. 配置httpd端口
```
vim /etc/httpd/conf/httpd.conf
Listen 6500
```

6. 启动httpd
```
htpd
```

7. 配置nginx
```
location ^~ /svn/ {
    client_max_body_size    100m;
    client_body_buffer_size 128k;
    proxy_connect_timeout 120;
    proxy_send_timeout 120;
    proxy_read_timeout 1200;
    proxy_set_header Host $host:80;
    proxy_set_header X-Forwarded-For $remote_addr;
    proxy_pass http://your.local.ip.address:6500;
}
```

8. 浏览器访问
```
https://your.domain/svn/cka-repos/
```

9. 参考
```
http://svnbook.red-bean.com/en/1.6/svn.serverconfig.httpd.html#svn.serverconfig.httpd.authz
```

10. 其它
[SVN推荐目录结构](https://versionsapp.com/documentation/about_svn_kc_trunktagbranch.html)