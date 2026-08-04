+++
date = '2026-08-04T20:23:39+08:00'
draft = false
title = '使用 SDKMAN! 在 Debian 上搭建 Java 开发环境'
description = '在 Debian 上使用 SDKMAN! 安装与管理 JDK、Maven、Gradle 等工具，轻松实现多版本 Java 环境的搭建与切换。'
tags = ['SDKMAN', 'Java', 'Debian', '开发环境']
+++

## 步骤 1：安装前置依赖

SDKMAN! 本身基于 Bash 脚本构建，安装需要系统预装 `curl`、`zip`、`unzip` 和 `zipnote` 等基础命令行工具。

打开终端，更新软件源并安装依赖：

```bash
sudo apt update
sudo apt install -y curl zip unzip

```

---

## 步骤 2：安装 SDKMAN!

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

## 步骤 3：使用 SDKMAN! 管理与安装 JDK

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

以下是 SDKMAN! 的常用命令分类汇总：

### 1. 版本查看与状态查询

| 命令 | 说明 | 示例 |
| --- | --- | --- |
| `sdk current` | 查看当前终端生效的所有 SDK 版本 | `sdk current` |
| `sdk current <sdk>` | 查看指定 SDK 在当前终端生效的版本 | `sdk current java` |
| `sdk list` | 列出 SDKMAN! 支持管理的所有工具（Java, Maven, Gradle, Kotlin 等） | `sdk list` |
| `sdk list <sdk>` | 查看某个 SDK 的所有可用版本及安装状态 | `sdk list java` |


### 2. 版本切换与默认值设置

| 命令 | 说明 | 示例 |
| --- | --- | --- |
| `sdk use <sdk> <version>` | **临时切换**当前终端会话的版本（不会改变全局默认配置） | `sdk use java 17.0.10-tem` |
| `sdk default <sdk> <version>` | 设置全局**默认版本**（对新打开的终端生效） | `sdk default java 21.0.2-tem` |
| `sdk home <sdk> <version>` | 获取指定 SDK 安装路径（常用于配置环境变量或 IDE） | `sdk home java 21.0.2-tem` |


### 3. 安装、卸载与本地离线版本关联

| 命令 | 说明 | 示例 |
| --- | --- | --- |
| `sdk install <sdk> [version]` | 安装指定版本（省略版本号则默认安装最新的 LTS/稳定版） | `sdk install maven` |
| `sdk uninstall <sdk> <version>` | 卸载已安装的指定版本 | `sdk uninstall java 11.0.22-tem` |
| `sdk install <sdk> <custom-name> <path>` | **关联本地手动下载的 SDK**（例如关联自行编译或解压的 JDK） | `sdk install java my-custom-17 /opt/jdk-17` |


### 4. 项目目录环境绑定 (`.sdkmanrc`)

| 命令 | 说明 | 示例 |
| --- | --- | --- |
| `sdk env init` | 在当前目录下生成 `.sdkmanrc` 配置文件（记录当前使用的环境） | `sdk env init` |
| `sdk env` | 加载并应用当前目录 `.sdkmanrc` 文件中记录的版本 | `sdk env` |
| `sdk env clear` | 清除 `.sdkmanrc` 带来的环境覆盖，恢复全局默认版本 | `sdk env clear` |


### 5. 工具维护与缓存清理

| 命令 | 说明 | 示例 |
| --- | --- | --- |
| `sdk update` | 刷新本地的 SDK 可用版本列表候选（当新版本 JDK 发布但列表没更新时使用） | `sdk update` |
| `sdk selfupdate` | 更新 SDKMAN! 工具本身到最新版本 | `sdk selfupdate` |
| `sdk flush archives` | 清理已下载的 SDK 安装包缓存，释放磁盘空间 | `sdk flush archives` |
| `sdk flush temp` | 清理 SDKMAN! 运行过程中产生的临时文件 | `sdk flush temp` |
| `sdk version` | 查看当前 SDKMAN! 版本 | `sdk version` |