This code is based on changbin <changbin.du@gmail.com>
`git clone https://github.com/Qunero/dnw4linux.git`

rewrite by izobs <ivincentlin@gmail.com>
==============================================================
##BEFORE COMPLIE
1.ID
=================================================
    before complie the code,please make sure your broad's idVendor number is 04e8,idproductis 1234.the meassage is writen in dnw.rules.
    Connect your broad to pc,type the following command :
    `sudo lsusb`
    if you are using webee210,you may see this:
    `ID 04e8:1234 Samsung Electronics Co.,Ltd`
    which mean you can complie the code now.if not,try to correct the idVendor and idproductis in dnw.rules
2.driver
===================================================
 the ./src/driver/ file include an usb driver,is an new driver of your broad.although your computer already has.Please install this one,because it can make the download task less erro.What's more,the ./src/dnw/dnw.c using the device named /dev/secbulk0.

##COMPLIE

1. build and install
=====================
	$ make
	$ sudo make install

2. dnw tool usage 
=====================
Connect board to PC and open minicom. Boot board and enter U-Boot command line mode.
Then run command "dnw <download address>" in U-Boot. U-Boot may print bellow message:
	Insert a OTG cable into the connector!
	OTG cable Connected!
	Now, Waiting for DNW to transmit data

Now, you can download your file to board by follow command on PC end:
	$ sudo dnw <file_to_download>

Note: If your board isn't FL Ok6410, please set right load-address via "a" option.
      Above steps have only download file to board's RAM, so you need  flash
      it to nand via U-Boot command "nand write".

If above doesn't work, pls check if you can see bellow message in `dmesg`.
	usb 1-1: new full speed USB device using uhci_hcd and address 2
	usb 1-1: configuration #1 chosen from 1 choice
	secbulk:secbulk probing...
	secbulk:bulk out endpoint found!

===============================================
some U-Boot commands special for FL Ok6410
   (1) download U-Boot
	$dnw 50008000
	$nand erase 0 100000
	$nand write.uboot 50008000 0 100000
	#dnw default load address is 0xc0000000
	all in one:
	$dnw 50008000 && nand erase 0 100000 && nand write.uboot 50008000 0 100000
   (2) download kernel
	$dnw 50008000
	$nand erase 100000 500000
	$nand write.e 50008000 100000 500000
	all in one:
	$dnw 50008000 && nand erase 100000 500000 && nand write.e 50008000 100000 500000
   (3) download yaffs2 root file system
	$dnw 50008000
	$nand erase 600000 #erase mtdblock2 partition
	$nand write.yaffs2 50008000 600000 8000000 #instead 8000000 of real image size
	all in one:
	$dnw 50008000 && nand erase 600000 && nand write.yaffs2 50008000 600000 8000000
   (4) download ubifs/cramfs root file system
	$dnw 50008000
	$nand erase 600000 #erase mtdblock2 partition
	$nand write.e 50008000 600000 8000000 #instead 8000000 of real image size
	all in one:
	$dnw 50008000 && nand erase 600000 && nand write.e 50008000 600000 8000000
   (5) download jffs2 root file system
	$dnw 50008000
	$nand erase 600000 #erase mtdblock2 partition
	$nand write.jffs2 50008000 600000 8000000 #instead 8000000 of real image size
	all in one:
	$dnw 50008000 && nand erase 600000 && nand write.jffs2 50008000 600000 8000000
   (6) deal with bad blocks
	$nand scrub
   (7) set kernel arguments
	$setenv bootargs "root=/dev/mtdblock2 rootfstype=yaffs2 console=ttySAC0,115200" 
	$save
	$reset
   (8) boot from NFS
	$setenv bootargs "root=/dev/nfs nfsroot=192.168.0.231:/FileSystem-Yaffs2 \
	 ip=192.168.0.232:192.168.0.231:192.168.0.201:255.255.255.0:8.8.8.8:eth0:off \
	 console=ttySAC0,115200"
	$save
	# "192.168.0.231" is your host ip; "192.168.0.232" is your board's ip;
	# "192.168.0.201" is gateway; "255.255.255.0" is mask.

