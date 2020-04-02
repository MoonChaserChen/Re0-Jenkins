# 无法识别mvn命令
## 在Window上遇到
### 简述
Windows上安装使用Jenkins，环境变量中Path已配置有：java,mvn路径，但Jenkins无法识别mvn命令
```
C:\Users\akira>mvn --version
Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f)
Maven home: D:\Program Files\maven\apache-maven-3.6.3\bin\..
Java version: 1.8.0_131, vendor: Oracle Corporation, runtime: D:\Program Files\Java\jre1.8.0_131
Default locale: zh_CN, platform encoding: GBK
OS name: "windows 10", version: "10.0", arch: "amd64", family: "windows"
```
	
### Pipeline：
```
pipeline {
   agent any

   stages {
        stage('Java') {
            steps {
                bat 'java -version'
            }
        }
        stage('MVN') {
            steps {
                bat 'mvn --version'
            }
        }
    }
}
```

### 结果：
```
D:\Program Files (x86)\Jenkins\workspace\first_pipline>java -version 
java version "1.8.0_131"
Java(TM) SE Runtime Environment (build 1.8.0_131-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.131-b11, mixed mode)
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (MVN)
[Pipeline] bat

D:\Program Files (x86)\Jenkins\workspace\first_pipline>mvn --version 
'mvn' 不是内部或外部命令，也不是可运行的程序
或批处理文件。
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
```

### 原因：
Windows的环境变量配置有两种，用户变量与系统变量，Jenkins只会取系统变量，因此mvn的路径应配置到系统变量的Path中（配置后需要重启Jenkins）。


## 在Linux上遇到
同样在shell中可以执行 `mvn -v`命令，但在jenkins中无法识别mvn命令

### 可能原因
【Linux上好像只会识别： `/usr/bin/` 目录下的命令】

### 解决办法
创建软链接： `ln -s /usr/local/maven3/bin/mvn /usr/bin/mvn`
