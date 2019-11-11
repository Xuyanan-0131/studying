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