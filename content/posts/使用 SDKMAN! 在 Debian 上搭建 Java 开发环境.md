+++
date = '2026-08-04T20:23:39+08:00'
draft = false
title = '使用 SDKMAN! 在 Debian 上搭建 Java 开发环境'
description = '在 Debian 上使用 SDKMAN! 安装与管理 JDK、Maven、Gradle 等工具，轻松实现多版本 Java 环境的搭建与切换。'
tags = ['SDKMAN', 'Java', 'Debian', '开发环境']
+++

## 步骤 1：安装 SDKMAN!

运行官方提供的安装脚本：

```bash
curl -s "https://get.sdkman.io" | bash

```

安装完成后，终端会提示初始化脚本。运行以下命令使 SDKMAN! 环境变量在当前终端会话中生效：

```bash
source "$HOME/.sdkman/bin/sdkman-init.sh"

```

验证安装是否成功：

```bash
sdk version

```

> **输出示例：** `SDKMAN! 5.18.x`

---

## 步骤 2：使用 SDKMAN! 管理与安装 JDK

SDKMAN! 支持 Temurin、Liberica、Corretto、Zulu 等多家厂商的 JDK 发行版。

1. **列出可用 JDK 版本:**
使用 `list` 命令查看所有可用的 JDK 版本及其状态：

```bash
sdk list java

```

终端将以列表形式输出各个厂商（如 `tem` 表示 Temurin, `amzn` 表示 Corretto）的不同版本。


2. **安装指定版本的 JDK:** 以 LTS 版本 Java 21 (Temurin) 为例.
运行以下命令安装最新的 Java 21 LTS 版本：

```bash
sdk install java 21.0.12-tem
sdk install java 8.0.502-tem


```

安装完成后，SDKMAN! 会自动将该版本设为当前默认的 Java 环境。


3. **验证 JDK 安装:**
查看当前系统生效的 Java 版本：

```bash
java -version

```


4. **多版本切换与管理:**
若项目中同时需要使用 Java 17 和 Java 21，可轻松实现随时切换：

```bash
# 安装 Java 17
sdk install java 17.0.10-tem

# 仅在当前终端会话临时切换到 17
sdk use java 17.0.10-tem

# 将 Java 21 设为全局默认版本
sdk default java 21.0.2-tem

```


---

## 步骤 4：安装构建工具（Maven / Gradle）

除了 JDK，SDKMAN! 还能管理 Java 生态中的各类构建工具。

### 安装 Apache Maven

```bash
sdk install maven

```

### 安装 Gradle

```bash
sdk install gradle

```

安装完成后，运行 `mvn -v` 或 `gradle -v` 即可确认构建工具已就位。

---

## 常用命令速查

| 命令 | 说明 | 示例 |
| --- | --- | --- |
| `sdk current` | 查看当前终端生效的所有 SDK 版本 | `sdk current` |
| `sdk list <sdk>` | 查看某个 SDK 的所有可用版本及安装状态 | `sdk list java` |
| `sdk use <sdk> <version>` | **临时切换**当前终端会话的版本（不会改变全局默认配置） | `sdk use java 17.0.10-tem` |
| `sdk default <sdk> <version>` | 设置全局**默认版本**（对新打开的终端生效） | `sdk default java 21.0.2-tem` |
| `sdk home <sdk> <version>` | 获取指定 SDK 安装路径（常用于配置环境变量或 IDE） | `sdk home java 21.0.2-tem` |