# jenkins执行启动脚本未生效
Jenkins会在构建完成后使用processTreeKiller杀掉了所有子进程。

## 解决办法
1. freestyle的Item添加环境变量：BUILD_ID=dontKillMe
2. pipeline的Item添加环境变量：JENKINS_NODE_COOKIE=dontKillMe

### Demo
```
pipeline {
    agent any

    environment {
        PLAN_SERVER = "/usr/local/server/plan/"
        ARCH_NAME = "plan-1.0-SNAPSHOT.jar"
    }

    stages {
        stage('Clean') {
            steps {
                sh 'rm -rf plan'
            }
        }
        stage('Clone') {
            steps {
                sh 'git clone git@github.com:MoonChaserChen/plan.git'
            }
        }
        stage('PKG') {
            steps {
                sh 'mvn clean package -DskipTests -f plan/pom.xml'
            }
        }
        stage('START') {
            environment {
                JENKINS_NODE_COOKIE = "dontKillMe"
            }
            steps {
                sh 'mv plan/target/${ARCH_NAME} ${PLAN_SERVER}${ARCH_NAME}'
                sh '${PLAN_SERVER}start.sh'
            }
        }
    }
}
```
- start.sh中启动SpringBoot项目
```
#!/bin/bash
cd /usr/local/server/plan/
pid=`ps -ef | grep plan-1.0-SNAPSHOT.jar | grep -v grep | awk '{print $2}'`
if [ -n "$pid" ]
then
{
    echo ========kill start: $pid ==============
    kill $pid
    echo ========kill end==============
    sleep 1
}
fi
echo ===========startup==============
nohup java -jar -Dspring.profiles.active=prod plan-1.0-SNAPSHOT.jar &
```

## 参考文档
1. [官方文档](https://wiki.jenkins.io/display/JENKINS/ProcessTreeKiller)
3. [issues](https://issues.jenkins-ci.org/browse/JENKINS-46481)