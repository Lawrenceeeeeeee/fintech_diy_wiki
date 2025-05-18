---
comments: true
---

# Linux

金融科技、金融工程的同学在大二之后基本上就要开始做一些机器学习、深度学习的项目了。其中有很多项目是需要大量算力和长期稳定运行的，因此你们大概率需要租借服务器。我们学校的GPU服务器平台交流群里有很多人来问问题，但大多数都是一些简单到不能再简单的问题。这个时候，你就需要具备基础的Linux操作知识。

推荐学习资料：
- [【推荐！】100个关于Linux的常识](https://www.bilibili.com/video/BV1BMGkzcE21?vd_source=9e94008dbf76e399a164028430118348)
- [Linux 教程](https://www.runoob.com/linux/linux-tutorial.html)

## 什么是Linux

Linux 内核最初只是由芬兰人林纳斯·托瓦兹（Linus Torvalds）在赫尔辛基大学上学时出于个人爱好而编写的。

我们现在常说的“Linux”其实是Linux发行版，简单点就是将 Linux 内核与应用软件做一个打包。目前市面上较知名的发行版有：**Ubuntu（常用）**、RedHat、**CentOS（也很常用）**、Debian、Fedora、SuSE、OpenSUSE、Arch Linux、SolusOS 等。

## 如何远程登录Linux服务器

### 1. SSH登录

Secure Shell(SSH) 是由 IETF(The Internet Engineering Task Force) 制定的建立在应用层基础上的安全网络协议。它是专为远程登录会话(甚至可以用Windows远程登录Linux服务器进行文件互传)和其他网络服务提供安全性的协议，可有效弥补网络中的漏洞。通过SSH，可以把所有传输的数据进行加密，也能够防止DNS欺骗和IP欺骗。还有一个额外的好处就是传输的数据是经过压缩的，所以可以加快传输的速度。目前已经成为Linux系统的标准配置。

配置好服务器、改好密码后，就可以开始SSH登录了。一般来说，服务器提供商都会帮你配置好SSH（不然你也没办法连）。

基础的连接命令：
```
ssh <username>@<ip> [-p <port>]
```

示例：
```
ssh root@192.168.0.1
```

然后按照提示输入密码就可以登录了。

当然，还有更加简便的方式。在本机上的`~/.ssh/config`中可以加入形如这样的配置：

```
// 第一个服务器
Host test
  HostName 159.75.155.105
  User root

// 第二个服务器
Host my_server
  HostName 8.209.215.61
  User root
  IdentityFile ~/.ssh/id_rsa
```

拿第二个作为例子，每次登录的时候，就用不着写`ssh root@8.209.215.61`了，直接`ssh my_server`即可。

当然，你一定会觉得每次都要输入一遍密码太费事儿了。我们还有更加快捷、安全的方案——密钥对登录。

首先，在本机上生成密钥对：

```
ssh-keygen
```

然后跟着引导一步步走就可以，密码什么的用不着设置。如果你之后要存很多私钥的话，建议还是分别命名，不然容易混淆。

接着将公钥上传到目标服务器
```
ssh-copy-id -i ~/.ssh/<你的公钥文件>.pub <username>@<ip>
```

这样就配置好了！直接登录即可，无需密码

```
ssh root@192.168.0.1
```

如果还是需要密码的话，需要在远程服务器上配置`sshd_config`

```
vim /etc/ssh/sshd_config
```

然后修改如下内容

```
# 开启RSA认证
RSAAuthentication yes
# 公钥认证
PubkeyAuthentication yes
# 认证的公钥信息所在文件
AuthorizedKeysFile .ssh/authorized_keys
# 允许root账户登录
PermitRootLogin yes
# 允许密码认证（暂时设置成yes，密钥对登录设置完之后建议改成no，安全性更高）
PasswordAuthentication yes
```

修改完成后按`:wq`保存退出（这是vim的内容，后面会讲到），然后重启sshd服务：

```
systemctl restart sshd
```

这样应该就可以了。

### 2. SSH客户端登录

市面上有很多做得很好的SSH客户端，可以帮你省去很多麻烦。

强力推荐：
[termius](https://termius.com)

### 3. 主流IDE的SSH插件

现在大部分的IDE都有SSH的功能，可以帮助你高效地在远程服务器上办公，例如Visual Studio Code、Jetbrains系列IDE（例如Pycharm、Clion、Idea）等。

虽然它们确实很好用，但我还是把他们放在最后讲，因为这种方法比前面提到的几种方法更“笨重”。为了更好地使用IDE的功能，连接的时候往往会在远程服务器上安装IDE的Server，占用很大的运行内存。如果你租用的是非常经济的小服务器，内存就2G甚至更低的话，再部署一点其他服务（比如用Docker起服务），这个服务器很快就会卡死、掉线，你只能重启。这种方法只推荐在性能相对充裕的服务器上使用。

[VSCode配置 SSH连接远程服务器+免密连接教程](https://zhuanlan.zhihu.com/p/667236864?s_r=0)

## Linux 基础命令

和我们常见的Windows、Mac这种图形化界面的操作系统不同，Linux是以命令行为基础的。

[【Linux】Linux常用命令60条（含完整命令语句）](https://blog.csdn.net/wzk4869/article/details/132855372)

## Vim

Vim是一种文本编辑工具，是从vi发展出来的。简单的来说，vi 是老式的字处理器，不过功能已经很齐全了，但是还是有可以进步的地方。 vim 则可以说是程序开发者的一项很好用的工具。连 vim 的官方网站 (https://www.vim.org/) 自己也说 vim 是一个程序开发工具而不是文字处理软件。如果使用得当的话，工作效率可能比你在Word上敲字还高。

[Vim编辑器使用教程（非常详细，基础操作看一篇就够了）](https://blog.csdn.net/weixin_58849785/article/details/137153298)

我理解迁移到Vim上很困难，所以如果你感觉还是很难掌握的话，可以考虑使用[LunarVim](https://www.lunarvim.org)，你可以理解为“命令行中”的VSCode，支持鼠标操作，可以自由安装各种插件（甚至可以接入Copilot，实现AI自动补全代码）。
