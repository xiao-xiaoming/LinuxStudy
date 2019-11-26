### 远程文件复制：scp 

scp 命令用于 Linux 之间复制文件和目录，scp是 secure copy 的缩写是linux系统下基于ssh登陆进行安全的远程文件拷贝命令。

scp 是加密的，[rcp](https://xiaoxiaoming.xyz/linux/linux-comm-rcp.html) 是不加密的，scp 是 rcp 的加强版。

使用scp命令要确保使用的用户具有可读取远程服务器相应文件的权限，否则scp命令是无法起作用的。

从本地复制到远程命令格式：

```
复制文件
scp local_file remote_username@remote_ip:remote_folder 
或者 
scp local_file remote_username@remote_ip:remote_file 
或者 
scp local_file remote_ip:remote_folder 
或者 
scp local_file remote_ip:remote_file

复制文件夹
scp -r local_folder remote_username@remote_ip:remote_folder 
或者 
scp -r local_folder remote_ip:remote_folder 
```

实例：

```
scp /home/space/music/1.mp3 root@xiaoxiaoming.xyz:/home/root/others/music 
scp /home/space/music/1.mp3 root@xiaoxiaoming.xyz:/home/root/others/music/001.mp3 
scp /home/space/music/1.mp3 xiaoxiaoming.xyz:/home/root/others/music 
scp /home/space/music/1.mp3 xiaoxiaoming.xyz:/home/root/others/music/001.mp3

scp -r /home/space/music/ root@xiaoxiaoming.xyz:/home/root/others/ 
scp -r /home/space/music/ xiaoxiaoming.xyz:/home/root/others/ 
```

从远程复制到本地：

```
scp root@xiaoxiaoming.xyz:/home/root/others/music /home/space/music/1.mp3 
scp -r xiaoxiaoming.xyz:/home/root/others/ /home/space/music/
```

-P 参数来设置命令的端口号：

```
#scp 命令使用端口号 4588
scp -P 4588 remote@xiaoxiaoming.xyz:/usr/local/sin.sh /home/administrator
```

### locate查找

locate命令会去保存文档和目录名称的数据库内，查找文件或目录。

一般情况我们只需要输入`locate your_file_name` 即可查找指定文件。

**参数：**

- -d或--database= 配置locate指令使用的数据库。locate指令预设的数据库位于/var/lib/mlocate目录里，文档名为mlocate.db。

查找passwd文件，输入以下命令：

```
locate passwd
```

locate与find的区别: find 是去硬盘找，locate 只在/var/lib/slocate资料库中找。

locate的速度比find快，它并不是真的查找，而是查数据库，一般文件数据库在/var/lib/mlocate/mlocate.db中，所以locate的查找并不是实时的，而是以数据库的更新为准，一般是系统自己维护，也可以手工升级数据库 ，命令为：

```
updatedb
```

### which命令

which查找$PATH中设置命令及安装文件目录所在位置

```
python@ubuntu:/var/lib/mlocate$ which locate
/usr/bin/locate
```

### echo

常见用法：

```
python@ubuntu:~$ echo -e "hello\t\t world！"  解析转义字符
hello            world！
python@ubuntu:~$ echo -E "hello\t\t world！"  不解析转义字符
hello\t\t world！
python@ubuntu:~$ echo $a  输出环境变量
b
```

### 设置或显示环境变量：export

在 shell 中执行程序时，shell 会提供一组环境变量。export 可新增，修改或删除环境变量，供后续执行的程序使用。export 的效力仅限于该次登陆操作。

```
export [-fnp][变量名称]=[变量设置值]
```

**参数说明**：

- -f 　代表[变量名称]中为函数名称。
- -n 　删除指定的变量。变量实际上并未删除，只是不会输出到后续指令的执行环境中。
- -p 　列出所有的shell赋予程序的环境变量。

```
# export MYENV=7 //定义环境变量并赋值
# export -p //列出当前的环境变量
```



### 修改主机名&ip地址

显示主机名
	hostname

临时修改
	hostname xxx

永久修改

对于Ubuntu 系统，需要修改文件 /etc/hostname 

vim /etc/hostname

对于centos系统

vim /etc/sysconfig/network

在此配置文件中添加一条HOSTNAME=node1

针对centos7系统，可以使用如下命令

hostnamectl set-hostname xxx

一般需要重开shell甚至重启操作系统才能生效。

修改IP地址
ifconfig eth0 192.168.12.22(重启后无效)

vim /etc/sysconfig/network-scripts/ifcfg-eth0



### mount挂载

mount  挂载外部存储设备到文件系统中
mkdir   /mnt/cdrom      创建一个目录，用来挂载
mount -t iso9660 -o ro /dev/cdrom /mnt/cdrom/     

将设备/dev/cdrom挂载到 挂载点 ：  /mnt/cdrom中
umount /mnt/cdrom



### ssh免密登陆

假如 A  要登陆  B
在A上操作：
首先生成密钥对

```
ssh-keygen   (提示时，直接回车即可)
```

再将A自己的公钥拷贝并追加到B的授权列表文件authorized_keys中

```
ssh-copy-id   B
```



### 常用命令

```
1.进入到用户根目录
cd ~ 或 cd

2.查看当前所在目录
pwd

3.进入到hadoop用户根目录
cd ~hadoop

4.返回到原来目录
cd -

5.返回到上一级目录
cd ..

6.查看hadoop用户根目录下的所有文件
ls -la

7.在根目录下创建一个hadoop的文件夹
mkdir /hadoop

8.在/hadoop目录下创建src和WebRoot两个文件夹
分别创建：mkdir /hadoop/src
		  mkdir /hadoop/WebRoot
同时创建：mkdir /hadoop/{src,WebRoot}

进入到/hadoop目录，在该目录下创建.classpath和README文件
分别创建：touch .classpath
		  touch README
同时创建：touch {.classpath,README}

查看/hadoop目录下面的所有文件
ls -la

在/hadoop目录下面创建一个test.txt文件,同时写入内容"this is test"
echo "this is test" > test.txt

查看一下test.txt的内容
cat test.txt
more test.txt
less test.txt

向README文件追加写入"please read me first"
echo "please read me first" >> README

将test.txt的内容追加到README文件中
cat test.txt >> README

拷贝/hadoop目录下的所有文件到/hadoop-bak
cp -r /hadoop /hadoop-bak

进入到/hadoop-bak目录，将test.txt移动到src目录下，并修改文件名为Student.java
mv test.txt src/Student.java

在src目录下创建一个struts.xml
> struts.xml

删除所有的xml类型的文件
rm -rf *.xml

删除/hadoop-bak目录和下面的所有文件
rm -rf /hadoop-bak

返回到/hadoop目录，查看一下README文件有多单词，多少个少行
wc -w README
wc -l README

返回到根目录，将/hadoop目录先打包，再用gzip压缩
分步完成：tar -cvf hadoop.tar hadoop
		  gzip hadoop.tar
一步完成：tar -zcvf hadoop.tar.gz hadoop
		  
将其解压缩，再取消打包
分步完成：gzip -d hadoop.tar.gz 或 gunzip hadoop.tar.gz
一步完成：tar -zxvf hadoop.tar.gz

将/hadoop目录先打包，同时用bzip2压缩，并保存到/tmp目录下
tar -jcvf /tmp/hadoop.tar.bz2 hadoop

将/tmp/hadoop.tar.bz2解压到/usr目录下面
tar -jxvf hadoop.tar.bz2 -C /usr/
```

### 系统命令

```
1.查看主机名
hostname

2.修改主机名(重启后无效)
hostname hadoop

3.修改主机名(重启后永久生效)
vi /ect/sysconfig/network

4.修改IP(重启后无效)
ifconfig eth0 192.168.12.22

5.修改IP(重启后永久生效)
vi /etc/sysconfig/network-scripts/ifcfg-eth0

6.查看系统信息
uname -a
uname -r

7.查看ID命令
id -u
id -g

8.日期
date
date +%Y-%m-%d
date +%T
date +%Y-%m-%d" "%T

9.日历
cal 2012

10.查看文件信息
file filename

11.挂载硬盘
mount
umount
加载windows共享
mount -t cifs //192.168.1.100/tools /mnt

12.查看文件大小
du -h
du -ah

13.查看分区
df -h

14.ssh
ssh hadoop@192.168.1.1

15.关机
shutdown -h now /init 0
shutdown -r now /reboot
```

### 用户和组

```
添加一个tom用户，设置它属于users组，并添加注释信息
分步完成：useradd tom
          usermod -g users tom
	      usermod -c "hr tom" tom
一步完成：useradd -g users -c "hr tom" tom

设置tom用户的密码
passwd tom

修改tom用户的登陆名为tomcat
usermod -l tomcat tom

将tomcat添加到sys和root组中
usermod -G sys,root tomcat

查看tomcat的组信息
groups tomcat

添加一个jerry用户并设置密码
useradd jerry
passwd jerry

添加一个交america的组
groupadd america

将jerry添加到america组中
usermod -g america jerry

将tomcat用户从root组和sys组删除
gpasswd -d tomcat root
gpasswd -d tomcat sys

将america组名修改为am
groupmod -n am america
```

### 权限

```
创建a.txt和b.txt文件，将他们设为其拥有者和所在组可写入，但其他以外的人则不可写入:
chmod ug+w,o-w a.txt b.txt

创建c.txt文件所有人都可以写和执行
chmod a=wx c.txt 或chmod 666 c.txt

将/hadoop目录下的所有文件与子目录皆设为任何人可读取
chmod -R a+r /hadoop

将/hadoop目录下的所有文件与子目录的拥有者设为root，用户拥有组为users
chown -R root:users /hadoop

将当前目录下的所有文件与子目录的用户皆设为hadoop，组设为users
chown -R hadoop:users *
```



### 帮助文档

```
1.内部命令：echo
查看内部命令帮助：help echo 或者 man echo

2.外部命令：ls
查看外部命令帮助：ls --help 或者 man ls 或者 info ls

3.man文档的类型(1~9)
man 7 man
man 5 passwd

4.快捷键：
ctrl + c：停止进程
ctrl + l：清屏
ctrl + r：搜索历史命令
ctrl + q：退出

5.善于用tab键
```

文件相关命令

```
1.进入到用户根目录
cd ~ 或者 cd
cd ~hadoop
回到原来路径
cd -

2.查看文件详情
stat a.txt

3.移动
mv a.txt /ect/
改名
mv b.txt a.txt
移动并改名
mv a.txt ../b.txt

4拷贝并改名
cp a.txt /etc/b.txt

5.vi撤销修改
ctrl + u (undo)
恢复
ctrl + r (redo)

6.名令设置别名(重启后无效)
alias ll="ls -l"
取消
unalias ll

7.如果想让别名重启后仍然有效需要修改
vi ~/.bashrc

8.添加用户
useradd hadoop
passwd hadoop

9创建多个文件
touch a.txt b.txt
touch /home/{a.txt,b.txt}

10.将一个文件的内容复制到里另一个文件中
cat a.txt > b.txt
追加内容
cat a.txt >> b.txt 


11.将a.txt 与b.txt设为其拥有者和其所属同一个组者可写入，但其他以外的人则不可写入:
chmod ug+w,o-w a.txt b.txt

chmod a=wx c.txt

12.将当前目录下的所有文件与子目录皆设为任何人可读取:
chmod -R a+r *

13.将a.txt的用户拥有者设为users,组的拥有者设为jessie:
chown users:jessie a.txt

14.将当前目录下的所有文件与子目录的用户的使用者为lamport,组拥有者皆设为users，
chown -R lamport:users *

15.将所有的java语言程式拷贝至finished子目录中:
cp *.java finished

16.将目前目录及其子目录下所有扩展名是java的文件列出来。
find -name "*.java"
查找当前目录下扩展名是java 的文件
find -name *.java

17.删除当前目录下扩展名是java的文件
rm -f *.java
```

### VIM

```
i
a/A
o/O
r + ?替换

0:文件当前行的开头
$:文件当前行的末尾
G:文件的最后一行开头
1 + G到第一行 
9 + G到第九行 = :9

dd:删除一行
3dd：删除3行
yy:复制一行
3yy:复制3行
p:粘贴
u:undo
ctrl + r:redo

"a剪切板a
"b剪切板b

"ap粘贴剪切板a的内容

每次进入vi就有行号
vi ~/.vimrc
set nu

:w a.txt另存为
:w >> a.txt内容追加到a.txt

:e!恢复到最初状态

:1,$s/hadoop/root/g 将第一行到追后一行的hadoop替换为root
:1,$s/hadoop/root/c 将第一行到追后一行的hadoop替换为root(有提示)
```



### 查找

```
1.查找可执行的命令：
which ls

2.查找可执行的命令和帮助的位置：
whereis ls

3.查找文件(需要更新库:updatedb)
locate hadoop.txt

4.从某个文件夹开始查找
find / -name "hadooop*"
find / -name "hadooop*" -ls

5.查找并删除
find / -name "hadooop*" -ok rm {} \;
find / -name "hadooop*" -exec rm {} \;

6.查找用户为hadoop的文件
find /usr -user hadoop -ls

7.查找用户为hadoop并且(-a)拥有组为root的文件
find /usr -user hadoop -a -group root -ls

8.查找用户为hadoop或者(-o)拥有组为root并且是文件夹类型的文件
find /usr -user hadoop -o -group root -a -type d

9.查找权限为777的文件
find / -perm -777 -type d -ls

10.显示命令历史
history

11.grep
grep hadoop /etc/password
```

### 打包与压缩

```
1.gzip压缩
gzip a.txt

2.解压
gunzip a.txt.gz
gzip -d a.txt.gz

3.bzip2压缩
bzip2 a

4.解压
bunzip2 a.bz2
bzip2 -d a.bz2

5.将当前目录的文件打包
tar -cvf bak.tar .
将/etc/password追加文件到bak.tar中(r)
tar -rvf bak.tar /etc/password

6.解压
tar -xvf bak.tar

7.打包并压缩gzip
tar -zcvf a.tar.gz

8.解压缩
tar -zxvf a.tar.gz
解压到/usr/下
tar -zxvf a.tar.gz -C /usr

9.查看压缩包内容
tar -ztvf a.tar.gz

zip/unzip

10.打包并压缩成bz2
tar -jcvf a.tar.bz2

11.解压bz2
tar -jxvf a.tar.bz2
```

### 正则表达式

```
规则：
.  : 任意一个字符
a* : 任意多个a(零个或多个a)
a? : 零个或一个a
a+ : 一个或多个a
.* : 任意多个任意字符
\. : 转义.
\<h.*p\> ：以h开头，p结尾的一个单词
o\{2\} : o重复两次

grep '^i.\{18\}n$' /usr/share/dict/words

查找不是以#开头的行
grep -v '^#' a.txt | grep -v '^$' 

以h或r开头的
grep '^[hr]' /etc/passwd

不是以h和r开头的
grep '^[^hr]' /etc/passwd

不是以h到r开头的
grep '^[^h-r]' /etc/passwd
```

### 输入输出重定向

```
1.新建一个文件
touch a.txt
> b.txt

2.错误重定向:2>
find /etc -name zhaoxing.txt 2> error.txt

3.将正确或错误的信息都输入到log.txt中
find /etc -name passwd > /tmp/log.txt 2>&1 
find /etc -name passwd &> /tmp/log.txt

4.追加>>

5.将小写转为大写（输入重定向）
tr "a-z" "A-Z" < /etc/passwd

6.自动创建文件
cat > log.txt << EXIT
> ccc
> ddd
> EXI

7.查看/etc下的文件有多少个？
ls -l /etc/ | grep '^d' | wc -l

8.查看/etc下的文件有多少个，并将文件详情输入到result.txt中
ls -l /etc/ | grep '^d' | tee result.txt | wc -l
```

### 进程控制

```
1.查看用户最近登录情况
last
lastlog

2.查看硬盘使用情况
df

3.查看文件大小
du

4.查看内存使用情况
free

5.查看文件系统
/proc

6.查看日志
ls /var/log/

7.查看系统报错日志
tail /var/log/messages

8.查看进程
top

9.结束进程
kill 1234
kill -9 4333
```

