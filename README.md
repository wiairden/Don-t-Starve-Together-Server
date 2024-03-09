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

