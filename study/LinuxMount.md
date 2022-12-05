# Linux Mount挂载命令详解

命令格式：mount [-参数][设备名称][挂载点]

1. mount /home/nfs /mnt /home/nfs 文件系统的磁盘或分区相关的设备文件 /mnt 挂载到的目录

2. umount /home/nfs umount /mnt 取消挂载可以是设备文件或者挂载点

3. mount 命令的其他参数

-t 指定设备的文件系统类型

        常见的文件系统类型有：
    
        ext2         linux目前常用的文件系统
    
        msdos     MS-DOS的fat，就是fat16
    
        vfat          Windows98常用的fat32
    
        nfs           网络文件系统
    
        iso9660   CD-ROM光盘标准文件系统
    
        ntfs          Windows NT/2000/XP的文件系统
    
        auto         自动检测系统

-o 指定挂在文件系统时的选项 unlock 禁用nfs locking

-a 安装在/etc/fstab⽂件中列出的所有⽂件系统。

-f 伪装mount，作出检查设备和⽬录的样⼦，但并不真正挂载⽂件系统。

-n 不把安装记录在/etc/mtab⽂件中。

-r 将⽂件系统安装为只读。

-v 详细显⽰安装信息。

-w 将⽂件系统安装为可写，为命令默认情况

实例：

    //将172.18.1.240:/home/nvr这个目录
    //（而且该目录是网络文件系统下的目录）挂载到 本设备的/mnt上
    mount -t nfs -o nolock 172.18.1.240:/home/nvr /mnt