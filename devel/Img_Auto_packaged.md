

# Img Auto package

参考建议：https://blog.csdn.net/qq_16054639/article/details/125050453

## 一、Download img

```bash
sudo apt-get install -y qemu qemu-user-static binfmt-support kpartx 
wget https://releases.ubuntu-mate.org/jammy/arm64/ubuntu-mate-22.04-desktop-arm64+raspi.img.xz #/ubuntu mate 22.04
```

- unzip img.xz to img

```bash
unxz  ubuntu-mate-22.04-desktop-arm64+raspi.img.xz
```

```bash
联网不好用，需要传递文件请直接本机copy
安装img文件并挂载使用
kpartx -av ubuntu-mate-22.04-desktop-arm64+raspi.img

mount /dev/mapper/loop... /mnt/文件

cp /usr/bin/qemu-aarch64-static /mnt/usr/bin

sudo apt install -y systemd-container
sudo systemd-nspawn -D /mnt/文件

登录后
su 用户名


卸载img文件
umount /dev/mapper/loop... 
sudo kpartx -d 文件.img
```

```
sudo docker run -it  --name test2 --privileged=true -v /home/yjx/image_dir:/root 680e5dfb52c7 /bin/bash
```

