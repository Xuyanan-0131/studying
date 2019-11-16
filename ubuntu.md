# Ubuntu command
## 1.安装 ××.deb
```
sudo dpkg -i **.deb
```

如果安装失败，输入：
```
sudo apt-get update
sudo apt-get -f install
```

然后再输入

```
sudo dpkg -i **.deb
```
----
## 2.网络配置
```
sudo gedit /etc/network/interfaces
```
打开interfaces的gedit文件，编辑：

>auto lo  
>iface lo inet loopback  
>#auto enp0s25  
>#iface enp0s25 inet static  
>#address 192.168.2.120

```
sudo /etc/init.d/networking restart
```

如果是动态转静态不用关机重启
如果是静态到静态需要重启

----

## 3.文件与文件夹操作
```
rm [-dfirv][--help][--version][文件或目录...] //执行rm指令可删除文件或目录，如欲删除目录必须加上参数”-r”，否则预设仅会删除文件。 
-d或–directory 　    //直接把欲删除的目录的硬连接数据删成0，删除该目录。 
-f或–force 　        //强制删除文件或目录。 
-i或–interactive 　  //删除既有文件或目录之前先询问用户。 
-r或-R或–recursive 　//递归处理，将指定目录下的所有文件及子目录一并处理。 
-v或–verbose 　      //显示指令执行过程。
rm -rf [dir]        //将会删除code目录以及其下所有文件、文件夹。（注意一定要加 -r，不然很麻烦）
rm -f xxx           //删除文件
sudo mkdir [文件名]  //创建文件
```

## 4.QT5无法输入中文
### 4.1 安装过程
本人系统为ubuntu16.04LTS，Qt版本为5.8.0
```
sudo apt-get install cmake          //安装cmake，已安装则略过
sudo apt-get install fcitx-libs-dev //安装fcitx-lib-dev
```

设置qmake的环境变量:
```
export PATH="/home/xu/Qt5.8.0/5.8/gcc_64/bin":$PATH //安装目录
```

下载fcitx-libs 源码
```
git clone https://github.com/fcitx/fcitx-qt5
cmake .
make
sudo make install
```

编译完成后，需要把编译得到的 platforminputcontext/libfcitxplatforminputcontextplugin.so 拷贝到 /home/xu/Qt5.8.0/Tools/QtCreator/lib/Qt/plugins/platforminputcontexts

完成

### 4.2 安装可能会遇到的问题
#### 4.2.1 cmake fcitx错误
```
CMake Error at CMakeLists.txt:8 (find_package):
Could not find a package configuration file provided by "ECM" (requested
version 1.4.0) with any of the following names:
ECMConfig.cmake
ecm-config.cmake
Add the installation prefix of "ECM" to CMAKE_PREFIX_PATH or set "ECM_DIR"
to a directory containing one of the above files. If "ECM" provides a
separate development package or SDK, be sure it has been installed.
— Configuring incomplete, errors occurred!
```
解决办法：
到这个页面 https://launchpad.net/ubuntu/+source/extra-cmake-modules/1.4.0-0ubuntu1 下载 extra-cmake-modules_1.4.0.orig.tar.xz，解压
```
cd extra-cmake-modules-1.4.0
cmake .
make
sudo make install
```

#### 4.2.2安装 extra-cmake-modules-1.4.0 失败
```
CMake Warning at tests/CMakeLists.txt:28 (find_package):
Could not find a package configuration file provided by "Qt5LinguistTools"
with any of the following names:
Qt5LinguistToolsConfig.cmake
qt5linguisttools-config.cmake
Add the installation prefix of "Qt5LinguistTools" to CMAKE_PREFIX_PATH or
set "Qt5LinguistTools_DIR" to a directory containing one of the above
files. If "Qt5LinguistTools" provides a separate development package or
SDK, be sure it has been installed.
— Looking for Sphinx Documentation Builder…
— Sphinx Documentation Builder not found – documentation will not be built (see http://sphinx-doc.org/)
— Configuring done
— Generating done
— Build files have been written to: /home/cposture/Downloads/extra-cmake-modules-1.4.0
```
解决方法：  
设置 CMAKE_PREFIX_PATH 环境变量 为 qtbase 目录（<Qt安装目录>/5.6/Src/qtbase/），我这里为：
export CMAKE_PREFIX_PATH="~/Qt5.6.0/5.6/Src/qtbase/"
如果还是不行，则修改为
export CMAKE_PREFIX_PATH="～/Qt5.6.0/5.6/gcc_64/lib/cmake/"

___以上是我遇到的所有问题，若有其他问题，百度大多数都能解决___   
___参考： 
https://my.oschina.net/lieefu/blog/505363?fromerr=NNm21wBS____

# 5.关于qt上位机的deb文件打包

deb打包前文件结构如下
```
-package
    |-DEBIAN
        |-control
    |-usr
        |-lib
            |-各个依赖库
        |-src
            |-可执行文件
```
## 5.1 在qt下构建release版本的执行文件，执行打包的sh指令，将执行文件及其依赖库打包
```
#!/bin/bash

LibDir=$PWD"/lib"
Target="sensor"

lib_array=($(ldd $Target | grep -o "/.*" | grep -o "/.*/[^[:space:]]*"))

$(mkdir $LibDir)

for Variable in ${lib_array[@]}
do
    cp "$Variable" $LibDir
done
```
首先需要明白自己需要哪些依赖库，可以使用```ldd+执行文件名```查看运行程序所需的依赖库，但是不一定全面，有些库还需要自己手动添加，qt的依赖库都在Qt5.8.0/5.8/gcc_64/lib下，这些库文件一般都是要考到usr/lib下。
同时，还需要拷贝Qt5安装目录中plugins（Qt5.8.0/5.8/gcc_64/plugins）中的一些目录,比如platforms目录（平台所需）、xcbglintegrations目录（xcb所需），使它与Qt运行文件在同级目录。
## 5.2 开始打包deb包
```
mkdir package
cd package
```
### 5.2.1 因为安装软件包的时候默认是将文件释放到根目录下，所以需要设定好它安装的路径，同时还需要建立一个DEBIAN目录。
```
mkdir -p usr/src
mkdir -p usr/lib
mkdir DEBIAN
```
### 5.2.2 把需要打包的文件及其库文件拷贝到相应的目录
```
cp home/youname/sensor usr/src
cp home/youname/lib/* usr/lib
```
### 5.2.3 重点：在 DEBIAN目录下创建一个control文件，并加入以下内容，内容可自定义：
```
gedit DEBIAN/control
Package: sensor
Version: 1.0.1
Section: utils
Priority: optional
Architecture: i386
Depends:
Installed-Size: 512
Maintainer: xu@163.com
Description: sensor package
```
### 5.2.4 使用dpkg 命令构建deb包
```
sudo chmod 755 * -R
dpkg -b . /home/youname/sensor_v1.0.1.deb
```
打包完成

## 5.3 安装与卸载
安装
```
dpkg -i sensor_v1.0.1.deb
```
卸载
```
dpkg -r sensor
```

## 5.4 出现的问题

### 5.4.1 出现qt_5 not found 
解决办法：
```
export LD_LIBRARY_PATH=LD_LIBRARY_PATH:/usr/lib  //你的依赖库安装路径
```
也可以在etc/ld.so.conf.d/ 下创建**.config，输入依赖库路径
例如：
```
/usr/lib
```
### 5.4.2 xcb not found
```
cp -a libQt5DBus.so* libQt5XcbQpa.so.5* 目标路径/lib
```
## 5.5 可执行文件快捷桌面方式
所有的桌面快捷方式都存在与usr/share/applications
在此目录下创建.desktop
```
[Desktop Entry]
Name = Boschda Sensor Tool
Exec = /usr/src/sensor/sensor %U
Icon = /usr/src/sensor/sensor/bin/seahores.png
Path = /usr/src/sensor/bin
Terminal = false
Type = Application
Categories = Network;
```
最后sudo chmod +x **.desktop