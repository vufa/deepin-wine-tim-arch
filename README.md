# 在arch以及衍生版本上运行TIM

   把deepin打包的TIM容器移植到arch

* 存在如下bug：
  * 1.无法视频通话
  * 2.无法记住密码

## 安装

* 注意在安装前要先卸载`AUR`里的`wine-tim`软件包

```shell
 git clone https://github.com/countstarlight/deepin-wine-tim-arch.git

 cd deepin-wine-tim-arch
  
 makepkg -si
```

* 运行开始菜单中创建的TIM，点击立即安装
* 安装完可直接启动，以后启动时无需安装




修复了部分字符无法显示的BUG，但是由于版权问题，字体看起来效果不是特别好，如果看不惯字体的，可以自行将微软雅黑或者微软宋体放进～/.deepinwine/Deepin-TIM/drive_c/windows/Fonts中。
