+++
date = '2026-03-25T16:48:18+08:00'
draft = true
title = 'Debian环境安装zsh'
+++

## 更新软件包列表

首先，确保你的本地软件包索引是最新的：

```bash
sudo apt update
```

## 安装 Zsh

使用 `apt` 命令行工具进行安装：

```bash
sudo apt install zsh -y
```

## 验证安装

安装完成后，可以通过查看版本号来确认是否成功：

```bash
zsh --version
```

## 设置 Zsh 为默认 Shell

默认情况下，Debian 使用 Bash。如果你希望每次打开终端都使用 Zsh，请运行以下命令：

```bash
chsh -s $(which zsh)
```

> **注意：** 设置完成后，你需要**注销并重新登录**，或者重启系统，更改才会生效。

---

## 初始配置

当你第一次启动 Zsh 时，它会询问你如何配置环境。建议选择选项 `2`（使用推荐的默认设置创建一个空的 `.zshrc` 文件），之后你可以根据需要自行修改。

## 进阶建议：安装 Oh My Zsh

原生 Zsh 虽然强大，但大多数用户会安装 **Oh My Zsh** 来获得更美观的界面和丰富的插件支持。

**安装命令：**

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

安装后，你可以通过编辑 `~/.zshrc` 文件来更换主题（如 `agnoster` 或 `robbyrussell`）和开启插件（如 `git`, `zsh-autosuggestions` 等）。

## 安装插件

**安装命令：**

1. zsh-autosuggestions (自动建议)

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

2. zsh-syntax-highlighting (语法高亮)

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

**启用插件：**
安装/选定插件后，你需要修改配置文件：

1.  打开文件：`nano ~/.zshrc`
2.  找到 `plugins=(git)` 这一行，修改为你需要的插件名：
    ```bash
    plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
    ```
3.  保存退出后，执行：`source ~/.zshrc` 使其立即生效。
