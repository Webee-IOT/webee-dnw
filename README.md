This code is based on changbin <changbin.du@gmail.com>
`git clone https://github.com/Qunero/dnw4linux.git`
Tested by izobs <ivincentlin@gmail.com>

#说明

1.设备的ID

网蜂的`webee210`的 idVendor 是 04e8,idproductis 是1234。确认你开发板也是那个信息，可以通过:           
    `sudo lsusb`                                        
而webee210打印的是下面的信息:             
    `ID 04e8:1234 Samsung Electronics Co.,Ltd`     
`idVendor` 和 `idproductis` 在 dnw.rules 文件夹下面，可以根据自己平台修改                             

2.linux 的dnw驱动

 `./src/driver/` 目录下有一个dnw 的usb 驱动，在ubuntu系统是有这个dnw的usb驱动支持的，但还是请安装这个驱动，因为这个可以减少错误.
 `./src/dnw/dnw.c` 使用的usb设备路径是 `/dev/secbulk0`.所以请用`sudo lsusb` 确认一下。

#使用

1. 编译安装

	$ makess
	$ sudo make install

2. dnw 工具的使用 

连接你的开发板并打开`minicom`. 通过下面可以下载到指定的地址:
	$ dnw <download address>


