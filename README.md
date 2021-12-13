## jenkins gitlab nexus3/harbor 此类清单已有，安装部署不再叙述

我这里ci/cd都在Jenkins同一个job内完成。如果ci/cd都在Jenkins中完成的话，正常的gitops应是ci/cd分为两个pipeline job完成，并通过gitlab webhook进行提交自动触发构建部署；或者ci在Jenkins中完成，cd通过argocd来完成纯自动化的devops。


我这里没有使用纯自动化的原因是，不太喜欢纯自动化。后续会把这个文档gitlab配置nexus3/harbor配置完善起来。同时也写一下上边我说的Jenkins分为ci/cd两个job纯自动化构建部署，很ci使用jenkins，cd采用argocd。


需准备：
用于提交开发代码的gitlab代码仓库（拉取代码 => 编译源码 => 通过dockerfile打包镜像推送至仓库）

用于存放yaml/helm资源清单配置文件（拉取配置清单 => sed修改镜像版本 => kubectl apply滚动更新或 rollback回滚）


Github: https://github.com/alexclownfish/jenkins_pipline
资源清单均在以上github地址
* 版本发布失败/成功推送钉钉
* 版本回滚成功/失败推送钉钉
* 构建stage过程成功/失败推送钉钉
## jenkins 所需插件
网络延迟大下载慢，可以到下边地址下载，再load到jenkins
https://updates.jenkins-ci.org/download/plugins/
```
Docker plugin
Kubernetes plugin
Kubernetes CLI Plugin
DingTalk
GitLab Plugin
Localization: Chinese (Simplified)
```
## kubernetes clouds 配置
### 添加kubernetes clouds
![在这里插入图片描述](https://img-blog.csdnimg.cn/b5d7bf8647884362986318f120ce3ab8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQWxleENsb3duZmlzaA==,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/01be65a17f824bff935707c4fd38212b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQWxleENsb3duZmlzaA==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/64d7f189fff840bb88c24626447ca882.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQWxleENsb3duZmlzaA==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/3d0852c42d224d27a06eefb4751aafbf.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQWxleENsb3duZmlzaA==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/a478fbb644584217a427d752a679fc46.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQWxleENsb3duZmlzaA==,size_20,color_FFFFFF,t_70,g_se,x_16)
### Pod Templates
![在这里插入图片描述](https://img-blog.csdnimg.cn/ae9beea44aec465280b87566cced3d7b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQWxleENsb3duZmlzaA==,size_20,color_FFFFFF,t_70,g_se,x_16)
#### jenkins-slave
![在这里插入图片描述](https://img-blog.csdnimg.cn/12eef9e0420e4fd896e7014197f683cb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQWxleENsb3duZmlzaA==,size_20,color_FFFFFF,t_70,g_se,x_16)
#### maven-3.6
![在这里插入图片描述](https://img-blog.csdnimg.cn/6de203aa14cf4403a235e3180d8952f8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQWxleENsb3duZmlzaA==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/c39c211b0fc247dcad37e6a1b979dee5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQWxleENsb3duZmlzaA==,size_20,color_FFFFFF,t_70,g_se,x_16)
#### docker-and-maven 继承maven-3.6，挂载docker.sock以便后续docker.build
![在这里插入图片描述](https://img-blog.csdnimg.cn/2001cd0343e24459bc47d3a998e26c8c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQWxleENsb3duZmlzaA==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/4ce10e598aab430fb53a65b7c6268413.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQWxleENsb3duZmlzaA==,size_20,color_FFFFFF,t_70,g_se,x_16)
#### kubectl (用于kubectl apply 滚动更新)
![在这里插入图片描述](https://img-blog.csdnimg.cn/ab1ceed539b74cf9999e1c8b78f542c3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQWxleENsb3duZmlzaA==,size_20,color_FFFFFF,t_70,g_se,x_16)
#### docker-and-maven-and-kubectl 继承docker-and-maven
![在这里插入图片描述](https://img-blog.csdnimg.cn/a85774d512ad4c2ab274755c236461cd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQWxleENsb3duZmlzaA==,size_20,color_FFFFFF,t_70,g_se,x_16)
## pipeline to dingding
![在这里插入图片描述](https://img-blog.csdnimg.cn/3ed14fa774d3469d8d05f6048bce0b04.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQWxleENsb3duZmlzaA==,size_20,color_FFFFFF,t_70,g_se,x_16)
## 凭据
![在这里插入图片描述](https://img-blog.csdnimg.cn/abbe4d4e457e441382df1a91b41e1e21.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQWxleENsb3duZmlzaA==,size_20,color_FFFFFF,t_70,g_se,x_16)
## Job 配置
### 参数化构建 （版本回滚）
![在这里插入图片描述](https://img-blog.csdnimg.cn/3b6ce4299f674e3fb702507921b8703f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQWxleENsb3duZmlzaA==,size_20,color_FFFFFF,t_70,g_se,x_16)
### pipeline as vscode
#### 将pipeline 以Jenkinsfile形式存放到代码仓库
![在这里插入图片描述](https://img-blog.csdnimg.cn/276993a1538c47c98f303b9bdfc89f2e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQWxleENsb3duZmlzaA==,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 配置Pipeline script from SCM （pipeline从代码仓库检出）
![在这里插入图片描述](https://img-blog.csdnimg.cn/2961be0afdba4656a5806f1b9beff2fe.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQWxleENsb3duZmlzaA==,size_20,color_FFFFFF,t_70,g_se,x_16)
## PIPELINE
```
pipeline {
    environment {
        appName = "spring-boot-helloword"
        appVersion = "v1.0.3"
        registryCredential = "nexus3-admin"
        registry = "http://sonatype-nexus.cicd.svc.cluster.local:8082"
        STATUS_URL = "http://xxx/job/spring-boot-helloword/${BUILD_NUMBER}/"
        CONSOLE_URL = "http://xxx/job/spring-boot-helloword/${BUILD_NUMBER}/console"
    }
    agent {
        kubernetes {
            inheritFrom 'docker-and-maven-and-kubectl'
        }
    }
    
    stages {
        stage ("PULL CODES FROM CANGKU") {
            when {
                environment name: 'action',value: 'deploy'
            }
            steps {
                git branch: 'master', url: 'http://gitlab.cicd.svc.cluster.local:31080/root/spring-boot-demo.git'
            }
            post {
                failure {
                    dingtalk (
                        robot: '1174f280-4f5a-43e7-aa2f-a062c3808993',
                        type: 'MARKDOWN',
                        title: 'spring-boot项目',
                        text: [
                            '### Jenkins_job_spring-boot',
                            '---',
                            '- 状态：<font color=#FF0000 >${appName}拉取代码失败</font>',
                            '- 版本：${BUILD_NUMBER}',
                            '- [查看部署详情](${STATUS_URL})',
                            '- [查看日志Console](${CONSOLE_URL})'
                        ],
                        at: [
                            '15737927244'
                        ]
                    )
                }
            }
        }
        stage ("BUILD JAR") {
            when {
                environment name: 'action',value: 'deploy'
            }
            steps {
                container ('maven') {
                    sh 'mvn clean test package'
                }
            }
            post {
                failure {
                    dingtalk (
                        robot: '1174f280-4f5a-43e7-aa2f-a062c3808993',
                        type: 'MARKDOWN',
                        title: 'spring-boot项目',
                        text: [
                            '### Jenkins_job_spring-boot',
                            '---',
                            '- 状态：<font color=#FF0000 >${appName}代码编译失败</font>',
                            '- 版本：${BUILD_NUMBER}',
                            '- [查看部署详情](${STATUS_URL})',
                            '- [查看日志Console](${CONSOLE_URL})'
                        ],
                        at: [
                            '15737927244'
                        ]
                    )
                }
            }
        }
        stage ("BUILDING APP IMAGE") {
            when {
                environment name: 'action',value: 'deploy'
            }
            steps {
                container ('docker') {
                    script {
                        dockerImage = docker.build appName + ":" + appVersion + "-" + BUILD_NUMBER
                    }
                }
            }
            post {
                failure {
                    dingtalk (
                        robot: '1174f280-4f5a-43e7-aa2f-a062c3808993',
                        type: 'MARKDOWN',
                        title: 'spring-boot项目',
                        text: [
                            '### Jenkins_job_spring-boot',
                            '---',
                            '- 状态：<font color=#FF0000 >${appName}镜像打包失败</font>',
                            '- 版本：${BUILD_NUMBER}',
                            '- [查看部署详情](${STATUS_URL})',
                            '- [查看日志Console](${CONSOLE_URL})'
                        ],
                        at: [
                            '15737927244'
                        ]
                    )
                }
            }
        }
        stage ("Push App Image") {
            when {
                environment name: 'action',value: 'deploy'
            }
            steps {
                container('docker') {
                    script {
                        docker.withRegistry(registry,registryCredential) {
                            dockerImage.push()
                        }
                    }
                }
            }
            post {
                failure {
                    dingtalk (
                        robot: '1174f280-4f5a-43e7-aa2f-a062c3808993',
                        type: 'MARKDOWN',
                        title: 'spring-boot项目',
                        text: [
                            '### Jenkins_job_spring-boot',
                            '---',
                            '- 状态：<font color=#FF0000 >${appName}推送镜像失败</font>',
                            '- 版本：${BUILD_NUMBER}',
                            '- [查看部署详情](${STATUS_URL})',
                            '- [查看日志Console](${CONSOLE_URL})'
                        ],
                        at: [
                            '15737927244'
                        ]
                    )
                }
            }
        }
        stage('Pull Source Config File') {
            steps {
                git branch: 'master',url:'http://gitlab.cicd.svc.cluster.local:31080/root/spring-boot-deploy.git'
            }
            post {
                failure {
                    dingtalk (
                        robot: '1174f280-4f5a-43e7-aa2f-a062c3808993',
                        type: 'MARKDOWN',
                        title: 'spring-boot项目',
                        text: [
                            '### Jenkins_job_spring-boot',
                            '---',
                            '- 状态：<font color=#FF0000 >${appName}配置清单拉取失败</font>',
                            '- 版本：${BUILD_NUMBER}',
                            '- [查看部署详情](${STATUS_URL})',
                            '- [查看日志Console](${CONSOLE_URL})'
                        ],
                        at: [
                            '15737927244'
                        ]
                    )
                }
            }
        }
        stage('Apply App Yaml') {
            when {
                environment name: 'action',value: 'deploy'
            }
            steps {
                container('kubectl') {
                    withKubeConfig([credentialsId: 'k8s-cluster-admin-kubeconfig-file']) {
                        sh '''
                        sed -ri "s@image: .*@image: sonatype-nexus.cicd.svc.cluster.local:8082/${appName}:${appVersion}-${BUILD_NUMBER}@g"  deploy/03-deployment.yaml
                        kubectl apply -f deploy/ --record
                        '''
                    }
                }
            }
            post {
                success {
                    dingtalk (
                        robot: '1174f280-4f5a-43e7-aa2f-a062c3808993',
                        type: 'ACTION_CARD',
                        title: 'spring-boot项目',
                        text: [
                            '### Jenkins_job_spring-boot',
                            '---',
                            '- 状态：<font color=#00CD00 >${appName}滚动更新成功</font>',
                            '- 版本：${BUILD_NUMBER}',
                            '- [查看部署详情](${STATUS_URL})',
                            '- [查看日志Console](${CONSOLE_URL})'
                        ],
                        at: [
                            '15737927244'
                        ]
                    )
                }
                failure {
                    dingtalk (
                        robot: '1174f280-4f5a-43e7-aa2f-a062c3808993',
                        type: 'MARKDOWN',
                        title: 'spring-boot项目',
                        text: [
                            '### Jenkins_job_spring-boot',
                            '---',
                            '- 状态：<font color=#FF0000 >${appName}滚动更新失败</font>',
                            '- 版本：${BUILD_NUMBER}',
                            '- [查看部署详情](${STATUS_URL})',
                            '- [查看日志Console](${CONSOLE_URL})'
                        ],
                        at: [
                            '15737927244'
                        ]
                    )
                }
            }
        }
        stage('Rollback') {
            when {
                environment name: 'action',value: 'rollback'
            }
            steps {
                container('kubectl') {
                    withKubeConfig([credentialsId: 'k8s-cluster-admin-kubeconfig-file']) {
                        sh '''
                        sed -ri "s@image: .*@image: sonatype-nexus.cicd.svc.cluster.local:8082/${appName}:${appVersion}-${BUILD_NUMBER}@g"  deploy/03-deployment.yaml
                        kubectl apply -f deploy/ --record
                        '''
                    }
                }
            }
            post {
                success {
                    dingtalk (
                        robot: '1174f280-4f5a-43e7-aa2f-a062c3808993',
                        type: 'MARKDOWN',
                        title: 'spring-boot项目',
                        text: [
                            '### Jenkins_job_spring-boot',
                            '---',
                            '- 状态：<font color=#00CD00 >${appName}回滚成功</font>',
                            '- 版本：${BUILD_NUMBER}',
                            '- [查看部署详情](${STATUS_URL})',
                            '- [查看日志Console](${CONSOLE_URL})'
                        ],
                        at: [
                            '15737927244'
                        ]
                    )
                }
                failure {
                    dingtalk (
                        robot: '1174f280-4f5a-43e7-aa2f-a062c3808993',
                        type: 'MARKDOWN',
                        title: 'spring-boot项目',
                        text: [
                            '### Jenkins_job_spring-boot',
                            '---',
                            '- 状态：<font color=#FF0000 >${appName}回滚失败</font>',
                            '- 版本：${BUILD_NUMBER}',
                            '- [查看部署详情](${STATUS_URL})',
                            '- [查看日志Console](${CONSOLE_URL})'
                        ],
                        at: [
                            '15737927244'
                        ]
                    )
                }
            }
        }
    }
}
```
