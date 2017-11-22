# 在arch以及衍生版本上运行TIM

   把deepin打包的TIM容器移植到arch

感谢:
* @[wszqkzqk](https://github.com/wszqkzqk) 的 [wszqkzqk-deepin-wine-tim-arch](https://github.com/wszqkzqk/wszqkzqk-deepin-wine-tim-arch)

* @[ssfdust](https://github.com/ssfdust) 的 [wszqkzqk-deepin-wine-tim-arch](https://github.com/ssfdust/wszqkzqk-deepin-wine-tim-arch)

存在如下bug：
  * 1.无法视频通话
  * 2.无法记住密码

## 安装
* 1.已添加到AUR [deepin-wine-tim](https://aur.archlinux.org/packages/deepin-wine-tim/)，可直接安装:
```shell
yaourt deepin-wine-tim
```

* 2.手动安装

```shell
 git clone https://github.com/countstarlight/deepin-wine-tim-arch.git

 cd deepin-wine-tim-arch
  
 makepkg -si
```

* 运行开始菜单中创建的TIM，点击立即安装
* 安装完可直接启动，以后启动时无需安装

## 更新日志
2017-11-21 TIM-2.0.0

使用文泉驿微米黑(wqy-microhei)字体，如果不喜欢这个字体，可以自行将微软雅黑或者微软宋体放进～/.deepinwine/Deepin-TIM/drive_c/windows/Fonts中。
