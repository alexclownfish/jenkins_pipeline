## jenkins gitlab nexus3/harbor 此类清单已有，安装部署不再叙述
Github: https://github.com/alexclownfish/jenkins_pipline
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

