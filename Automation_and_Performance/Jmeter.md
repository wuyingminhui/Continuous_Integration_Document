#Jmeter 自动化测试及性能测试指导文档
Apache JMeter是Apache组织开发的基于Java的压力测试工具。

在测试web应用或者c/s应用的时候也可作为自动化测试回归工具使用。

它主要的特点: 开源，学习曲线平滑，功能全面，文档齐全。

***注: 本文所用Jmeter可在[这里](https://pan.baidu.com/s/1o8tpajg)下载***

##API接口测试的流程
1. 项目启动后，测试人员要尽早找到开发人员拿到接口测试文档
　
2. 获取接口测试文档后，就可以进行接口用例的编写和调试
　
3. 接口用例编写调试完成后，部署到持续集成的测试环境中
　
4. 设定脚本运行频率，告警方式等基本参数，进行接口的日常监控
　
5. 和开发人员一起处理接口异常


###1. 了解Jmeter基本流程用法
  1. 打开Jmeter, bin目录下执行jmeter.bat(windows)或者jmeter.sh(linux)
  ![1](https://github.com/wuyingminhui/Continuous_Integration_Document/blob/master/Automation_and_Performance/img/jmeter1.jpg)
  
  2. 添加线程组, 在“测试计划”上点击鼠标右键-->添加-->threads(Users)-->线程组
  
  添加测试场景设置组件，接口测试中一般设置为1个“线程数”，根据测试数据的个数设定“循环次数”
  ![2](https://github.com/wuyingminhui/Continuous_Integration_Document/blob/master/Automation_and_Performance/img/jmeter2.jpg)
  3. 添加'HTTP Cookie管理器', 主要用于保存必要的请求头(如不需要登入信息保存的请跳过这步骤)
  ![3](https://github.com/wuyingminhui/Continuous_Integration_Document/blob/master/Automation_and_Performance/img/jmeter3.jpg)
  
  4. 添加'Http请求默认值'组件，填写被测系统的域名和端口，http请求的实现包版本以及具体协议类型，线程组里的所有“HTTP Sampler”可默认使用此设置。用于统一区分环境的测试。(可以理解为之后HTTP请求的基类)
  ![4](https://github.com/wuyingminhui/Continuous_Integration_Document/blob/master/Automation_and_Performance/img/jmeter4.jpg)

  5. 在'线程组'里添加'HTTP 请求'的Sampler, 填写请求路径，对应的请求方法，以及随请求一起发送的参数
  ![5](https://github.com/wuyingminhui/Continuous_Integration_Document/blob/master/Automation_and_Performance/img/jmeter5.jpg)
  
  6. 设置检查点：'线程组'-'断言'-'响应断言'。在被测接口对应的'HTTP 请求'上，添加'响应断言'并添加对相应结果的正则判断。
  ![6](https://github.com/wuyingminhui/Continuous_Integration_Document/blob/master/Automation_and_Performance/img/jmeter6.jpg)
  
  7. 添加监听器：'线程组'-'监听器'-'查看结果树', 方便查看运行后的结果
  ![7](https://github.com/wuyingminhui/Continuous_Integration_Document/blob/master/Automation_and_Performance/img/jmeter7.jpg)

###2. Jmeter做接口自动化测试
  通过上述的基本用法添加的http请求测试可以理解为一个HTTP接口测试，按照自己的需求将接口组织完善即可。
  
  Jmeter与持续集成的结合：
   1. 触发机制: 可以在每次代码修改部署完成后通过Jenkins命令行触发。
   2. 报告机制: 通过添加Jenkins email notification, 当有api接口回归测试fail的时候需要及时通知。

###3. Jmeter做性能测试
  Jmeter的性能测试主要通过线程组的线程数及集合点的设置来做。
  
  一般性能测试分为压力测试及性能测试两种:
  
  1. 压力测试即在一定性能压力下跑持续耐久测试，看服务器的响应及资源使用情况。(baseline 测试)
  2. 性能测试及不断测试被测端的压力承受能力得到其的性能最高瓶颈 (capacity 测试)
  
  这里 图一 为一般压力测试的线程组配置。
  配置线程数100， 集合点为5秒，持续时间为30秒。
  ![imgp3](https://github.com/wuyingminhui/Continuous_Integration_Document/blob/master/Automation_and_Performance/img/performance3)
  图二为一般性能测试配置
  配置线程数100， 集合点为10秒，跑1次性能测试。
  ![imgp1](https://github.com/wuyingminhui/Continuous_Integration_Document/blob/master/Automation_and_Performance/img/performance1)
  图三为设置的聚合报告(添加'监听器' - '聚合报告')
  ![imgp2](https://github.com/wuyingminhui/Continuous_Integration_Document/blob/master/Automation_and_Performance/img/performance2)
  
  
当一台压力机器不够时也可通过agent的方式连接多台压力机器。


***Guide用到的demo可在[这里](https://github.com/wuyingminhui/Continuous_Integration_Document/tree/master/Automation_and_Performance/demo)下载***
  
