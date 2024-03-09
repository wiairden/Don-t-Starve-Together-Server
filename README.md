# Don-t-Starve-Together-Server
## 清除烧录系统硬盘分区受到保护解决方法：  

1.使用管理员进入CMD

2.在C:\Windows\system32下输入```diskpart```，进入DiskPart  

3.输入```list disk```，显示磁盘信息

4.以上为例，输入```select disk=1```，选中磁盘1。

5.成功选中后输入```list partition```,显示分区信息。  

6.同上，例如使用```select partition=1```选择分区1  

则当前焦点为【磁盘1的分区1】571MB的恢复分区，删除全请确认。  

7.删除分区使用```delete partition```命令，恢复分区需要 强制保护参数【override】命令如下  

```delete partition override```
# 使用的系统为ubuntu  
## 常用命令记录:  
1. SSH 服务将会被自动启动。你可以验证 SSH 是否正在运行，输入：  

```
sudo systemctl status ssh
```

输出将会告诉你服务正在运行，并且启用开机启动  

2. 查看ip地址:
```
ip add
```
3. ss
## 打开ubuntu的ssh和sftp:
1. 打开ssh指令:
```
sudo apt update
sudo apt install openssh-server
```
2. 打开sftp:
执行如下命令，创建SFTP用户组，方便进行管理测试。  

>提示：也可以不进行创建，直接使用root用户。  

```
sudo addgroup sftp-test
```  

依次执行如下命令，创建SFTP用户，并配置相应权限。  

```
sudo adduser alice
sudo usermod -G sftp-test -s /bin/false alice
```

依次执行如下命令，创建SSH用户组，并把管理员加入到该组。  

```
sudo addgroup ssh-users
sudo usermod -a -G ssh-users admin
```
注：-a参数的意思是不从其他用户组用移除用户。
执行如下命令，编辑SSH配置文件。  
```
sudo vim /etc/ssh/sshd_config
```
在sshd_config文件的中，添加如下内容。  
```
AllowGroups ssh-users sftp-users
Match Group sftp-users
   ChrootDirectory /home/sftp_root
   AllowTcpForwarding no
   X11Forwarding no
    ForceCommand internal-sftp
```
## 将源更换为阿里云源:
Ubuntu采用apt作为软件安装工具，其镜像源列表记录在/etc/apt/source.list文件中。

首先将source.list复制为source.list.bak备份。
```
cp /etc/apt/source.list /etc/apt/source.list.bak
```
修改完成后保存source.list文件，需要执行命令后才能生效：
```
sudo apt update
```
本文为 Ubuntu 22.04 的阿里云镜像源列表。若为其他版本，将所有jammy更改为其他版本代号即可。
常用的Ubuntu版本代号如下：
Ubuntu 22.04：jammy
Ubuntu 20.04：focal
Ubuntu 18.04：bionic
Ubuntu 16.04：xenia
```
vim /etc/apt/source.list
```

将文件内容清空，然后复制下方代码粘贴，wq保存退出即可。  

```
deb https://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse
deb-src https://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse

deb https://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse
deb-src https://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse

deb https://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse
deb-src https://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse

# deb https://mirrors.aliyun.com/ubuntu/ jammy-proposed main restricted universe multiverse
# deb-src https://mirrors.aliyun.com/ubuntu/ jammy-proposed main restricted universe multiverse

deb https://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse
deb-src https://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse
```
# 在Ubuntu上设置DST服务器
## 1. 安装依赖：  
安装服务器所需steam和饥荒的依赖  

用有sudo权限的用户登录服务器，一般直接用root用户就行  
```
sudo add-apt-repository multiverse
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install libstdc++6 libgcc1 libcurl4-gnutls-dev:i386 lib32z1
```
第一行指令如果提示'add-apt-repository command not found'通过如下方式解决：
```
sudo apt update
sudo apt install software-properties-common
sudo apt update
```
注意：有些源里可能没有libcurl4-gnutls-dev，需要直接安装：
```
cd /usr/lib/
ln -s libcurl.so.4 libcurl-gnutls.so.4
```

如果一会启动饥荒服务器时报错：  


> 'error while loading shared libraries: libcurl-gnutls.so.4: cannot open shared object file: No such file or directory'

说明依赖项没有装好，最好把上一步也做了防止出现该问题  

## 2. 安装 steamcmd：
创建steamcmd文件夹
```
mkdir ~/steamcmd
cd ~/steamcmd
```
下载、解压并启动steamcmd  
```
wget https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz
tar -xvzf steamcmd_linux.tar.gz
./steamcmd.sh
```
输入左侧出现“steam>”说明steamcmd启动成功  
接着，安装饥荒联机版
```
force_install_dir ../dontstarvetogether_dedicated_server
login anonymous
app_update 343050 validate
quit
````
'app_update 343050 validate'是去steam中下载饥荒联机服务器的命令，需要等待一会，当执行完成后输入'quit'退出steam服务
## 3.创建服务器存档
这里需要使用到FileZilla工具，简单理解为一个系统间服务器传输文件的工具，到官网下载，网上也有使用教程  

创建游戏->创建世界，具体想设置什么自己调一下，洞穴记得勾选，模组也根据自己需求选上，创建好世界开始后加载到如下选人物页面就完成了 

在服务器中创建存档的文件夹
```
mkdir -p ~/.klei/DoNotStarveTogether/Cluster_1
```

在本地电脑中的C://User/xxx/文档/klei/DoNotStarveTogether文件夹下，找到你刚才创建的饥荒存档，上传到服务器的/.klei/DoNotStarveTogether/Cluster_1这个文件夹里面（上面创建的），具体操作一会会用FileZilla实现  

回到饥荒联机版主页面，获取服务器token  

依次选择'账号'->'游戏'  

进入  

复制令牌，记得是全部  

在本地电脑，还是刚刚游戏存档的位置，创建cluster_token.txt文件，记事本打开，把刚刚复制的令牌粘贴进去 

该目录下创建cluster_token.txt文件  

保存  

复制自己的Klei用户ID，一会用来创建管理员权限  

复制灰色的ID  

设置管理员权限  

设置服务器密码、人数等  

自行设置

把设置好的存档传入服务器  

使用FileZilla  

连接服务器  

输入密码，密码是你在服务器给root设置过的密码  

输入密码，提示什么都点确定  

连接成功  

设置启动脚本  

编写服务器中启动饥荒服务器的脚本  

根目录下，创建并设置脚本文件  
```
cd ~
vim boot.sh
```
把如下代码粘贴到刚创建的boot.sh
```
#!/bin/bash

steamcmd_dir="$HOME/steamcmd"
install_dir="$HOME/dontstarvetogether_dedicated_server"
cluster_name="Cluster_1"
dontstarve_dir="$HOME/.klei/DoNotStarveTogether"

function fail() {
    echo Error: "$@" >&2
    exit 1
}

function check_for_file() {
    if [ ! -e "$1" ]; then
        fail "Missing file: $1"
    fi
}

cd "$steamcmd_dir" || fail "Missing $steamcmd_dir directory!"
check_for_file "steamcmd.sh"
check_for_file "$dontstarve_dir/$cluster_name/cluster.ini"
check_for_file "$dontstarve_dir/$cluster_name/cluster_token.txt"
check_for_file "$dontstarve_dir/$cluster_name/Master/server.ini"
check_for_file "$dontstarve_dir/$cluster_name/Caves/server.ini"
check_for_file "$install_dir/bin"
cd "$install_dir/bin64" || fail
run_shared=(./dontstarve_dedicated_server_nullrenderer_x64)
run_shared+=(-console)
run_shared+=(-cluster "$cluster_name")
run_shared+=(-monitor_parent_process $$)
run_shared+=(-shard)
"${run_shared[@]}" Caves | sed 's/^/Caves: /' &
"${run_shared[@]}" Master | sed 's/^/Master: /'
```

注意：代码粘贴完毕后按“：”，然后输入“wq”，回车，便可保存并退出

该设置中的启动方式为64位服务器，想启动32位的如下修改

修改这两行
如下：
```
cd "$install_dir/bin" || fail
run_shared=(./dontstarve_dedicated_server_nullrenderer)
```
设置boot.sh权限

sudo chmod u+x boot.sh
饥荒服务器还剩下最后模组mods部分，下面单独放一个模块说明 
