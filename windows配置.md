# Windows下配置c++环境

<https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win64/Personal%20Builds/mingw-builds/8.1.0/threads-posix/seh/x86_64-8.1.0-release-posix-seh-rt_v6-rev0.7z/download>

## cmake 出现以下问题

>*CMake was unable to find a build program corresponding to "Unix Makefiles".  CMAKE_MAKE_PROGRAM is not set.  You probably need to select a different build tool.*

意思就是不能生成Unix Makefiles，这是缺少make程序造成的，
解决方法就是找到mingw安装目录下mingw32-make.exe拷贝一份并重命名为make.exe

编译通过

## vscode 调试文件launch.json报错

![avatar](H:\study\studying\pic\launch_error.PNG)

解决办法：在launch.json添加miDebuggerPath gdb在本地的路径

```bash
    "miDebuggerPath": "E:\\configuration\\mingw64\\bin\\gdb.exe",
```

## git SSH配置

```bash
git config --global user.name [your name]
git config --global user.email [your email]
ssh-keygen -t rsa -C "your_email@example.com" //三次回车，不要输入任何东西
eval $(ssh-agent -s)
ssh-add /c/Users/chenjs/.ssh/id_rsa  //第三条指令中显示
clip < /c/Users/chenjs/.ssh/id_rsa.pub      //复制公钥，打开网页github，添加到ssh
ssh -T git@github.com                       //测试，若出现自己的用户名，以及successfull，则设置成功
```
