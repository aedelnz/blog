---
title: PRoot-Distro安装与使用
published: 2026-04-28
description: PRoot-Distro 是一个用于 Android 设备的 Linux 发行版安装脚本。
image: ''
tags: [Android, Termux]
category: 默认
draft: false
lang: ''
---

一个用于管理基于 chroot 的 Linux 发行版安装的 Bash 脚本包装器。它不需要 root 权限或任何特殊的 ROM、内核等。开始使用所需的一切就是最新版本的 [Termux] 应用程序。详细信息请参阅[安装](#安装)。

::github{repo="termux/proot-distro"}

如果你喜欢这个项目并想要支持开发，请考虑[捐赠](#捐赠)。

***

## 捆绑的发行版

PRoot Distro 为常用发行版提供了一套最小根文件系统 tarball。每个发行版保证至少支持 AArch64（ARM64）CPU。为了减少维护工作量，我们只打包单一版本的发行版（stable、lts 或 rolling-release），极少数例外。

| 发行版 | PD 别名 | 版本 | 状态 |
|------------------|------------|------------|---------------|
| Termux | termux | rolling | supported |
| Adelie Linux | adelie | 1.0-beta6 | no i686 |
| AlmaLinux | almalinux | 10 | only 64bit |
| Alpine Linux | alpine | 3.22.2 | frozen |
| Arch Linux | archlinux | rolling | supported |
| Artix Linux | artix | rolling | aarch64 only |
| Chimera Linux | chimera | rolling | unstable |
| Debian | debian | trixie | supported |
| Deepin | deepin | beige | only 64bit |
| Fedora | fedora | 43 | unstable |
| Manjaro | manjaro | rolling | aarch64 only |
| OpenSUSE | opensuse | Leap 16.0 | only 64bit |
| Oracle Linux | oracle | 10 | ⛔️ |
| Pardus | pardus | yirmibes | ⛔️ |
| Rocky Linux | rockylinux | 10 | only 64bit |
| Trisquel | trisquel | aramo | supported |
| Ubuntu | ubuntu | 25.10 | no i686 |
| Void Linux | void | rolling | supported |
| Guix | N/A | N/A | not supported |
| NixOS | N/A | N/A | not supported |
| *其他* | *DIY* | *DIY* | *DIY* |

关于发行版状态的说明：

* `supported` 表示期望在 aarch64、arm、i686 和 x86_64 设备上无严重问题地运行。
* `only 64bit` 表示发行版仅适用于 64 位架构：aarch64、x86_64 等。
* `no armv7`、`no ...` 表示缺少对特定架构的支持。
* `frozen` 表示由于已知问题，该发行版可能不会收到版本升级。
* `unstable` 表示已知该发行版在与 proot 一起使用时存在严重问题。

所有内容均按原样提供。根文件系统 tarball 是从所选发行版的仓库生成的内容创建的，我们这边没有任何修改。

构建由 [GitHub Actions](https://github.com/termux/proot-distro/actions) 自动化：

* 配置：https://github.com/termux/proot-distro/blob/master/.github/workflows/build.yml
* 根文件系统打包脚本：https://github.com/termux/proot-distro/tree/master/distro-build

我们不为此处列出的任何发行版开发软件包，因此关于它们的错误报告将被忽略。尽管在大多数"错误"情况下，你可能需要归咎于 Android OS 或 [proot] 工具。

PRoot Distro 只是 [proot] 的一个包装器（启动器）。

如果你需要自定义版本，你需要自己添加。请参阅[添加发行版](#添加发行版)。

### 安全

**用户必须在安装发行版后升级软件包，以确保存在所有最新的错误修复和安全补丁。**

我们目录中发行版的根文件系统存档根据需求更新。实际上，这意味着新安装的发行版可能已经过时几个月了。

请记住，`proot`（`proot-distro` 的核心）不像 `docker`、`firejail` 和其他类似的知名工具那样提供高级别隔离。

### 对于 ARMv9 用户

符合 ARMv9 标准的 SoC 不包含对 32 位指令集的支持。这是与 ARMv8 的主要区别。

由于某些用户可能没有预料到 32 位支持在硬件级别被丢弃，当检测到缺少 32 位指令时，`proot-distro` 会显示红色警告。一旦 ARMv9 变得更加普遍，我们将移除该警告。

在具有 ARMv9 架构的设备上，32 位发行版将被视为外来架构，你需要 `qemu-user-arm` 模拟器软件包。

## 安装

使用包管理器：
```
pkg install proot-distro
```

使用 git：
```
pkg install git file proot
git clone https://github.com/termux/proot-distro
cd proot-distro
./install.sh
```

依赖项：bash、bzip2、coreutils、curl、file、findutils、gawk、gzip、ncurses-utils、proot、sed、tar、util-linux、xz-utils

如果你想要命令行自动补全，请安装 `bash-completion` 软件包。

## 功能概述

PRoot Distro 旨在提供管理已安装发行版的所有功能：安装、卸载、备份、恢复、登录。每个操作都通过命令定义。每个命令都接受其特定于所执行任务的唯一选项集。

基本用法：
```
proot-distro <command> <arguments>
```

替代变体（v4.0.0+）：
```
pd <command> <arguments>
```

其中 `<command>` 是 proot-distro 操作命令（请参阅下文了解有哪些可用），`<arguments>` 是给定命令特定选项的列表。

安装发行版的示例：
```
proot-distro install debian
```

某些命令支持别名。例如，你可以输入：
```
proot-distro list
proot-distro install debian
proot-distro login debian
proot-distro remove debian
```

或者这样：
```
proot-distro ls
proot-distro i debian
proot-distro sh debian
proot-distro rm debian
```

有关支持别名的信息，可以在每个命令的帮助中查看。

已知的发行版通过插件脚本定义，这些脚本定义了要下载根文件系统存档的 URL 和用于完整性检查的校验和集合。插件还可以定义在发行版安装期间执行的一组命令。

请参阅[添加发行版](#添加发行版)了解更多如何向 PRoot Distro 添加自己的发行版。

### 访问内置帮助

命令：`help`

此命令将显示有关 `proot-distro` 用法的帮助信息。
* `proot-distro help` - 主页面。
* `proot-distro <command> --help` - 查看特定命令的帮助。

### 备份发行版

命令：`backup`

别名：`bak`、`bkp`

将指定发行版及其插件备份到 tar 存档。备份的内容可以打印到 stdout 进行进一步处理或写入文件。

压缩方式根据文件扩展名确定，例如 `.tar.gz` 将使用 GZip 压缩，`.tar.xz` 将使用 XZ。管道备份数据始终不压缩，以便用户自由进行进一步处理。

使用示例：
```
proot-distro backup debian | xz | ssh example.com 'cat > /backups/pd-debian-backup.tar.xz'
proot-distro backup --output backup.tar.gz debian
```

*此命令是通用的。所有额外处理（如加密）应由用户通过外部命令完成。*

### 安装发行版

命令：`install`

别名：`add`、`i`、`in`、`ins`

安装由别名指定的发行版 - 一个指向所选发行版插件的短名称。

使用示例：
```
proot-distro install alpine
```

默认情况下，已安装的发行版将与命令行中指定的别名相同。这意味着你将无法同时安装多个副本。你可以在安装时使用选项 `--override-alias` 重命名发行版，这将创建发行版插件的副本。

使用示例：
```
proot-distro install --override-alias alpine-test alpine
proot-distro login alpine-test
```

复制的插件具有以下命名格式 `<name>.override.sh`，并存储在与其他插件相同的目录中（`$PREFIX/etc/proot-distro`）。

你可以通过环境变量 `PD_OVERRALL_TARBALL_URL` 指定自定义 URL 来下载 rootfs 存档，例如：
```
export PD_OVERRIDE_TARBALL_URL="http://localhost:8080/debian.tar.gz"
proot-distro install debian
```

也可以选择指定 `PD_OVERRIDE_TARBALL_STRIP_OPT` 来定义提取 tarball 时应剥离多少路径组件。如果未在发行版的插件脚本中定义，默认值为 1。如果 rootfs 未存储在子目录中，则指定为 0。

请注意，不检查自定义下载的 rootfs 的完整性。

可以强制指定要安装的发行版的自定义 CPU 架构。为此，你需要将 `DISTRO_ARCH` 环境变量设置为以下值之一：`aarch64`、`arm`、`i686`、`riscv64`、`x86_64`。示例：

```
DISTRO_ARCH=arm proot-distro install alpine
```

通常，如果你的主机是 64 位，同一架构的 32 位发行版应该可以无缝工作，但这不能保证。因此，如果你在 AArch64 主机上使用 ARM 版本系统时遇到问题，这应该是 [proot](https://github.com/termux/proot) 工具的 bug 或与主机支持的 CPU 指令不兼容。

使用外来架构（如 AArch64 主机上的 x86_64 目标）总是需要 QEMU 用户模式软件包。

使用一条命令安装所有支持的 QEMU 用户模式软件包：

```
pkg install qemu-user-aarch64 qemu-user-arm qemu-user-i386 qemu-user-x86-64
```

`x86_64` 目标还支持 Blink 用户模式 CPU 模拟器（实验性）。使用详情请参阅[下文](#实验性-blink-模拟器支持)。

### 列出发行版

命令：`list`

别名：`li`、`ls`

显示可用发行版列表、它们的别名、安装状态和注释。

### 启动 shell 会话

命令：`login`

别名：`sh`

在给定发行版中执行 shell。例如：
```
proot-distro login debian
```

以指定用户在给定发行版中执行 shell：
```
proot-distro login --user admin debian
```

你也可以运行自定义命令：
```
proot-distro login debian -- /usr/local/bin/mycommand --sample-option1
```

参数 `--` 作为 `proot-distro login` 选项处理的终止符。它后面的所有参数都不会被视为 PRoot Distro 的选项。

Login 命令支持以下行为修改选项：
* `--user <username>`

  使用自定义登录用户而不是默认的 `root`。你需要通过 `useradd -U -m username` 创建用户，然后才能使用此选项。

* `--fix-low-ports`

  强制将低网络端口重定向到高端口号（2000 + 端口）。将此选项与需要低端口的软件一起使用，而这些端口没有真正的 root 权限是无法使用的。

  例如，此选项会将端口 80 重定向到类似 2080 的端口。

* `--isolated`

  不要在 proot 环境中挂载主机卷。如果给出此选项，以下挂载点将不可访问：

  * /apex（仅 Android 10+）
  * /data/dalvik-cache
  * /data/data/com.termux
  * /sdcard
  * /storage
  * /system
  * /vendor

  你将无法在 proot 环境中使用 Termux 实用程序。

* `--termux-home`

  将 Termux 主目录挂载为 proot 环境中的用户主目录。

  此选项优先于选项 `--isolated`。

* `--shared-tmp`

  与 proot 环境共享 Termux 临时目录。优先于选项 `--isolated`。

* `--bind path:path`

  创建自定义文件系统路径绑定。选项期望以下格式的参数：
  ```
  <host path>:<proot path>
  ```

  优先于选项 `--isolated`。

* `--no-link2symlink`

  禁用 PRoot link2symlink 扩展。这将禁用硬链接模拟。只有在 SELinux 禁用或处于 permissive 模式时才能使用此选项。

* `--no-sysvipc`

  禁用 PRoot System V IPC 模拟。如果遇到崩溃，请尝试此选项。

* `--no-kill-on-exit`

  当 shell 会话终止时不要终止进程。如果你有任何后台进程运行，通常会导致会话挂起。

* `--no-arch-warning`

  抑制关于 CPU 不支持 32 位指令的警告。

* `--kernel`

  自定义 `uname -r` 命令显示的 Linux 内核版本字符串。

* `--hostname`

  自定义系统主机名。

* `--work-dir`

  将工作目录设置为给定值。默认工作目录与用户主目录相同。

### 卸载发行版

命令：`remove`

别名：`rm`

此命令完全删除给定系统的安装。小心，因为它不会要求确认。删除的数据无法恢复。

使用示例：
```
proot-distro remove debian
```

### 重命名发行版

命令：`rename`

别名：`mv`

通过更改别名名称、重命名其插件和根文件系统目录来重命名发行版。如果要重命名默认发行版，将创建插件的副本。

使用示例：
```
proot-distro rename ubuntu ubuntu-test01
```

只能重命名已安装的发行版。

### 重新安装发行版

命令：`reset`

别名：-

删除指定发行版并重新安装。这是以下操作的快捷方式：
```
proot-distro remove <dist> && proot-distro install <dist>
```

使用示例：
```
proot-distro reset debian
```

与命令 `remove` 一样，删除的数据将无法恢复。小心。

### 从备份恢复

命令：`restore`

别名：-

从给定的 proot-distro 备份（tar 存档）恢复发行版。

恢复操作执行对存档中备份状态的完整回滚。小心，因为此命令会不可恢复地删除以前的数据。

压缩方式根据文件扩展名自动确定。管道数据在提供给 `proot-distro` 之前必须始终解压缩。

使用示例：
```
ssh example.com 'cat /backups/pd-debian-backup.tar.xz' | xz -d | proot-distro restore
proot-distro restore ./pd-debian-backup.tar.xz
```

### 复制文件到发行版或从发行版复制

命令：`copy`

别名：`cp`

从/向发行版复制给定的文件或目录。

使用示例：
```
proot-distro copy ./localfile.txt ubuntu:/home/user/targetfile.txt
```

如果传递了选项 `--move`，则可以移动文件而不是复制。

不支持 glob。源和目标路径只能指定一次。

### 清除下载缓存

命令：`clear-cache`

别名：`clear`、`cl`

这将删除所有缓存的根文件系统存档。

## 添加发行版

发行版通过包含元数据变量的插件脚本定义。最小的一个看起来像这样：
```.bash
DISTRO_NAME="Debian"
TARBALL_URL['aarch64']="https://github.com/termux/proot-distro/releases/download/v1.10.1/debian-aarch64-pd-v1.10.1.tar.xz"
TARBALL_SHA256['aarch64']="f34802fbb300b4d088a638c638683fd2bfc1c03f4b40fa4cb7d2113231401a21"
```

脚本存储在目录 `$PREFIX/etc/proot-distro` 中，应命名为 `<alias>.sh`，其中 `<alias>` 是引用发行版的期望名称。例如，Debian 插件通常命名为 `debian.sh`。

### 插件变量参考

`DISTRO_ARCH`：指定要安装的发行版的 CPU 架构变体。

通常这个变量是自动确定的，你不应该设置它。典型的用例是在 QEMU 模拟器（用户模式）下设置自定义架构来运行发行版。

支持的架构有：`aarch64`、`arm`、`i686`、`riscv64`、`x86_64`。

`DISTRO_NAME`：发行版的名称，类似于"Alpine Linux (3.14.1)"。

`DISTRO_TYPE`：发行版的类型，可以是以下之一：

  * `normal`：默认，用于常规发行版
  * `termux`：特殊，用于打包的 Termux 引导环境

`DISTRO_COMMENT`：当前发行版的注释。

通常不需要此变量。使用它来通知用户某些功能不工作或启动此发行版需要额外步骤。

`TARBALL_STRIP_OPT`：提取 rootfs 存档时应剥离多少前导路径组件。默认值为 1，因为所有默认 rootfs tarball 都将内容存储在子目录中。

`TARBALL_URL`：根文件系统 tarball URL 的 Bash 关联数组。

至少应该为你的 CPU 架构定义。有效架构名称与 `DISTRO_ARCH` 相同。应以正确的协议方案开头。
例如，`https://`、`file://`、`ftp://` 等，用于访问本地或远程文件。

`TARBALL_SHA256`：每个 rootfs 变体的 SHA-256 校验和的 Bash 关联数组。

必须为 `TARBALL_URL` 中的每个 tarball 集合定义。

### 运行额外的安装步骤

可以配置插件在安装发行版后执行指定命令。这是通过函数 `distro_setup` 完成的。

示例：
```.bash
distro_setup() {
	run_proot_cmd apt update
	run_proot_cmd apt upgrade -yq
}
```

`run_proot_cmd` 用于在 rootfs 内执行命令。

## 实验性 Blink 模拟器支持

如果用户指定的 `DISTRO_ARCH` 与当前设备架构不同，将使用 CPU 模拟模式。

默认 CPU 模拟后端是 QEMU 用户模式。但是对于 `x86_64` 目标架构，用户可以启用 Blink 模拟器。要将 Blink 作为模拟后端，用户需要设置一个环境变量：
```
export PROOT_DISTRO_X64_EMULATOR=BLINK
```

`PROOT_DISTRO_X64_EMULATOR` 仅接受 `QEMU` 或 `BLINK` 值。

使用此命令安装 Blink 模拟器软件包：
```
pkg install blink
```

模拟模式不保证稳定性。用户可能会观察到程序行为异常和崩溃。有些发行版可能可以工作，而有些则不行。由于模拟器开销，性能也会降低。

## PRoot 问题与 Chroot 的区别

虽然 PRoot 通常被称为用户空间 chroot 实现，但它在实现和工作功能上与 chroot 有很大不同。以下是你应该知道的最显著差异列表。

1. PRoot 由于非本地执行而速度慢且可能不稳定

   每个进程都通过 `ptrace()` 连接。这样做是为了能够转换文件路径（模拟 `chroot`）、伪造 root 用户身份并处理不支持的系统调用。

   这种对执行流程的侵入通常可以正常工作。然而，在某些情况下，用户可能会观察到"不可能"的错误，如崩溃或奇怪的程序行为，这些在本地 Linux 发行版设置（PC、Raspberry Pi）上无法重现。

   使用 gdb 或 strace 等调试工具可能会出现问题。

   **重要**：`proot-distro` 相比其他 proot 环境设置脚本可能会表现出更高的性能下降。原因是更多使用了目录和文件绑定。这不是一个 bug，也不计划"修复"。

2. PRoot 无法从运行中的进程分离。

   由于 PRoot 通过 `ptrace()` 控制运行中的进程，它无法从进程中分离。这意味着你不能启动守护进程（例如 sshd）然后关闭 PRoot 会话。你将不得不终止进程、等待其完成或让 proot 在会话关闭时立即终止它。

3. PRoot 不提升权限。

   Chroot 本身也不提升权限。只是 PRoot 被配置为劫持用户 ID，即让它显示为 `root`。所以实际上，你的用户名、ID 和权限保持不变，就像没有 PRoot 一样，但进行当前用户健全性检查的程序会认为你以 root 用户运行。

   特别是，伪造的 root 用户使得在 proot 环境中使用包管理器成为可能。

4. PRoot 不模拟权限分离。

   你的 root 和非 root 实际上是相同的。文件将显示为由你当前用户拥有，这意味着 root 和非 root 用户都将能够编辑你的 `proot` 发行版设置的文件。

   根据你的 PRoot Distro 使用场景，这可能是一个安全问题。

5. PRoot 不启用硬件和文件系统挂载访问。

   你将无法读取/写入设备，如内部和外部驱动器分区、USB 设备、Wi-Fi 和蓝牙适配器。

   使用 FUSE 挂载文件系统也不可能：Android OS 不像标准 Linux 发行版那样在 `/dev/fuse` 上设置全局可写权限。

6. Appimage、Flatpak 和 Snap 在 PRoot 下无法工作。

   自给自足的应用容器（如 Appimage、Flatpak 或 Snap）依赖于文件系统挂载功能、FUSE 和其他在没有真正 root 权限的情况下不可用的功能。

## 镜像

我不提供自己的 rootfs 存档托管服务器。请不要询问，除非你愿意支付托管费用。

所有文件都作为 GitHub releases 发布。后者意味着没有有效的方法来镜像所有可用的 rootfs tarball，因此不存在镜像，`proot-distro` 也没有内置的切换镜像方法。

如果你无法从 GitHub 下载 rootfs 存档，你有这些选择：

1. 尝试使用 GitHub 代理，例如 `https://gh-proxy.ygxz.in/`

2. 构建自己的 rootfs tarball 并使用自己的服务器托管。对于本地文件，你可以尝试指定格式为 `file:///path/to/file.tar.gz` 的 URL。

   这两种选择都需要更新存储在 `$PREFIX/etc/proot-distro` 下的文件中的链接和 sha-256 校验和。

## 捐赠

支持对于长期维护项目非常重要。我很感激任何金额的加密货币打赏：

* Bitcoin: `bc1qxuwtc0sfjt43n3sufck6s0gaeand8eaeguajxs`
* Ethereum: `0x1F5196A5b0120D4a66FCAABBe71728239B06EC12`
* Tron: `TEJiwRMMGV1JXvRYDRVJ1qw7kFgskEk3sJ`

未来将添加更方便的选项。

*资金将流向 [@sylirre](https://github.com/sylirre)，PRoot-Distro 项目的作者和主要维护者。*

## 分叉

如果你希望将 PRoot Distro 或其部分作为自己项目的基础，请确保遵守 GNU GPL v3.0 许可证。

分叉必须以不同的名称分发。

[Termux]: <https://termux.com>
[proot]: <https://github.com/termux/proot>
