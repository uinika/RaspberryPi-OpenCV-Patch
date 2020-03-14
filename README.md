# raspberry-pi-opencv-patch

Missing `.i` & `.h` file during installation of opencv on Raspberry Pi 4B.

![](./logo.png)

由于 Raspberry Pi 4B 上编译`opencv 3.4.9`以及`opencv_contrib-3.4.9`的依赖过程当中，由于国内网络屏蔽造成的部分`.i`文件无法正常下载，而在执行`sudo make install`命令时出现`~/opencv_contrib/modules/xfeatures2d/src/boostdesc.cpp:673:20: fatal error: boostdesc_bgm.i: No such file or directory`等一系列错误信息。

## Problem

通过打开`/home/pi/Downloads/opencv/build`目录（即源码编译后的二进制文件所在目录）下的`CMakeDownloadLog.txt`日志文件，搜索如下缺失的`.i`文件名称：

```
boostdesc_bgm.i
boostdesc_lbgm.i
boostdesc_bgm_bi.i
boostdesc_bgm_hd.i

boostdesc_binboost_064.i
boostdesc_binboost_128.i
boostdesc_binboost_256.i

vgg_generated_48.i
vgg_generated_64.i
vgg_generated_80.i
vgg_generated_120.i
```

## Solution

找到对应的下载地址，完成下载后将其拷贝至`/home/pi/Downloads/opencv_contrib-3.4.9/modules/xfeatures2d/src`目录下面，重新执行`sudo make install`即可解决报错问题。

> **注意**：上述`.i`依赖文件已经全部放入本项目根目录下，适用于`opencv 3.4.9`和`opencv_contrib-3.4.9`版本的编译。

## Appendix

对于在执行`sudo make install`命令过程当中，出现的无法找到`cuda.hpp`、`xfeatures2d.hpp`、`nonfree.hpp`头文件的情况，只需要用文件查找命令，查询到该文件的所在位置：

```
find -name "xxx.hpp"
```

然后将头文件所在的`/home/...`路径名，添加至报错的`.cpp`源代码文件的`#include`预包含指令里面，即可以解决问题。
