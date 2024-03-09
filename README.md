# Don-t-Starve-Together-Server
## 清除烧录系统硬盘分区受到保护解决方法：  

1.使用管理员进入CMD

2.在C:\Windows\system32下输入【diskpart】，进入DiskPart  

3.输入【list disk】，显示磁盘信息

```list disk```

4.以上为例，输入【select disk=0】，选中磁盘0。

```select disk=0```

5.成功选中后输入【list partition】,显示分区信息。  


6.同上，例如使用【select partition=3】选择分区3  



则当前焦点为【磁盘0的分区3】571MB的恢复分区，删除全请确认。  

7.删除分区使用【delete partition】命令，恢复分区需要 强制保护参数【override】命令如下  

```delete partition override```
