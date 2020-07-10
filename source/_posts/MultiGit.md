---
title: 配置多个git账号
date: 2020-07-10 18:23:57
tags:
---

### 按目录配置多用户

在2017年，git新发布的版本2.13.0包含了一个新的功能includeIf配置，可以把匹配的路径使用对应的配置用户名和邮箱；

在~/目录下面存在三个配置文件，

- .gitconfig // 全局通用配置文件
- .gitconfig-self // 个人工程配置文件
- .gitconfig-work // 公司工程配置文件

全局通用配置文件~/.gitconfig里面的内容是：主要是通过includeIf配置匹配不用的目录映射到不同配置文件上，

```
[includeIf "gitdir:~/self-workspace/"]
    path = .gitconfig-self
[includeIf "gitdir:~/workspace/"]
    path = .gitconfig-work
```

个人工程配置文件~/.gitconfig-self：

```
[user]
    name = yourname-self
    email = yourname-self@gmail.com
```

公司工程配置文件~/.gitconfig-work：

```
[user]
    name = yourname-work
    email = yourname-work@yourCompanyName.com
```

可能遇到的问题：

1. 文件~/.gitconfig里面的includeIf后面的path最后需要/结尾
2. 文件~/.gitconfig里面原有的user部分需要删除
3. 个人工程目录和公司工程目录需要要求是非包含关系，就是这两个工程目录配置路径不可以是父子关系。

### 设置多个 SSH keys

SSH keys里面包含了账号和邮箱信息，所以新账号必须另外在生成一份SSH keys。
- 用ssh-keygen命令生成一组新的id_rsa_new和id_rsa_new.pub

```
ssh-keygen -t rsa -C "new email"
```

平时我们都是直接回车，默认生成id_rsa和id_rsa.pub。这里特别需要注意，出现提示输入文件名的时候(Enter file in which to save the key (~/.ssh/id_rsa): id_rsa_new)要输入与默认配置不一样的文件名，比如：我这里填的是 id_rsa_new

注：windows用户和mac用户的区别就是，mac中.ssh文件夹在根目录下，所以表示成~/.ssh/,而windwos用户是C:\Users\Administrator\.ssh。
下面同理，不在赘述。
- 执行ssh-agent让ssh识别新的私钥。
因为默认只读取id_rsa，为了让SSH识别新的私钥，需将其添加到SSH agent中：

```
ssh-add ~/.ssh/id_rsa_work   
```

如果出现Could not open a connection to your authentication agent的错误，就试着用以下命令：

```
ssh-agent bash
ssh-add ~/.ssh/id_rsa_work
```

- 配置~/.ssh/config文件
前面我们在~/.ssh目录下面，使用ssh-keygen -C “your_email” -t rsa 生成公私秘钥，当有多个github账号的时候，可以生成多组rsa的公司密钥。然后配置~/.ssh/config文件（如果没有的话请重新创建一个）。

```
touch config        # 创建config文件
```

然后修改如下：
我的config配置如下：

```
# 该文件用于配置私钥对应的服务器
# Default github user(chping_2125@163.com)
Host git@github.com
 HostName https://github.com
 User git
 IdentityFile ~/.ssh/id_rsa

 # second user(second@mail.com)
 # 建一个github别名，新建的帐号使用这个别名做克隆和更新
Host git@code.xxxxxxx.com
 HostName https://code.xxxxxxx.com    #公司的gitlab
 User git
 IdentityFile ~/.ssh/id_rsa_new
```


### 参考资料
1. git config配置多用户场景实践: [https://segmentfault.com/a/1190000019714862](https://segmentfault.com/a/1190000019714862)
2. 同一客户端下使用多个git账号: [https://www.jianshu.com/p/89cb26e5c3e8](https://www.jianshu.com/p/89cb26e5c3e8)
3. 如何生成SSH key: [https://www.jianshu.com/p/31cbbbc5f9fa/](https://www.jianshu.com/p/31cbbbc5f9fa/)