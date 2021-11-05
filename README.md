# Mac 创建并配置多个 SSH 以支持 github和gitlab等代码仓库

## 第一步 生成 ssh-key
### 在终端输入:
```
ssh-keygen -t rsa -C "youremail@email.com"
```
### 回车后，提示输入自定义的 ssh key 名字，我这里输入:
```
id_rsa_mt
```
### 输入完毕接着回车
![lqliqi 图标](https://github.com/lqliqi/mac-create-config-multi--ssh/blob/main/imges/1.png?raw=true "生成ssh-key")

## 第二步 为 ssh key 设置 git账号密码，
#### 输入两次，保持一致，可直接回车，不设置，建议这里设置，如图：
![lqliqi 图标](https://github.com/lqliqi/mac-create-config-multi--ssh/blob/main/imges/2.png?raw=true "为ssh-key设置密码")

### 进入到.ssh目录，查看生成的 .id_rsa_mt 和 .id_rsa_mt.pub 两个文件
```
 cd ~/.ssh
➜  .ssh ls
 id_rsa_mt  id_rsa_mt.pub
```
### 重复以上步骤，生成不同git 平台对应的 id_rsa_xxx 和 id_rsa_xxx.pub 文件

> GitHub 生成的对应ssh-key
> > id_rsa_github 私钥
> > id_rsa_github.pub 公钥

> Gitlab 生成的对应ssh-key 
> > id_rsa_mt 私钥
> > id_rsa_mt.pub 公钥

## 第三步 在 ~/.ssh 中 创建 config 文件并配置
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

Host gitlab.com
HostName gitlab.com
User git
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_mt

# IdentityFile: 填写的是私钥名，没有pub后缀
```

## 第四步 将私钥添加到 ssh-agent
### 执行以下命令
```
ssh-add ~/.ssh/id_rsa_mt
```
### 输入git账户密码并回车
![lqliqi 图标](https://github.com/lqliqi/mac-create-config-multi--ssh/blob/main/imges/4.png?raw=true "私钥添加到ssh-agent")

### 显示这个说明此项操作成功
```
Identity added: /Users/liqiqi/.ssh/id_rsa_mt (youremail@email.com)
```

## 第五步 查看 ssh-agent 中的密钥是否添加成功,输入
```
ssh-add -l
```
### 显示以下说明成功
![lqliqi 图标](https://github.com/lqliqi/mac-create-config-multi--ssh/blob/main/imges/6.png?raw=true "查看ssh-agent 秘钥")

## 第六步 将各平台对应的 id_rsa_xxx.pub 添加到 GitHub/Gitlab

### 6.1 拷贝id_rsa_mt.pub 到剪切板
```
pbcopy < ~/.ssh/id_rsa_mt.pub
```

### 6.2 将该字符串拷贝粘贴到Git网站对应添加ssh-key的地方：

### Github: Setting->SSH and GPG keys
![lqliqi 图标](https://github.com/lqliqi/mac-create-config-multi--ssh/blob/main/imges/9.png?raw=true "github 添加 ssh key")
### Gitlab: Setting->SSH keys
![lqliqi 图标](https://github.com/lqliqi/mac-create-config-multi--ssh/blob/main/imges/8.png?raw=true "gitlab 添加 ssh key")
### 添加完成
## 第七步 使用 SSH克隆项目,这里以我自己的github 为例
```
git@github.com:lqliqi/mac-create-config-multi--ssh.git
```

![lqliqi 图标](https://github.com/lqliqi/mac-create-config-multi--ssh/blob/main/imges/7.png?raw=true "克隆项目 git clone")

### Done 完成所有配置