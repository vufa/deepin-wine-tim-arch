在Archlinux及衍生发行版上运行TIM
=======

<p align="center">
  <a href="https://travis-ci.org/countstarlight/deepin-wine-tim-arch">
    <img src="https://travis-ci.org/countstarlight/deepin-wine-tim-arch.svg?branch=master" alt="Build Status">
  </a>
  <a href="https://office.qq.com/download.html">
    <img src="https://img.shields.io/badge/TIM-3.0.0.21315-blue.svg" alt="TIM Version">
  </a>
  <a href="https://aur.archlinux.org/packages/deepin-wine-tim/">
    <img src="https://img.shields.io/aur/version/deepin-wine-tim.svg" alt="AUR Version">
  </a>
  <a href="https://github.com/countstarlight/deepin-wine-tim-arch/releases">
    <img src="https://img.shields.io/github/downloads/countstarlight/deepin-wine-tim-arch/total.svg" alt="GitHub Release">
  </a>
  <a href="https://github.com/countstarlight/deepin-wine-tim-arch/issues">
    <img src="https://img.shields.io/github/issues/countstarlight/deepin-wine-tim-arch.svg" alt="GitHub Issues">
  </a>
</p>

Deepin 打包的 TIM 容器移植到 Archlinux，不依赖 `deepin-wine`，包含定制的注册表配置，TIM 安装包替换为官方最新

<!-- TOC -->

- [安装](#安装)
    - [从AUR安装](#从aur安装)
    - [用安装包安装](#用安装包安装)
    - [本地打包安装](#本地打包安装)
- [切换到 `deepin-wine`](#切换到-deepin-wine)
    - [自动切换(推荐)](#自动切换推荐)
    - [手动切换](#手动切换)
        - [1. 安装 `deepin-wine`](#1-安装-deepin-wine)
        - [2. 对于非 GNOME 桌面(KDE, XFCE等)](#2-对于非-gnome-桌面kde-xfce等)
        - [3. 删除已安装的TIM目录](#3-删除已安装的tim目录)
        - [4. 修复 `deepin-wine` 字体渲染发虚](#4-修复-deepin-wine-字体渲染发虚)
- [常见问题及解决](#常见问题及解决)
    - [不能记住密码](#不能记住密码)
    - [网络连接状态改变后不能重连](#网络连接状态改变后不能重连)
    - [高分辨率屏幕支持](#高分辨率屏幕支持)
    - [使用全局截图快捷键](#使用全局截图快捷键)
    - [使用其他字体](#使用其他字体)
- [感谢](#感谢)
- [更新日志](#更新日志)

<!-- /TOC -->

## 安装

`deepin-wine-tim` 依赖`Multilib`仓库中的 `wine`，`wine-gecko` 和 `wine-mono`，Archlinux 默认没有开启 `Multilib`仓库，需要编辑`/etc/pacman.conf`，取消对应行前面的注释([Archlinux wiki](https://wiki.archlinux.org/index.php/Official_repositories#multilib)):

```diff
# If you want to run 32 bit applications on your x86_64 system,
# enable the multilib repositories as required here.

#[multilib-testing]
#Include = /etc/pacman.d/mirrorlist

-#[multilib]
-#Include = /etc/pacman.d/mirrorlist
+[multilib]
+Include = /etc/pacman.d/mirrorlist
```

### 从AUR安装

已添加到 AUR [deepin-wine-tim](https://aur.archlinux.org/packages/deepin-wine-tim/)，使用 `yay` 安装（如未安装 `yay`，请先 `pacman -S yay` 进行安装）：

```shell
yay -S deepin-wine-tim
```

### 用安装包安装

> 由 [Travis CI](https://travis-ci.org/countstarlight/deepin-wine-tim-arch) 在 Docker 容器 [mikkeloscar/arch-travis](https://hub.docker.com/r/mikkeloscar/arch-travis) 中自动构建的 ArchLinux 安装包

在 [GitHub Release](https://github.com/countstarlight/deepin-wine-tim-arch/releases) 页面下载后缀为 `.pkg.tar.xz` 或 `.pkg.tar.zst` 的安装包，使用`pacman`安装：

```bash
sudo pacman -U #下载的包名
```

`.md5` 文件用于校验包完整性：

```bash
md5sum -c *.md5
```

### 本地打包安装

```shell
 git clone https://github.com/countstarlight/deepin-wine-tim-arch.git

 cd deepin-wine-tim-arch
  
 makepkg -si
```

* 运行应用菜单中创建的 TIM 快捷方式，开始安装 TIM

  **注意：安装TIM时不需要修改安装路径，如果修改默认路径，要对应修改 `deepin-wine-tim` 的启动脚本：**

  `/opt/deepinwine/apps/Deepin-TIM/run.sh`

  ```bash
  env WINEPREFIX="$WINEPREFIX" WINEDEBUG=-msvcrt $WINE_CMD "c:\\Program Files\\Tencent\\TIM\\Bin\\TIM.exe" &
  ```

  改为修改后的安装路径，否则只有安装后第一次能够运行

* 安装完可直接启动

## 切换到 `deepin-wine`

原版 `wine` 在 [DDE(Deepin Desktop Environment)](https://www.deepin.org/dde/) 上，有托盘图标无法响应鼠标事件([deepin-wine-tim-arch#21](https://github.com/countstarlight/deepin-wine-tim-arch/issues/21))的问题，且原版 `wine` 尚不能实现保存登录密码等功能，可以选择切换到 `deepin-wine`。

**注意：切换前先确保 `deepin-wine` 支持**

根据 [deepin-wine-wechat-arch#15](https://github.com/countstarlight/deepin-wine-wechat-arch/issues/15#issuecomment-515455845)，[deepin-wine-wechat-arch#27](https://github.com/countstarlight/deepin-wine-wechat-arch/issues/27)，由 [@feileb](https://github.com/feileb), [@violetbobo](https://github.com/violetbobo), [@HE7086](https://github.com/HE7086)提供的方法：

### 自动切换(推荐)

```bash
/opt/deepinwine/apps/Deepin-TIM/run.sh -d
```

这会安装需要的依赖，移除已安装的TIM目录并回退对注册表文件的修改

切换回 `wine`：

```bash
rm ~/.deepinwine/Deepin-TIM/deepin
```

如果要卸载自动安装的依赖：

```bash
sudo pacman -Rns deepin-wine xsettingsd lib32-freetype2-infinality-ultimate
```

### 手动切换

#### 1. 安装 `deepin-wine`

```bash
yay -S deepin-wine
```

#### 2. 对于非 GNOME 桌面(KDE, XFCE等)

需要安装 `xsettingsd`：

根据 [deepin-wine-wechat-arch#36](https://github.com/countstarlight/deepin-wine-wechat-arch/issues/36#issuecomment-612001200)，由[Face-Smile](https://github.com/Face-Smile)提供的方法：

```bash
sudo pacman -S xsettingsd
```

修改 `/opt/deepinwine/apps/Deepin-TIM/run.sh`：

```diff
-WINE_CMD="wine"
+WINE_CMD="deepin-wine"

 RunApp()
 {
+    if [[ -z "$(ps -e | grep -o xsettingsd)" ]]
+    then
+        /usr/bin/xsettingsd &
+    fi
        if [ -d "$WINEPREFIX" ]; then
                UpdateApp
        else
```

**注意：对 `/opt/deepinwine/apps/Deepin-TIM/run.sh` 的修改会在 `deepin-wine-tim` 更新或重装时被覆盖，可以单独拷贝一份作为启动脚本**

#### 3. 删除已安装的TIM目录

```bash
rm -rf ~/.deepinwine/Deepin-TIM
```

#### 4. 修复 `deepin-wine` 字体渲染发虚

kde桌面参考：[deepin-wine-wechat-arch#36](https://github.com/countstarlight/deepin-wine-wechat-arch/issues/36)

deepin 桌面：

```bash
yay -S lib32-freetype2-infinality-ultimate
```

**注意：切换到 `deepin-wine` 后，对 `wine` 的修改，如更改dpi，都改为对 `deepin-wine` 的修改**

## 常见问题及解决

### 不能记住密码

参照[切换到 `deepin-wine`](#切换到-deepin-wine) 解决

### 网络连接状态改变后不能重连

参照[切换到 `deepin-wine`](#切换到-deepin-wine) 解决

### 高分辨率屏幕支持

在 `winecfg` 的Graphics选项卡中修改dpi，如 修改为`192`

对于 `wine`：

```bash
env WINEPREFIX="$HOME/.deepinwine/Deepin-TIM" winecfg
```

对于 `deepin-wine` ：

```bash
env WINEPREFIX="$HOME/.deepinwine/Deepin-TIM" deepin-wine winecfg
```

### 使用全局截图快捷键

使用全局截图快捷键和解决Gnome上窗口化问题，参见[issue2](https://github.com/countstarlight/deepin-wine-tim-arch/issues/2)

### 使用其他字体

默认使用文泉驿微米黑(`wqy-microhei`)字体，可以使用Windows平台常用字体替代，直接将字体文件或字体链接文件放置到字体文件夹就会生效，不会影响系统字体

字体文件夹在：`$HOME/.deepinwine/Deepin-TIM/drive_c/windows/Fonts`

## 感谢

* [Wuhan Deepin Technology Co.,Ltd.](http://www.deepin.org/)

* [@wszqkzqk](https://github.com/wszqkzqk) 的 [wszqkzqk-deepin-wine-tim-arch](https://github.com/wszqkzqk/wszqkzqk-deepin-wine-tim-arch)

* [@ssfdust](https://github.com/ssfdust) 的 [wszqkzqk-deepin-wine-tim-arch](https://github.com/ssfdust/wszqkzqk-deepin-wine-tim-arch)

## 更新日志

* 2020-04-01 TIM-3.0.0.21315
* 2019-09-21 TIM-2.3.2.21173
* 2019-03-06 TIM-2.3.2.21158
* 2019-02-05 TIM-2.3.1_3
* 2018-02-23 TIM-2.1.5
* 2017-12-23 TIM-2.1.0
* 2017-11-28 修复音频功能（麦克风录音和播放语音消息）
* 2017-11-21 TIM-2.0.0
