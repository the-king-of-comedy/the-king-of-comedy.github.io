+++
date = '2026-04-27T13:39:20+08:00'
draft = false
title = '通过 WSL2 和 vs Code 搭建 Java 开发环境'
+++

## 1. 准备工作：启用 WSL2

如果你尚未安装 WSL2，请按以下步骤操作：

1.  **以管理员身份打开 PowerShell**。
2.  运行安装命令（默认安装 Ubuntu）：
    ```powershell
    wsl --install
    ```
3.  重启计算机。
4.  重启后，根据提示设置 Linux 的用户名和密码。

---

## 2. 安装并配置 VS Code

1.  在 Windows 上下载并安装 [Visual Studio Code](https://code.visualstudio.com/)。
2.  打开 VS Code，进入插件市场（`Ctrl+Shift+X`），搜索并安装 **WSL** 扩展插件。
3.  点击 VS Code 左下角的蓝色角标 `><`，选择 **Connect to WSL**。连接成功后，左下角会显示 `WSL: Ubuntu`（或你安装的发行版名称）。

---

## 3. 在 WSL2 中安装 Java (JDK)

在 WSL 终端中，我们推荐使用包管理器安装 JDK。

### 更新系统软件包
```bash
sudo apt update && sudo apt upgrade -y
```

### 安装 JDK
你可以选择安装常用的 OpenJDK（例如 Java 17 或 Java 21）：
```bash
# 安装 OpenJDK 17
sudo apt install openjdk-17-jdk -y
```

### 验证安装
```bash
java -version
javac -version
```

---

## 4. 配置 Java 环境变量

通常通过 `apt` 安装的 JDK 会自动配置，但为了确保工具链正常，建议手动检查环境变量。

1.  编辑配置文件：`nano ~/.bashrc`
2.  在文件末尾添加以下内容（路径请根据实际版本确认）：
    ```bash
    export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
    export PATH=$PATH:$JAVA_HOME/bin
    ```
3.  使配置生效：`source ~/.bashrc`

---

## 5. 安装 VS Code Java 扩展包

**重要提示：** 必须在 VS Code 连接到 WSL 的状态下安装扩展。

1.  在 VS Code 插件市场搜索 **Extension Pack for Java**（由 Microsoft 提供）。
2.  点击 **Install in WSL: [Your Distro Name]**。该扩展包包含：
    * Language Support for Java™ by Red Hat
    * Debugger for Java
    * Maven for Java
    * Project Manager for Java

---

## 7. 安装构建工具

如果你需要开发复杂的项目，建议安装 Maven 或 Gradle。

* **安装 Maven：**
    ```bash
    sudo apt install maven -y
    mvn -version
    ```
* **安装 Gradle：**
    推荐使用 [SDKMAN!](https://sdkman.io/) 来管理 Java 相关的多个版本：
    ```bash
    curl -s "https://get.sdkman.io" | bash
    source "$HOME/.sdkman/bin/sdkman-init.sh"
    sdk install gradle
    ```