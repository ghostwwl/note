### linux 对SSD硬盘优化的方法


- 修改 /etc/fstab  

通过 ls -alht /dev/disk/by-uuid/  查看 uuid呢

```
1、关闭日志功能

方法一：

fstab里加挂载参数data=writeback，如：

原始的UUID=2f6be0cf-2f54-4646-b8c6-5fb0aa01ef23 / ext4 relatime,errors=remount-ro 0 1

改后的UUID=2f6be0cf-2f54-4646-b8c6-5fb0aa01ef23 / ext4 relatime,errors=remount-ro,data=writeback 0 1

 

方法二：

还有直接去掉日志功能的方法，这样是不用重新分区的：

运行指令:tune2fs -O ^has_journal  /dev/sda1

自己把/dev/sda1改为相应的分区号，要卸载分区才能运行该指令。

 

2、禁止记录最近一次访问的时间戳

默认情况下，Linux ext 文件系统在文件被访问、创建、修改等的时候记录下了文件的一些时间戳，比如：文件创建时间、最近一次修改时间和最近一次访问时间。因为系统运行的时候要访 问大量文件，如果能减少一些动作（比如减少时间戳的记录次数等）将会显著提高磁盘 IO 的效率、提升文件系统的性能。Linux 提供了 noatime 这个参数来禁止记录最近一次访问时间戳。

给文件系统挂载的时候加上 noatime 参数能大幅提高文件系统性能：

原始的UUID=2f6be0cf-2f54-4646-b8c6-5fb0aa01ef23 / ext4 defaults,errors=remount-ro 0 1

改后的UUID=2f6be0cf-2f54-4646-b8c6-5fb0aa01ef23 / ext4 defaults,noatime,errors=remount-ro 0 1

 

3、启用 TRIM 功能

Trim的作用

原本在机械硬盘上，写入数据时，Windows会通知硬盘先将以前的擦除，再将新的数据写入到磁盘中。而在删除数据时，Windows只会在此处做 个标记，说明这里应该是没有东西了，等到真正要写入数据时再来真正删除，并且做标记这个动作会保留在磁盘缓存中，等到磁盘空闲时再执行。

这样一来，磁盘需要更多的时间来执行以上操作，速度当然会慢下来。

而当Windows识别到SSD并确认SSD支持Trim后，在删除数据时，会不向硬盘通知删除指令，只使用Volume Bitmap来记住这里的数据已经删除。Volume Bitmap只是一个磁盘快照，其建立速度比直接读写硬盘去标记删除区域要快得多。这一步就已经省下一大笔时间了。然后再是写入数据的时候，由于NAND 闪存保存数据是纯粹的数字形式，因此可以直接根据Volume Bitmap的情况，向快照中已删除的区块写入新的数据，而不用花时间去擦除原本的数据。

以上就是Trim的原理以及真正作用。

注意：如果SSD组RAID0后，将失去Trim功能

 

Linux内核从2.6.33开始提供TRIM支持，所以先运行“uname -a”命令，查看自己的内核版本，如果内核版本低于2.6.33的，请先升级内核。

运行: hdparm -I /dev/sdb|grep TRIM 看是你的ssd否支持TRIM, 一般都支持, 但是我们还是确认下

都支持 那么你就可以在fstab中通过discard 来开启 TRIM 了
贴我的 根目录的 的呢
UUID=0c84d93c-8c02-418d-9437-fdaa4a7cccb4 /                       ext4    defaults,relatime,errors=remount-ro        0 1

```
