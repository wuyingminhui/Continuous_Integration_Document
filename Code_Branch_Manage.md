#分支管理建议

现阶段分支管理分为Dev, Master.

需要增加一Release 分支。
 ![image](https://github.com/ctofunds/ctofunds/blob/master/LOGO/jenkins.png)

下面列下分支主要区分：

###1. Dev 分支
开发分支， 主要用于开发人员开发feature及自测。

***之后可以将可以被多个feature开发分支取代。***

***[注意]:可以酌情不在Jenkins中建立相关的job对应dev分支***

###2. Master 分支
当开发dev自测完成之后可以合并dev相关代码到Master分支。并开给测试人员进行测试


***[注意]: 这里需要构建Jenkins的job对master分支进行自动化发布。这里设置github的branch对应master branch。***

这个环境中建议需要加入java junit的测试即mvn 打包不跳过test。
测试人员如果有相关的测试job可以作为依赖的job当构建发布完成之后触发


###3. Release 分支
未包含新发布的Release分支必须保持和外网的代码同步。
当Master上的代码完成测试之后，通过git tag命令对master上需要上线的代码进行打tag。
然后在Release分支上进行tag的合并。 

***[注意]: 对应Release分支的jenkins job不建议使用git提交触发。使用人为主动触发。***
另外可以加入含有少量smoke的api测试的job作为发布之后的验证。


Codereview 初期可以在提交dev分支的时候发起pull request给leader进行review。


