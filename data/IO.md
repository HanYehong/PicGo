程序不能直接访问内核，需要通过系统调用



VFS虚拟文件系统：

FD 文件描述符：seek 偏移量，程序读取pagecache的时候有各自的指针和偏移量

inode

pagecache 4k 页缓存



两个程序打开同一个文件，不会打开两次，会检测VFS是否已经打开过这个文件，打开过则直接访问



dirty脏

程序与数据之间隔了一个内核，若过程中数据被某个程序修改了的话，该page页会被标记为脏

flush 将最新的数据刷新到磁盘中

1、程序调用内核的时候刷

2、内核每隔一段时间刷



df命令 挂载

![image-20210418215422841](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210418215422841.png)

mount 挂在

unmount 取消挂载



计算机组成：计算器、控制器、主存储器、输入输出设备（IO）

抽象：一切皆文件，摄像头可以是文件，打印机可以是文件，文件可以是文件……

如何区分文件：文件类型

-：普通文件，可执行，图片，文本……

d：目录

l：连接（ln 硬连接 软连接）

b：块设备（硬盘）

c：字符设备（键盘）

s：socket

p：pipeline

[eventpoll]：

![image-20210418220620952](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210418220620952.png)

windows的快捷方式就是软连接

losetup命令

mke2fs命令

ldd命令

docker 若干个虚拟空间共用一个内核

lsof 命令：进程打开了哪些文件

lsof -p $$

![image-20210418223222753](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210418223222753.png)

0 标准输入

1 标准输出

2 报错输出

Type：哪一类

exec 8< ooxx.txt 打开一个文件描述符8 指向ooxx.txt

cd /proc/$$/fd 可以看见当前进程打开了哪些文件描述符

lsof -op $$ 查看文件描述符的偏移量（$$是当前程序的PID）

![image-20210424213157604](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424213157604.png)

stat 命令

![image-20210424213242499](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424213242499.png)

read a 0<& 8 读取8赋值给a（read读到换行符就不读了）

echo $a

偏移量变成4了

![image-20210424213353356](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424213353356.png)

多个程序对同一个文件读取的话是多个不同的fd，互不影响

![image-20210424213629967](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424213629967.png)

swap 内存与磁盘进行交换

pcstat -pid $$

cat /proc/vmstat | grep dirty 查看脏页

![image-20210424214312056](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424214312056.png)

![image-20210424214345567](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424214345567.png)

sysctl -a | grep dirty 查看脏页刷新配置

![image-20210424214505242](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424214505242.png)

![image-20210424214642682](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424214642682.png)

![image-20210424214707904](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424214707904.png)

/proc文件夹下是进程管理

![image-20210424215130255](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424215130255.png)

$BASHPID 当前进程号

重定向：不是命令，是机制

![image-20210424215415343](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424215415343.png)

1> 标准输出到了 ls.out文件里去了

cat 读取内容

![image-20210424215820049](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424215820049.png)

可以单独重定向错误输出

将报错输出重定向到标准输出，再将标准输出重定向到文件，依然报错

![](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424220005061.png)

应该换个位置，先把1指向文件，再把2指向1，最终2才是指向文件的

![image-20210424220130401](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424220130401.png)

head显示文件头前十行

tail显示文件末尾十行

head -2 tail - 2 显示头/尾第二行

如何读取第八行？使用管道，先读出前8行，再保留最后一行

![image-20210424220407391](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424220407391.png)

管道|：前面的输出是后面的输入

pstree命令：显示进程树

![image-20210424220608760](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424220608760.png)

子进程父进程

![image-20210424220648824](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424220648824.png)

进程数据隔离，父进程的数据子进程是读不到的，除非使用export

![image-20210424221013270](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424221013270.png)

这也是环境变量需要用export进行修饰的原因

{} 代码块

解释执行，管道左右两边都是子进程，所以在子进程中将a改为9是对父进程没有影响的

![image-20210424221251467](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424221251467.png)

echo $$ | cat 显示的是父进程的PID

echo $BASHPID | cat 显示的是子进程的PID

因为$$的优先级高于管道

![image-20210424221452121](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424221452121.png)

![image-20210424221618544](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424221618544.png)

![image-20210424221630686](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424221630686.png)

![image-20210424221734740](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424221734740.png)

1指向了0，通过管道连接起来了

![image-20210424221853128](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424221853128.png)

![image-20210424221918231](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424221918231.png)

int 0x80中断

保护现场，切换用户态内核态

内核缓冲区

app缓冲区

硬盘缓冲区

DMA协处理器

![image-20210424222605811](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424222605811.png)

![image-20210424222659015](https://oss-hanyehong.oss-cn-hangzhou.aliyuncs.com/typoraImgs/image-20210424222659015.png)

关闭客户机会刷新脏页

