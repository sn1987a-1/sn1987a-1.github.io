---
title:  以git-ftp为主的学习记录 
comments: true
toc: true
categories:
  - git
tags:
  - wsl2
  - git
abbrlink: 15203
date: 2022-2-27 15:46:11
cover: https://user-images.githubusercontent.com/74918703/155833584-1eba0e4e-2d29-48a6-a5a2-c04cc982b0ec.png

---

# 以git-ftp为主的学习记录

> 原本的目的是美化个人FTP主页（之前都是用FileZilla进行课程pdf文件等的传输，看到同学的个人主页后突然意识到也可以利用hexo完成相关功能，虽说没什么用，但也记录一下学习的过程。
>
> ......遇到了好多问题> _ <尚未完成
>
> 操作环境：win11 wsl2+Ubuntu 20.04 LTS,USTC ftp（默认首页为public_html下的index.html文件

## git-ftp关联

优点：便于管理ftp

在github中创建名为`git-ftp`的repo后，使用`git clone`操作克隆到本地（或者可以在本地进行`git init`操作，创建.git文件，使用`git remote`操作与远程链接；

因为2021年8月13起github不允许在git操作时进行登录确认，因此要提前配置好：

```shell
git config --global user.name YOURNAME
git config --global user.email YOUREMAIL
```

要使用git-ftp的功能，需要先安装git-ftp：（确保git已经安装）`sudo apt-get install git-ftp `

此外，要进行ftp的config操作：

```shell
git config git-ftp.url ftp-url
git config git-ftp.user ftp-user
git config git-ftp.password pswd
```

若想对以上内容进行查看，可以在本仓库`.git`目录进行查看和修改

```yaml
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[git-ftp]
        password = pswd
        user = name
        url = ftp-url
[remote "origin"]
       #后序添加完可以查看url和fetch内容
```



对于git-ftp的相关操作，可以阅读 [官方文档](https://github.com/git-ftp/git-ftp/blob/master/man/git-ftp.1.md) 。

之后可以对该目录进行初始化:`git ftp init`,或者对已存在的文件进行`git catchup`操作。

之后常用的操作就是`git ftp push`.顺便提一句，可能会遇到“Dirty repository :Having uncommited changes.Exiting..."的问题，就要先进行`git add .`,`git commit ..`,`git push`的操作。

此时仍无法进行`git ftp push`的操作，因为这一操作需要安装lftp：`sudo apt-get install lftp`,安装后即可使用。

## hexo搭建

为了防止hexo操作将其他文件一并删除，此处可以新建一个文件夹存储hexo框架：`hexo init page`,打开目录底下的_config.yml进行修改，并且将deploy进行修改：(对生成的public_dir文件的位置大概也需要改)

PS:对于_config.yml的内容修改，可以不需要重新进行`hexo g`

```yaml
deploy:
 type: ftpsync
 host: # ftp服务器地址
 user: # ftp用户名
 pass:  # 你的ftp用户密码
 remote:  # 你要上传到的地址，例如/wwwroot
 port:  # ftp端口，不同的ftp可能会不一样
 delete: true # 上传本地文件是否删除ftp中的所有文件
 verbose: true # 是否打印调试信息
 ignore_errors: false # 是否忽略错误
```

大部分的操作为hexo的常规操作，但与利用域名搭建的hexo博客不同，这里需要安装插件：

`npm install hexo-deployer-ftpsync –save`

这里还有一个问题，`hexo d`后，我的原本存在public_html底下的文件被删除？

如果使用git clone安装了新主题，如此次安装了"Archer"主题，若hexo版本在4.0.0以上，需要将/themes/Archer目录下的_config.yml重命名为 _config.archer.yml，并移动到根目录，同时，因为使用的是`git clone`操作， 会将原repo的.git文件clone到本地，默认为与原repo关联，这个repo我们是没有权限进行修改的，相对于两个git项目进行了嵌套，外层无法update内部的repo，这时在根目录下进行`git add .`或者`git add all`操作均是无法将Archer文件的内容进行push的，也就无法进行`git ftp push`操作，解决方法就是利用`git remote remove`操作取消内部的git关联。 

## 遇到的琐事

使用hexo操作时 ，发生对于Yaml的报错提示，从StackOverflow得知需要更新npm版本，遂进行更新，使用`npm install -g npm`时，又发生报错，大致意思为可以检测到新版本但是当前的node.js不兼容新版本，无法完成更新，而且此项更新未完成时，可能会对于npm安装其他的插件有影响。

google了半天，终于搞清楚了：

我在一开始安装npm时未安装nvm，二者的关系是：npm时node.js的包管理工具，nvm是node.js的版本管理工具，二者均是node.js应用程序开发的工具，因此要更新Node.js到最近版本，需要先安装nvm，`$ wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | sh`,其中sh可以替换成shell的具体名称，如我的就是zsh，默认为bash，安装完之后可以在~/.nvm看到该文件夹，不知道为什么，我的执行上述操作后无反应，重新启动了wsl才正常进行，完成后，在对应的配置文件( ~ /.bashrc或者~/.zshrc)中添加：

```shell
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

完成后保存退出，`source ~/.zshrc`，即可使用nvm命令

`nvm install`可以指定安装Node.js 的版本 ，可以选比较近的稳定版本17.0.0，安装完后`nvm ls`查看已安装版本：

```shell
❯ nvm ls
->      v17.0.0
         system
default -> v17.0.0
iojs -> N/A (default)
unstable -> N/A (default)
node -> stable (-> v17.0.0) (default)
stable -> 17.0 (-> v17.0.0) (default)
lts/* -> lts/gallium (-> N/A)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.17.0 (-> N/A)
lts/dubnium -> v10.24.1 (-> N/A)
lts/erbium -> v12.22.10 (-> N/A)
lts/fermium -> v14.19.0 (-> N/A)
lts/gallium -> v16.14.0 (-> N/A)
```

之后就可以对npm进行更新了。`npm -v`可以查看现有的版本，问题解决了

这里还有一个问题，就是nvm安装的版本居然也不是全局版本，在另外一个目录下进行`npm -v`或者`node -v`竟然也是原版本...





emmmm 后面暴躁到不想记录了呜呜呜 

## 利用CSS对界面优化

2022.3.1update:learn how CSS works

CSS相关资源：

https://chokcoco.github.io/CSS-Inspiration/#/

https://cssreference.io/


这个网站整合了很多CSS工具：

https://juejin.cn/post/6982363593241002014

HTML CSS工具网站,可以copy已有模板（nice！）和实时观察到html,CSS,JS的呈现结果  https://codepen.io 
