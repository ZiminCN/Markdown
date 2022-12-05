# Linux中systemctl详细解释及常用命令

Linux服务管理有两种方式**service**与**systemctl**



## Service

service是linux系统的初始化（init），能够提高系统的启动速度，注意尽可能启动较少的进程，尽可能使更多的进程启动。

systemctl对应的进程管理命令是systemctl

即systemctl也会去/etc/init.d目录下，查看，执行相关程序

    systemctl redis start
     
    systemctl redis stop
     
    # 开机自启动
     
    systemctl enable redis



## **Systemctl**

systemctl命令管理systemd的资源Unit一般在目录/etc/systemd/system，其中主要有四种类型文件.mount、.service、.target、.wants;



#### .mount

该文件定义了一个挂载点，其中配置了What、Where、Type三个数据项

等同于

```
mount [-参数] [挂载文件] [被挂载目录]
```

------



#### .service

该文件定义了一个服务，配置了Unit、Service、Install三个小节

三个小节如下：

> [Unit]
>
> Description:描述，
>
> After：在network.target,auditd.service启动后才启动
>
> ConditionPathExists: 执行条件
>
> [Service]
>
> EnvironmentFile:变量所在文件
>
> ExecStart: 执行启动脚本
>
> Restart: fail时重启
>
> [Install]
>
> Alias:服务别名
>
> WangtedBy: 多用户模式下需要的

例如：

> [Unit]
> Description="Diablo Daemon"
> After=network-online.target
> Wants=network-online.target
>
> [Service]
> Type=idle
> User=diablo
> Group=diablo
> #Environment="ROS_DOMAIN_ID=42"
> Environment="PATH=/opt/ros/foxy/bin"
> Environment="LD_LIBRARY_PATH=/opt/ros/foxy/lib:/home/diablo/mqtt_ws/install/lib"
> Environment="PYTHONPATH=/opt/ros/foxy/lib/python3.8/site-packages:/home/diablo/mqtt_ws/install/lib/python3.8/site-packages"
> Environment="AMENT_PREFIX_PATH=/opt/ros/foxy:/home/diablo/mqtt_ws/install"
> ExecStartPre=/bin/sleep 3
> ExecStart=/opt/ros/foxy/bin/ros2 launch learning_launch simple.launch.py
> Restart=on-failure
>
> [Install]
> WantedBy=multi-user.target

------



#### .target

文件定义了一些基础的组件，供.service文件调用

#### .wants

文件定义了要执行的文件集合，每次执行，.wants文件夹里面的文件都会执行





## 常用命令

命令格式：

```bash
systemctl [command] [unit]
```



其中command命令只要包括：

```
start:启动后面的unit
stop:停止后面的unit
restart:关闭后面的unit，随后再启动后面的unit
reload:不关闭unit的情况时，重新载入配置文件,使设置生效
enable:设置为下次开机时，后面的unit会被启动
disable:设置为下次开机时，后面的unit不会被启动
status:显示后面接的unit的状态，列出是否在执行、开机时是否启动等信息
is-active:检测后面的unit是否正在运行中
is-enable:检测后面的unit是否在开机时默认启动
kill:向后面的unit发送信号
show:列出unit的配置
mask:彻底注销后面的unit，注销后便无法再启动后面的unit
unmask:取消对后面unit的注释
```



查看系统上的unit

基本命令格式为

```bash
systemctl [command][--type=TYPE][-all]
```

其中command有

```
list-units:列出当前已经启动的unit,如果添加-all会同时列出没有启动的unit
list-unit-files:根据/lib/systemd/system/目录内的文件列出所有的unit
--type=TYPE:可以过滤某个类型的unit
不带任何参数执行systemctl命令会列出所有已经启动的unit
```

例如：

```
systemctl list-units （或者直接 sudo systemctl）
```



介绍输出参数：

```
UNIT：项目的名称，包括各个unit的类别（看拓展名）

LOAD：开机时unit的配置是否被加载

ACTIVE：指目前的状态

DESCRIPTION：描述信息
```

查看系统上一共装了多少个unit

```
systemctl list-unit-files
```

