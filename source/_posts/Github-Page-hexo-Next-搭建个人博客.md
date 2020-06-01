---
title: Github Pages + hexo Next 搭建个人博客
date: 2020-05-07 16:43:46
tags: hexo
---

#### 一、Github Pages 和 hexo是什么

   Github Pages是github提供的托管个人博客的工具，不过只能存放静态页面。通过修改项目的setting就可以开启Github Pages服务。配置好以后，它会给你一个可以访问的域名，这样你就不用去买服务器和域名以及备案了。

   hexo是一个搭建博客的框架，使用了node.js技术。用户只需要写Markdown文件(当然也可以写html)就能快速生成博客文章的页面。

   把二者结合起来搭建个人网站，不需要写一行代码，非常迅速。把hexo生成的内容直接push到Github上即可发布网站，博客管理变得非常方便。


#### 二、步骤

**1. 配置好一个Github Pages仓库**

   新建一个Github项目,命名规则为： [`Github用户名`].github.io

   建好以后开始配置Github Pages，步骤是：

   > Github项目 -> Setting -> GitHub Pages -> 选择Source

   不需要选择主题，如果选择主题则默认用Github提供的jekyll工具搭建网站，而我们使用的是hexo。到这里Github Pages的访问地址已经有了，但是还没有内容，因此不能访问。

**2. npm安装hexo**

   首先使用npm安装hexo-cli

   ```
   $ npm install -g hexo-cli
   ```

   看到控制台有安装成功的log后，可以开始使用hexo搭建网站了。

   新建一个项目文件夹，比如我的项目文件夹叫sproutlxq，在windows下的命令是：

   ```
   $ mkdir sproutlxq
   ```

   cd进入刚才新建的项目文件夹，然后依次运行下面两个命令：

   ```
    $ hexo init   #初始化hexo博客
    $ npm install   #安装必要组件
   ```

   这时候hexo已经为我们创建了一个hello_world的默认网页。但是在新版hexo中，还需要安装一个叫hexo-server的模块，才能启动本地服务。

   ```
   $ npm install hexo-server --save
   ```

   安装成功后启动本地服务：

   ```
   $ hexo server
   $ hexo s   #简写的命令
   $ hexo s --debug  #开启debug模式
   ```

   当我们看到服务启动成功的提示后，就可以根据提示去访问 http://localhost:4000/
   

**3. 安装next主题Next**

   hexo官方网站上有很多主题，可以选择自己喜欢的主题。按照github上的star数，Next主题是最受欢迎的。我也选择了Next。

   在项目文件夹下面，执行主题安装命令：

   ```
   $ git clone https://github.com/iissnan/hexo-theme-next themes/next
   ```
   这个主题会自动被安装到项目文件夹下的themes文件夹内。

   接下来要启用Next 主题，在站点配置文件中配置。
   `theme: next`

   到此，NexT 主题安装完成。下一步我们将验证主题是否正确启用。在切换主题之后、验证之前， 我们最好使用 ` hexo clean` 来清除 Hexo 的缓存。

   运行 `hexo s --debug` 命令启动本地服务器，如果看到如下界面，说明安装成功了。

   ![Next 主题](http://theme-next.iissnan.com/uploads/five-minutes-setup/validation-default-scheme-mac.png)

   Next主题目前提供了五种Scheme供用户选择，默认是Muse，我觉得Pisces也很漂亮，所以更改下主题配置文件<kbd>_config.yml</kbd>

   ```
   #scheme: Muse
   #scheme: Mist
   scheme: Pisces
   ```
   
   把不启用的scheme先注释掉。刷新页面就可以看到效果了。

   接下来还可以参考[hexo 官方网站](http://theme-next.iissnan.com/getting-started.html) ，去配置语言、菜单等内容。
   
   有兴趣的还可以仔细浏览主题配置文件和站点配置文件，修改自己感兴趣的配置项。

**4. 发布到Github**

   有两种方式可以把我们上面生成的网站发布到Github上：HTTPS和SSH。HTTPS比较简单，但SSH一劳永逸，我用了SSH的方式，这里偷懒只记录SSH方式。

   首先，要安装hexo-deployer-git组件。

   ```
   $ npm install hexo-deployer-git --save
   ```

   看下本地git的配置：
   ```
   git config --global user.name
   git config --global user.email
   ```

   如果没有配置好就配置一下。

   ```
   git config --global user.name [your name]
   git config --global user.email [your email]
   ```
   接下来，我们就可以根据上面配置的用户名与邮箱生成SSH公钥私钥（RSA算法），我们将公钥发给GitHub的SSH管理员时，我们如果拿着私钥去访问GitHub，GitHub SSH管理员就可以进行公钥私钥配对，来完成认证。当然，第一次将公钥给GitHub时，GitHub管理员是需要我们输入密码来认证的。

   确认配置过git用户名与邮箱之后，运行以下命令，生成公钥私钥。连续回车使用默认参数。

   ```
   $ ssh-keygen -t rsa -C "xxx@xxx.com"  #邮箱为GitHub邮箱
   ```

   根据控制台的提示，找到公钥私钥存放的位置。按照下面的步骤把公钥告诉Github。

   > 将公钥复制 -> 打开GitHub -> settings -> SSH and GPG keys -> New SSH key -> title随便起，将公钥复制到key框中 -> Add SSH key -> 输入GitHub密码 -> 成功。

   接下来修改站点配置文档 <kbd>_config.yml</kbd>

   ```
   # Deployment
   ## Docs: https://hexo.io/docs/deployment.html
   deploy:
      type: git
      repository: git@github.com:xxx/xxx.github.io.git
      branch: master
   ```

   将repository修改为我们Github的ssh地址。SSH地址可以在GitHub -> GitHub page库 -> clone or download -> Use SSH获取.

   修改完成之后，可以运行以下命令来进行部署：
   ```
   hexo clean            #清除之前静态页面
   hexo generate            #生成静态页面
   hexo deploy(可缩写为d)  # 部署

   ```

   至此部署完成，访问github page给的地址，就可以看到自己的个人页面了。
   https://sproutlxq.github.io/

**5. 发布新文章**

  新建博文的命令是：

  ```
  hexo new [title]
  ```
  
  这时候在post文件夹下就生成了一个新的.md文件，里面就可以写自己的新文章了。

  hexo文章里引用本地图片一般需要使用npm安装新的hexo功能插件，比较麻烦，更推荐使用图床服务，比如：https://sm.ms/