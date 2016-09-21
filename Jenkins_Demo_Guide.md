#Jenkins Demo 说明

Demo 地址: [http://139.196.179.230:8100](http://139.196.179.230:8100)

使用到的jenkins插件	

1. GitHub plugin,  用于git commit触发及git checkout。

2. Publish Over SSH, 用于远程传输文件及执行命令。

3. HTTP Request Plugin, 用于发送http请求。

4. Version Number Plug-In， 用于修改具体build的名称

5. Cobertura Plugin/Jococo Plugin， 用于展示测试覆盖率

6. Findbugs Plug-In, 用于展示findbugs检查的问题


## 普通Jenkins job配置
Demo中参看demo tab下的tasks [详情](http://139.196.179.230:8100/view/demo/)
###1. Job:Spring-boot-demo
Spring-boot-demo 主要的流程如下

1. 本地通过github plugin进行主动式触发，进行本地findbugs代码静态检查 (前期可以跳过此步骤，此步骤建立在有良好的代码规范的基础上)

2. 进行本地ut测试并计算覆盖率（前期可以跳过此步骤，后期必须加入以保证提交质量）

3. 对代码进行打包。

4. 将本地包通过Publish Over SSH pulgin的sftp上传到远程服务器的depoly文件夹。jar文件存储在/home/deploy/jenkins-Spring-boot-demo-$build_number中。

5. 在远程服务器中 通过cp命令 将jar 文件放入/home/deploy/demo，命名为demo.jar. 并通过supervisor进行重启java服务。

6. 结束后调用job check_demo 进行httpstatus的检查

###2. Job:rollback_demo
rollback_demo 主要通过已经成功部署过的Spring-boot-demo-normal的版本号进行回滚

1. 传入需要回滚的对应的jenkins中Spring-boot-demo-normal的参数类似"Spring-boot-demo-normal-36"

2. 通过将远程服务器中备份的jar包重新覆盖/home/deploy/demo/demo.jar 并使用supervisor进行重启java服务。

## Pipeline Jenkins job配置
Pipeline是jenkins1.6之后加入的功能即工作流模式，利用groovy对工作流进行划分。

Demo中参看demo pipeline下的tasks [详情](http://139.196.179.230:8100/view/pipeline/)
###1. Job:pipeline_demo 的主要流程分三部分
1. Init 初始化相关
2. UnitTesting 单元测试并生成报告
3. Checkout git代码签出
4. Build 打包
5. SFTP and Restart service 远程传输打包文件并重启服务

###2. Job:remote_server_pipeline_demo 
主要负责上传包及远程重启服务的job

