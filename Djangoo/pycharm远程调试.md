## pycharm远程调试

##### **简介**：主要介绍了如何使用PyCharm进行远程开发和调试

##### 解决场景：

- 在本地开发应用程序，但是最后需要在linux服务器上运行。

- 开发时没有问题但是到了正式的Linux环境下面却出现问题，需要保证开发环境跟运行环境的一致。

##### Pycharm支持四种远程调试：

- vagrant：有了Docker不用它了

- SFTP：与SSH连接方式结合使用，可以把一个项目的interper由本地/远程-->远程/另一个远程

- SSH：远程调试的主要方法
- Docker：据说很棒，有机会再试

本文主要介绍SFTP与SSH两种调试方法

##### 系统：

- 本地：Mac OS
- 服务器：Ubuntu16.04
- IDE：Pycharm

##### 准备：

- 服务器开启ssh远程登录

- 服务器和本地同时安装pydevd包

  ~~~shell
  pip install pydevd 
  ~~~

#### 一、本地与服务器代码同步

**1.打开Tools --> Deployment --> Configuration**

![Snip20181108_4](https://ws1.sinaimg.cn/large/006tNbRwly1fx0pqdsq96j31bg0wgb29.jpg)

**2.点击左上角+按钮，设置Type为SFTP，配置远程服务器的ip、用户名和密码，点击Test按钮进行连接测试，如果返回Success，则表示服务器链接成功**

![Snip20181108_5](https://ws4.sinaimg.cn/large/006tNbRwly1fx0pqr5757j31c814s476.jpg)

**3.配置Mappings参数设置，进行本地项目路径和远程服务器项目路径的关联，红框中的路径为远程服务器项目路径。注意：Deploy path on server 这里填写相对于root path的目录.**

![Snip20181108_6](https://ws2.sinaimg.cn/large/006tNbRwly1fx0prdf5f0j318q0moq5z.jpg)

**4.打开 Tools  -->  Deployment  -->  Automatic upload，开启自动上传。现在Pycharm就可以自动同步文件了，同时之前灰色的选项都可以选择了，如果需要直接从服务器下载，只需要新建一个同名空文件夹，选择Download from 从服务器将项目代码都下载到本地**

- Upload to ：上传代码
- Download from ：从服务器下载代码
- Compare with Deployed to：对比远程目录下文件和本地文件的差异
- Sync with deployed to：对比远程和本地文件的差异，而且可以进行同步

![image-20181108141648522](https://ws4.sinaimg.cn/large/006tNbRwly1fx0prkq9fbj31dy0vkhdt.jpg)

#### 二、PyCharm远程Debug

远程Debug需要完成两个配置

- 配置PyCharm的Python解释器，使用服务器上的Python解释器 

- 配置PyCharm的Debug功能，让PyCharm运行服务器上的代码

**首先配置解释器：**

**1.进入 Preference  --> Project Interpreter，点击⚙，选择Add...**

![Snip20181108_7](https://ws4.sinaimg.cn/large/006tNbRwly1fx0prq7oxtj31kw0jqjz6.jpg)

**2.进入界面后，选择SSH Interpreter，设置host和username后next**

![Snip20181108_9](https://ws1.sinaimg.cn/large/006tNbRwly1fx0prtqlyrj31ec0uggpe.jpg)

**3.输入密码后next**

![Snip20181108_10](https://ws3.sinaimg.cn/large/006tNbRwly1fx0prxgvs6j31ek0uotc5.jpg)

**4.修改解释器为远程服务器项目所用到的解释器，并且修改项目映射路径，点击文件夹图标**

![image-20181108113834885](https://ws1.sinaimg.cn/large/006tNbRwly1fx0ps1aoggj31e00uojua.jpg)

**5.修改Remote Path为项目路径，可以点击后进入选择，完成后点击Finish结束（这个也可以在选取解释器的时候进行设置）**

![Snip20181108_12](https://ws4.sinaimg.cn/large/006tNbRwly1fx0ps5sn5bj31eg116wwz.jpg)

**6.完成后PyCharm就将远程服务器上相应Python解释器的相关文件都同步下来，需要等待一下，然后就会出现下面界面，观察Project Interpreter，它已经是远程服务器的Python环境了**

![Snip20181108_13](https://ws3.sinaimg.cn/large/006tNbRwly1fx0psa9ut4j31kg0kkwlq.jpg)

**接下来配置Pycharm Debug界面：**

**7.确保解释路径映射与服务器项目路径一致**

![Snip20181108_15](https://ws4.sinaimg.cn/large/006tNbRwly1fx0psdyfntj31ke0lin1w.jpg)

**8.在项目启动文件manage.py中打开  Run-->Edit Configuration...，设置Parameters ，查看interpreter是否正确**

![Snip20181108_14](https://ws3.sinaimg.cn/large/006tNbRwly1fx0psib6sdj31kw10ftfd.jpg)

**9.所有都配置完后，就可以进行Debug了，此时使用的就是远程的Python解释器，Debug的也是远程服务器的Python代码**

