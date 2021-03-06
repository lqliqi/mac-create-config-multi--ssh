# Mac 创建并配置多个 SSH-Key 以支持 github和gitlab等代码仓库

## 名词解释

简单来说，ssh是一种网络协议，用于计算机之间的加密登录。

如果一个用户从本地计算机，使用ssh协议登录另一台远程计算机，我们就可以认为，这种登录是安全的，即使被中途截获，密码也不会泄露。

需要指出的是，ssh只有一种协议，存在多种实现，既有商业实现，也有开源实现。
* * *
## 第一步 查看本机用户目录 ~ 下是否有 .ssh 文件夹 
### 进入该目录， 在终端输入:
```
cd ~/.ssh
```
### 如果没有该目录，创建该目录,在终端输入:
```
mkdir ~/.ssh
```
## 第二步 ，在 .ssh 目录下生成 ssh-key
### 在终端输入:
```
ssh-keygen -t rsa -C "youremail@email.com"
```

### 回车后，提示输入自定义的 ssh key 名字，我这里输入:
```
id_rsa_mt
```
### 输入完毕接着回车
![lqliqi 图标](https://raw.githubusercontent.com/lqliqi/mac-create-config-multi--ssh/main/imges/1.png "生成ssh-key")

## 第三步 为 ssh key 设置 git账号密码，
#### 输入两次密码，保持一致；建议这里不设置，直接回车两次，(方便后边开机添加自动程序执行:将私钥添加到 ssh-agent: )如图：
![lqliqi 图标](https://raw.githubusercontent.com/lqliqi/mac-create-config-multi--ssh/main/imges/2.png "为ssh-key设置密码")

### 进入到.ssh目录，查看生成的 .id_rsa_mt 和 .id_rsa_mt.pub 两个文件
```
 cd ~/.ssh
➜  .ssh ls
 id_rsa_mt  id_rsa_mt.pub
```

![lqliqi 图标](https://raw.githubusercontent.com/lqliqi/mac-create-config-multi--ssh/main/imges/3.png "成功生成.id_rsa_mt 和 .id_rsa_mt.pub 两个文件")

### 重复以上步骤，生成不同git 平台对应的 id_rsa_xxx 和 id_rsa_xxx.pub 文件

> GitHub 生成的对应ssh-key
> > id_rsa_github 私钥
> > id_rsa_github.pub 公钥

> Gitlab 生成的对应ssh-key 
> > id_rsa_mt 私钥
> > id_rsa_mt.pub 公钥

## 第四步 在 ~/.ssh 中 创建 config 文件并配置
```
➜  ~ cd ~/.ssh
➜  .ssh
touch config
vim config
```
##### touch config:创建config 空文件
##### vim config:编辑该文件

具体配置如下
```
Host github
HostName github.com
User git
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_github

Host gitlab
HostName gitlab.com
User git
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_mt

# 以 # 开头的是注释，会被忽略
# 每项配置都是 参数名 参数值 或者 参数名=参数值，参数名不分大小写，参数值区分
# Host: 机器别名，用于标识特定的配置
# HostName: 主机名，一般为 ip 或者 主机域名(实际测试 gitlab.com 或者 gitlab.company.com 都可用)
# IdentityFile: 私钥证书文件位置，没有pub后缀；默认位置是~/.ssh/id_rsa，如果采用默认证书，可不填此项
# Port: SSH访问主机的端口号，默认是22端口
```

## 第五步 将私钥添加到 ssh-agent
### 执行以下命令
```
ssh-add ~/.ssh/id_rsa_mt
```
#### 如何从ssh-agent 删除私钥呢，执行如下命令

```
ssh-add -d ~/.ssh/id_rsa_mt
```

### 输入git账户密码并回车
![lqliqi 图标](https://raw.githubusercontent.com/lqliqi/mac-create-config-multi--ssh/main/imges/4.png "私钥添加到ssh-agent")

### 显示这个说明此项操作成功
```
Identity added: /Users/liqiqi/.ssh/id_rsa_mt (youremail@email.com)
```

## 第六步 查看 ssh-agent 中的密钥是否添加成功,输入
```
ssh-add -l
```
### 显示以下说明成功
![lqliqi 图标](https://raw.githubusercontent.com/lqliqi/mac-create-config-multi--ssh/main/imges/6.png "查看ssh-agent 秘钥")

## 第七步 将各平台对应的 id_rsa_xxx.pub 添加到 GitHub/Gitlab

### 6.1 拷贝id_rsa_mt.pub 到剪切板
```
pbcopy < ~/.ssh/id_rsa_mt.pub
```

### 6.2 将该字符串拷贝粘贴到Git网站对应添加ssh-key的地方：

### Github: Setting->SSH and GPG keys
![lqliqi 图标](https://raw.githubusercontent.com/lqliqi/mac-create-config-multi--ssh/main/imges/9.png "github 添加 ssh key")
### Gitlab: Setting->SSH keys
![lqliqi 图标](https://raw.githubusercontent.com/lqliqi/mac-create-config-multi--ssh/main/imges/8.png "gitlab 添加 ssh key")
### 添加完成
## 第八步 使用 SSH克隆项目,这里以我自己的github 为例
```
git@github.com:lqliqi/mac-create-config-multi--ssh.git
```

![lqliqi 图标](https://raw.githubusercontent.com/lqliqi/mac-create-config-multi--ssh/main/imges/7.png "克隆项目 git clone")

## 你以为这样就完成了吗？不
### 当我们电脑重启的时候，我们第五步添加的私钥会丢失，所以我们需要开机的执行自动化程序添加私钥到 ssh-agent

## 第九步 找到 "自动操作" 程序并打开
![lqliqi 图标](https://raw.githubusercontent.com/lqliqi/mac-create-config-multi--ssh/main/imges/auto1.jpeg "找到 自动操作 程序并打开")

## 第十步 选取文稿类型为 “应用程序” ,点击 选取
![lqliqi 图标](https://raw.githubusercontent.com/lqliqi/mac-create-config-multi--ssh/main/imges/auto2.png "找到 应用程序，点击选取")
## 第十一步 选择 “实用工具”，双击 “运行shell脚本”
![lqliqi 图标](https://raw.githubusercontent.com/lqliqi/mac-create-config-multi--ssh/main/imges/auto3.png "双击 运行shell脚本")
## 第十二步 在右侧 shell脚本输入框中输入以下脚本
![lqliqi 图标](https://raw.githubusercontent.com/lqliqi/mac-create-config-multi--ssh/main/imges/auto4.jpeg "在右侧 shell脚本输入框中输入以下脚本")
```
ssh-add ~/.ssh/id_rsa_mt
ssh-add ~/.ssh/id_rsa_github
```
## 第十三步 点击窗口上方菜单 文件 -> 存储 为应用程序
![lqliqi 图标](https://raw.githubusercontent.com/lqliqi/mac-create-config-multi--ssh/main/imges/auto5.png "存储")
## 第十四步 设置文件名字为 autoAddSSH ，文件格式为： 应用程序，点击 存储

![lqliqi 图标](https://raw.githubusercontent.com/lqliqi/mac-create-config-multi--ssh/main/imges/auto6.png "存储")


## 第十五步 在 "用户与群组" 中 找到 登录项 ，找到 autoAddSSH.app 应用程序，添加进来，下次开机就会自动添加 私钥到 ssh-agent 中
![lqliqi 图标](https://raw.githubusercontent.com/lqliqi/mac-create-config-multi--ssh/main/imges/auto7.jpeg "存储")


### Done 完成所有配置
----------
## 如果对您有帮助，欢迎👏🏻 Star⭐️，Thanks♪(･ω･)ﾉ
