---
title: 如何为 GitHub 配置 SSH 密钥
date: 2024-11-29 18:03:46
tags:
- github
- ssh
- macos
---

在使用 GitHub 执行 `pull`、`push` 或 `fetch` 等操作时，如果提示“权限不足”:

```shell
> OpenSSH_8.1p1, LibreSSL 2.7.3
> debug1: Reading configuration data /Users/YOU/.ssh/config
> debug1: Reading configuration data /etc/ssh/ssh_config
> debug1: /etc/ssh/ssh_config line 47: Applying options for *
> debug1: Connecting to github.com port 22.
```

可能需要配置 SSH 密钥来建立安全连接。以下步骤基于 macOS 系统。

---

<!--more-->

### **1. 创建 SSH 密钥**

#### 推荐使用 ED25519 算法生成密钥

ED25519 是一种更安全且现代的算法，建议优先使用。如果您的系统较旧且不支持 ED25519，则可以选择 RSA 算法。

运行以下命令生成密钥：

```shell
ssh-keygen -t ed25519 -C "your_email@example.com"
```

如果需要使用 RSA：

```shell
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

系统提示时按如下步骤操作：

1. **设置密钥保存路径**  
   当看到以下提示时，直接按 `Enter` 使用默认路径即可：
   
   ```shell
   > Enter a file in which to save the key (/Users/YOU/.ssh/id_ALGORITHM): [Press enter]
   ```

2. **设置访问密码**（可选）  
   系统会提示设置访问密码，但可以直接跳过，不填写：

```shell
   > Enter passphrase (empty for no passphrase): [Type a passphrase]
   > Enter same passphrase again: [Type passphrase again]
```

### **2. 将 SSH 密钥添加到 ssh-agent**

> **背景知识：**  
> `ssh-agent` 是 SSH 的一个组件，主要用于管理私钥，帮助用户自动完成公钥认证，从而避免重复输入密码。

#### **步骤：**

1. **启动 `ssh-agent`**  
   使用以下命令启动 `ssh-agent`：
   
   ```shell
   $ eval "$(ssh-agent -s)"
   > Agent pid 90076
   ```
   
   返回类似以下内容，表示启动成功：

```shell
   > Agent pid 90076
```

2. **配置自动加载密钥**  
   如果您使用的是 macOS Sierra 10.12.2 或更高版本，请编辑 `~/.ssh/config` 文件，添加以下内容：
   
   ```text
   Host github.com
     AddKeysToAgent yes
     UseKeychain yes
     IdentityFile ~/.ssh/id_ed25519
   ```
   
   > **说明：**
   > 
   > - `AddKeysToAgent yes`：允许将密钥添加到 `ssh-agent`。
   > - `UseKeychain yes`：启用 macOS 钥匙串管理密码。
   > - `IdentityFile`：指定您的私钥文件路径。

3. **将密钥添加到 `ssh-agent` 并存储密码**  
   使用以下命令添加私钥并将密码存储到钥匙串中：
   
   ```shell
   ssh-add --apple-use-keychain ~/.ssh/id_ed25519
   ```

---

### **3. 在 GitHub 中添加新的 SSH 公钥**

1. **复制 SSH 公钥**  
   使用以下命令将公钥复制到剪贴板：
   
   `pbcopy < ~/.ssh/id_ed25519.pub`

2. **在 GitHub 中添加公钥**
   
   1. 打开 GitHub 网站，单击右上角的头像，进入 **Settings**。
   2. 在左侧菜单的 "Access" 部分，点击 **SSH and GPG keys**。
   3. 点击 **New SSH key**。
   4. 在 **Title** 输入框中，为此密钥命名（例如：MacBook Pro SSH Key）。
   5. 将之前复制的公钥粘贴到 **Key** 输入框中。
   6. 点击 **Add SSH key** 保存。

---

### **4. 测试 SSH 连接**

使用以下命令测试 SSH 连接是否正常：

`ssh -T git@github.com`

如果配置正确，您会看到如下信息：

`Hi USERNAME! You've successfully authenticated, but GitHub does not provide shell access.`

---

### **5. 克隆仓库**

验证配置是否成功，尝试从 GitHub 克隆一个仓库：

`git clone git@github.com:GITHUBID/REPOSITORY_NAME.git`

如果克隆成功，说明您的 SSH 配置已正确完成！ 🎉

---

### **附加提示**

- 建议定期更新 SSH 密钥，提升安全性。
- 如果遇到问题，可检查 `~/.ssh/config` 文件是否正确配置，或参考 GitHub 官方文档。
