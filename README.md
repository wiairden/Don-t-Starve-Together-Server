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
