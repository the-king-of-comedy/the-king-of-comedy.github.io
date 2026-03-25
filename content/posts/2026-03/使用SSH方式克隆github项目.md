+++
date = '2026-03-25T15:15:53+08:00'
draft = true
title = '使用SSH方式克隆github项目'
+++

## 1. 生成 SSH 密钥对
首先，检查系统是否已有密钥，若没有则生成一对新的（公钥和私钥）。

```bash
# 检查是否已有密钥
ls -al ~/.ssh

# 生成新密钥（建议使用更安全的 ed25519 算法）
# "your_email@example.com" 仅为标注信息，可自定义修改
ssh-keygen -t ed25519 -C "your_email@example.com"
```
*按回车键确认保存路径（默认即可），接着会提示设置密码短语（Passphrase），可以直接回车跳过，也可以设置以增强安全性。*

---

## 2. 将公钥添加到 GitHub
你需要告诉 GitHub 你的公钥，以便它识别你的身份。

1.  **复制公钥内容：**
    ```bash
    cat ~/.ssh/id_ed25519.pub
    ```
    *选中并复制终端输出的以 `ssh-ed25519` 开头的整行字符串。*

2.  **在 GitHub 设置：**
    * 登录 GitHub，点击右上角头像 -> **Settings**。
    * 左侧菜单选择 **SSH and GPG keys**。
    * 点击 **New SSH key**，Title 随便起（如 "Debian-Laptop"），将复制的内容粘贴到 **Key** 框中，点击 **Add SSH key**。

---

## 3. 测试连接
在克隆之前，验证一下配置是否成功：

```bash
ssh -T git@github.com
```
*如果是第一次连接，会提示 `Are you sure you want to continue connecting (yes/no/[fingerprint])?`，输入 **yes** 并回车。如果看到 "Hi [你的用户名]! You've successfully authenticated..."，说明配置成功。*

---

## 4. 克隆项目
现在你可以使用 SSH 地址克隆仓库了。

1.  在 GitHub 项目页面，点击绿色的 **Code** 按钮。
2.  切换到 **SSH** 选项卡，复制地址（格式为 `git@github.com:用户名/仓库名.git`）。
3.  在 Debian 终端执行：

```bash
# 示例命令
git clone git@github.com:username/repository.git
```