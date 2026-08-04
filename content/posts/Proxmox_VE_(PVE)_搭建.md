+++
date = '2026-04-27T11:45:20+08:00'
draft = false
title = 'Proxmox_VE_(PVE)_搭建'
+++

## 1. 安装前准备
* **镜像下载**：前往 [Proxmox 官网](https://www.proxmox.com/en/downloads) 下载最新的 ISO 镜像。
* **启动盘制作**：使用 **Rufus**（推荐 DD 模式）或 **Ventoy** 将 ISO 写入 U 盘。
* **BIOS 设置**：
    * 开启 **Intel VT-x** 或 **AMD-V** 虚拟化技术。
    * 关闭 **Secure Boot**（安全启动）。
    * 设置 U 盘为第一启动项。

---

## 2. PVE 安装步骤
1.  **启动安装程序**：选择 `Install Proxmox VE (Graphical)`。
2.  **目标磁盘**：选择你要安装系统的硬盘。点击 `Options` 可以配置文件系统（单盘建议 `ext4`，多盘组阵列建议 `ZFS`）。
3.  **时区与键盘**：Country 输入 `China`，Timezone 选择 `Asia/Shanghai`。
4.  **密码与邮箱**：设置 root 密码（务必牢记）和通知邮箱。
5.  **网络配置**：
    * **Management Interface**：选择你的物理网卡。
    * **Hostname**：例如 `pve.home`。
    * **IP Address**：设置一个固定的静态 IP（例如 `192.168.1.100`）。
    * **Gateway/DNS**：指向你的路由器 IP。
6.  **确认安装**：完成后系统会自动重启，拨出 U 盘。

---

## 3. 基础初始化配置
安装完成后，通过浏览器访问 `https://你的IP:8006`，忽略证书警告，使用 `root` 和设置的密码登录。

### 3.1 更换国内软件源 (Debian 12 Bookworm 示例)
由于默认是企业订阅源，国内更新较慢，建议更换。

**修改基础源：**
```bash
sed -i 's/deb.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
sed -i 's/security.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
```

**修改 PVE 软件源：**
```bash
# 删除默认企业源
rm /etc/apt/sources.list.d/pve-enterprise.list
# 添加无订阅源
echo "deb https://mirrors.ustc.edu.cn/proxmox/debian/pve bookworm pve-no-subscription" > /etc/apt/sources.list.d/pve-no-subscription.list
```

**更新系统：**
```bash
apt update && apt upgrade -y
```

### 3.2 关闭订阅弹窗
如果你没有购买订阅服务，登录时会有弹窗。可以通过以下脚本一键移除：
```bash
sed -E -i.bak "s/(Ext.Msg.show\(\{)/void\(\1/g" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js && systemctl restart pveproxy
```

---

## 4. 存储与网络进阶

### 4.1 开启硬件直通 (IOMMU)
如果你需要将显卡、网卡或硬盘控制器直通给虚拟机，需开启 IOMMU。
1. 编辑 Grub：`nano /etc/default/grub`
2. 找到 `GRUB_CMDLINE_LINUX_DEFAULT="quiet"`：
   * **Intel CPU**: 改为 `"quiet intel_iommu=on"`
   * **AMD CPU**: 改为 `"quiet amd_iommu=on"`
3. 更新 Grub 并重启：`update-grub && reboot`

### 4.2 硬盘挂载
* **LVM-Thin**：适合存放虚拟机磁盘，支持快照，性能好。
* **Directory**：适合存放 ISO 镜像、备份文件。在 `数据中心 -> 存储 -> 添加` 中配置。
