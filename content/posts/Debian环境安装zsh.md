+++
date = '2026-03-25T16:48:18+08:00'
draft = false
title = 'Debian环境安装zsh'
+++

## 更新软件包列表
首先，确保你的本地软件包索引是最新的：
```bash
sudo apt update
```

## 安装 Zsh 及必要依赖
使用 `apt` 安装 Zsh 以及后续配置 Oh My Zsh 所需的 `curl` 和 `git`：
```bash
sudo apt install zsh curl git -y
```

## 验证安装
安装完成后，确认 Zsh 已成功安装：
```bash
zsh --version
```

## 设置 Zsh 为默认 Shell
默认情况下 Debian 使用 Bash。运行以下命令将当前用户的 Shell 切换为 Zsh：
```bash
chsh -s $(which zsh)
```
> **注意：** 修改后需要**注销并重新登录**（或重启系统），更改才会完全生效。

---

## 初始配置
当你第一次启动 Zsh 时（输入 `zsh` 即可进入），会看到配置界面。
* 建议选择选项 **`2`**：使用推荐的默认设置创建一个空的 `.zshrc` 文件。

## 安装 Oh My Zsh
安装 Oh My Zsh 以获得强大的插件管理和主题支持：
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

---

## 安装外部插件
在 Oh My Zsh 框架下，执行以下命令克隆常用插件到自定义目录：

1. **zsh-autosuggestions (自动建议)**
```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

2. **zsh-syntax-highlighting (语法高亮)**
```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

---

## 启用插件与配置
安装插件后，必须在配置文件中声明才能生效。

1.  **打开配置文件：**
    ```bash
    nano ~/.zshrc
    ```
2.  **查找并修改 `plugins` 行：**
    找到 `plugins=(git)`，将其修改为（插件名称之间用**空格**分隔）：
    ```bash
    plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
    ```
3.  **（可选）修改主题：**
    找到 `ZSH_THEME="robbyrussell"`，可以按需更改主题名称（如 `ys` 或 `agnoster`）。
4.  **保存并退出：**
    按下 `Ctrl + O` 保存，`Enter` 确认，`Ctrl + X` 退出。
5.  **使配置立即生效：**
    ```bash
    source ~/.zshrc
    ```