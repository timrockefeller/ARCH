---
title: Permission denied when git push
date: 2018-12-15 22:53:00
tags: coding
---

在很长一段时间没有使用git push 后，你的sshkey可能会过期：

```
Permission denied(publickey).
fatal: Could not read from remote repository
Please make sure you have the correct access rights and the repository exists.
```

解决方案：

1. 

```shel
ssh-keygen -t rsa -C "youremail@example.com"
ssh -v git@github.com
ssh-agent  -s
# eval `ssh-agent  -s`
ssh-add ~/.ssh/id_rsa  
cat  ~/.ssh/id_rsa.pub
```

*注意，上述youremail@example.com是指github账户的注册邮箱*

上述命令执行后id_rsa.pub文件内容将输出到终端，复制里面的密钥（内容一般是以ssh-rsa 开头，以github账号的注册邮箱结尾的，全部复制下来）至github设置中的ssh-keys。

最后运行以下命令

```
ssh -T git@github.com
```

> Hi ---! You've successfully authenticated, but GitHub does not provide shell access.