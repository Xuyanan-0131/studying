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