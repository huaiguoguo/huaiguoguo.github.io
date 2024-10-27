### 先安装opensshd软件 <a href="https://www.mls-software.com/opensshd.html" target="_blank">https://www.mls-software.com/opensshd.html</a>
<a href="[https://www.google.com](https://www.mls-software.com/opensshd.html)" target="_blank">点击这里官网下载</a>


生成 SSH 密钥的过程很简单，你可以按照以下步骤在不同的操作系统上生成 SSH 密钥。这些步骤适用于 Windows、macOS 和 Linux，确保你可以为 Git 或其他服务生成一个新的 SSH 密钥对。

### 步骤：生成 SSH 密钥

#### 1. 打开终端或命令行
- **Windows**：可以使用 Git Bash 或 PowerShell。
- **macOS/Linux**：直接打开终端。

#### 2. 生成 SSH 密钥对
在终端中运行以下命令来生成一个新的 SSH 密钥对：

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

- `-t rsa`：指定使用 `RSA` 算法生成密钥。
- `-b 4096`：指定密钥长度为 4096 位，增强密钥的安全性。
- `-C "your_email@example.com"`：为该密钥附加一个注释，通常是你的邮箱地址，用来标识密钥。

#### 3. 保存密钥
系统会提示你选择保存生成密钥的位置。通常情况下，默认的保存路径为 `~/.ssh/id_rsa`（或 Windows 的 `C:\Users\<YourUser>\.ssh\id_rsa`）。你可以直接按 `Enter` 使用默认位置，也可以输入一个自定义的文件名和路径。

```bash
Enter file in which to save the key (/Users/you/.ssh/id_rsa):
```

#### 4. 设置密钥密码（可选）
接下来，系统会提示你为生成的密钥设置一个密码。如果你想要为 SSH 密钥增加一层安全性，可以设置密码；如果你不想每次使用密钥时都输入密码，可以直接按 `Enter` 跳过。

```bash
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```

#### 5. 查看生成的 SSH 密钥
SSH 密钥对由两个文件组成：
- **私钥**：默认路径为 `~/.ssh/id_rsa`，这个文件要妥善保管，不要泄露。
- **公钥**：默认路径为 `~/.ssh/id_rsa.pub`，可以安全地提供给远程服务器或服务（如 GitHub、GitLab）。

#### 6. 添加 SSH 密钥到 SSH 代理（可选，但推荐）
如果你使用的是 macOS 或 Linux，并且希望在当前会话中自动加载 SSH 密钥，你可以使用 SSH 代理来管理 SSH 密钥。

首先，启动 SSH 代理：
```bash
eval "$(ssh-agent -s)"
```

然后，将你生成的 SSH 私钥添加到 SSH 代理中：
```bash
ssh-add ~/.ssh/id_rsa
```

#### 7. 将 SSH 公钥添加到远程服务（GitHub、GitLab 等）
将生成的公钥复制到剪贴板中，以便添加到 GitHub 或 GitLab 账户中。可以使用以下命令查看并复制公钥内容：

```bash
cat ~/.ssh/id_rsa.pub
```

你会看到类似于以下的输出：

```bash
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC9i5...your_email@example.com
```

将其复制，然后根据你使用的服务，将公钥粘贴到该服务的 SSH 密钥设置中。

- **GitHub**：[SSH 密钥添加教程](https://github.com/settings/keys)
- **GitLab**：[SSH 密钥添加教程](https://gitlab.com/profile/keys)

#### 8. 测试 SSH 连接
添加公钥后，你可以测试 SSH 是否成功连接到远程服务。例如，测试是否成功连接到 GitHub：

```bash
ssh -T git@github.com
```

如果成功，你会看到如下的输出（如果是第一次连接，可能会提示是否继续连接，输入 `yes` 即可）：

```bash
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

### 总结

1. 打开终端，运行 `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"` 生成 SSH 密钥。
2. 按 `Enter` 使用默认保存路径，设置或跳过密钥密码。
3. 将公钥 (`~/.ssh/id_rsa.pub`) 添加到远程服务（GitHub、GitLab 等）。
4. 测试 SSH 连接，确保配置正确。

通过这些步骤，你就可以生成并使用 SSH 密钥与远程服务进行安全通信。


<br>
<br>
<br>

# 无密码ssh登录 linux

在 Windows 10 上，默认情况下 `ssh-copy-id` 命令是不可用的，因为它是 Linux 或类 Unix 系统中的一个工具。Windows 10 自带的 OpenSSH 客户端不包含这个工具。如果你在 Windows 上想将本地的 SSH 公钥复制到远程 Linux 服务器（如 CentOS 7）上，可以通过以下几种方式来实现。

### 方法 1：手动复制 SSH 公钥

1. **查看并复制公钥**：
   打开命令提示符，查看并复制你的公钥内容：

   ```bash
   type C:\Users\<你的用户名>\.ssh\id_rsa.pub
   ```

   复制输出的公钥内容。

2. **在虚拟机上手动添加公钥**：
   使用密码登录到你的 CentOS 7 虚拟机：

   ```bash
   ssh <用户名>@<虚拟机IP地址>
   ```

   登录后，创建 `.ssh` 目录并设置权限：

   ```bash
   mkdir -p ~/.ssh
   chmod 700 ~/.ssh
   ```

   然后将你在 Windows 上复制的公钥粘贴到 `authorized_keys` 文件中：

   ```bash
   echo "<你的公钥内容>" >> ~/.ssh/authorized_keys
   ```

   最后，设置 `authorized_keys` 文件的权限：

   ```bash
   chmod 600 ~/.ssh/authorized_keys
   ```

### 方法 2：通过 `cat` 和 `ssh` 将公钥复制到远程服务器

你可以通过 `ssh` 命令将本地公钥通过 `cat` 命令添加到远程服务器的 `authorized_keys` 文件中。

1. **使用以下命令将本地公钥传送到虚拟机**：

   ```bash
   type C:\Users\<你的用户名>\.ssh\id_rsa.pub | ssh <用户名>@<虚拟机IP地址> "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys"
   ```

   这条命令的作用：
   - `type C:\Users\<你的用户名>\.ssh\id_rsa.pub` 会显示你的公钥内容。
   - `ssh <用户名>@<虚拟机IP地址>` 通过 SSH 登录到虚拟机。
   - `mkdir -p ~/.ssh` 会在虚拟机上创建 `.ssh` 目录（如果不存在）。
   - `cat >> ~/.ssh/authorized_keys` 会将公钥内容追加到 `authorized_keys` 文件中。
   - `chmod 700 ~/.ssh` 和 `chmod 600 ~/.ssh/authorized_keys` 会设置正确的权限。

### 方法 3：使用第三方工具

如果你不想手动复制，可以在 Windows 上安装一些工具来简化这一过程：

1. **Git Bash**：
   安装 [Git for Windows](https://gitforwindows.org/)，它包含了一个 Bash 环境和大部分常用的 Unix 工具，包括 `ssh-copy-id`。

   安装完成后，在 Git Bash 中执行以下命令将公钥复制到远程服务器：
   
   ```bash
   ssh-copy-id <用户名>@<虚拟机IP地址>
   ```

2. **PuTTYgen + PuTTY**：
   如果你使用的是 PuTTY 作为 SSH 客户端，你可以使用 `PuTTYgen` 生成密钥，并通过 `Pageant` 管理你的 SSH 密钥，也可以通过 `PuTTY` 的终端手动将公钥复制到服务器。

### 总结

由于 Windows 10 没有内置的 `ssh-copy-id` 工具，你可以选择手动复制公钥、使用 `ssh` 和 `cat` 来传输公钥，或者安装 Git Bash 等第三方工具来简化操作。