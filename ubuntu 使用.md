# ubuntu 使用

开机启动某些命令

~~**修改开机启动文件：/etc/rc.local（或者/etc/rc.d/rc.local）**~~

实测ubuntu 21.04 未找到

**自己写一个shell脚本**

将写好的脚本（.sh文件）放到目录 /etc/profile.d/ 下，系统启动后就会自动执行该目录下的所有shell脚本。

```bash
cd /etc/profile.d/
touch **.sh
sudo nano **.sh		#在sh文件中写入要执行的命令即可
```







