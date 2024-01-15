---
title: 解锁你的GitHub：一步步解决'Permission denied (publickey)'错误
date: 2024-01-11 17:06:50
tags:
- github
- ssh
---

![](https://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/picgo/202401122133765.png)

>  如果在github pull/push/fetch时提示 **Permission denied (publickey)** 没权限，则需要为github配置ssh密钥，这里针对macOS系统举例，亲测可用。

# 创建SSH密钥

```shell
ssh-keygen -t ed25519 -C "youremail@gmail.com"
```

建议使用 ED25519 这个新算法，如果是旧的系统不支持，则可以使用rsa，那么后续流程中的文件名也要更改为`id_rsa`。

```shell
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
<!-- more -->
```shell
> Enter a file in which to save the key (/Users/YOU/.ssh/id_ALGORITHM): [Press enter]
```

看到提示，按enter即可。

```shell
> Enter passphrase (empty for no passphrase): [Type a passphrase]
> Enter same passphrase again: [Type passphrase again]
```

提示输入访问密码，可以忽略不填。

# 将 SSH 密钥添加到 ssh-agent

> SSH-Agent是SSH (Secure Shell) 的一个组件，SSH是一种网络协议，用于安全地连接到远程服务器。SSH-Agent的主要作用是管理用户的私钥，帮助用户自动处理公钥认证，从而避免了多次输入密码的麻烦。

1. 后台启动ssh-agent
   
   ```shell
   $ eval "$(ssh-agent -s)"
   > Agent pid 90076
   ```

2. 如果您使用的是 macOS Sierra 10.12.2 或更高版本，则需要修改 `~/.ssh/config` 文件以自动将密钥加载到 ssh-agent 中并将密码存储在钥匙串中。修改该文件添加以下内容。
   
   ```text
   Host github.com
   AddKeysToAgent yes
   UseKeychain yes
   IdentityFile ~/.ssh/id_ed25519
   ```

3. 将您的 SSH 私钥添加到 ssh-agent 并将您的密码存储在钥匙串中。
   
   ```shell
   ssh-add --apple-use-keychain ~/.ssh/id_ed25519
   ```

# 在github中添加新的SSH密钥

1. 将 SSH 公钥 `~/.ssh/id_ed25519.pub`复制到剪贴板。
2. 在github的右上角，单击您的个人资料照片，然后单击**Settings**。
3. 在侧边栏的“访问”部分中，单击 **SSH and GPG keys**。
4. 单击 **New SSH key** 。
5. Title 填入你喜欢的名称。
6. Key type 选 Authentication Key。
7. Key 粘贴SSH公钥

# 测试SSH连接

```shell
$ ssh -T git@github.com
## 出现以下内容则代表公钥匹配正确
> Hi USERNAME! You've successfully authenticated, but GitHub does not provide shell access.
```

# 克隆仓库

克隆仓库看看是否能成功。👍

```shell
$ git clone git@github.com:GITHUBID/REPOSITORY_NAME.git
```
