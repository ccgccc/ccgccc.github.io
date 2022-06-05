---
title: "原创 Linux 宝典"
date: 2022-06-05
---

# Linux
* TOC  
{:toc}

**Explanation**
```
[] : Optional
<> : Variable
() : Replacement(no space left to former word) / Explanation
|  : Or
```
## Command
### Linux Help
```
<command> --help | -? : show command help
<command> --usage : show command usage
man <command> : show manual pages

type <command> : Describe a command. For each name, indicate how it would be interpreted if used as a command name.
```
```
// 我是谁
whoami

// 都有谁 : 显示当前所有登录用户
who

// 它是啥 : 显示命令的简要介绍
whatis <command>

// 它在哪 : 定位可执行文件、源代码文件、帮助文件在文件系统中的位置
whereis <file> : Search $path, man pages and source files for an application file.

// 定位命令所在位置
which <command> : search the user's $path for a program file

// 显示终端机连接标准输入设备的文件名称
tty : print the file name of the terminal connected to standard input

// 查看历史执行命令
history
```
<br/>



### File & Directory Management
#### cp | mv | rm
```
cp : Copy src to desc, or multiple source(s) to directory
cp <filename1> <filename2> : copy filename1 to filename2(exist or not)
cp <filename1> <filename2> <dir> : copy filename1 and filename2 to dir
cp -r <dir1> <dir2> : copy dir1 and dir1's subdirectory to dir2
cp -r <dir1>/. <dir2> : copy dir1's subdirectory to dir2

mv : move or rename files or directories
mv <filename> <dir>/ : move file to another directory
mv <filename> <dir>/<filename2> : move file
mv <filename> <filename2> : rename file

rm <file> : remove files
rm -f <file> : force to remove files
rmdir <dir> : remove empty folders
rm -rf <dir> : force to remove folders
```
#### touch | mkdir
```
touch <filename> : create new file

mkdir <dir> : create new folder(s) (make directory)
```
#### cd | pwd
```
cd : change directory
cd ~ : enter home
cd - : enter last directory

pwd : print working directory
```
#### ls
```
ls : list information about the FILEs (the current directory by default).
ls -l : use a long listing format
ll : alias for "ls -l" in some system
ls -lh : with human readable size
```
```
// about ls
ls * : print current directory files(list way) & subdirectory files(dir way)
ls */ : print subdirectory files(dir way, dir end with "/")
ls */* : print subdirectory files(list way) & subsubdirectory files(dir way)
ls */*/ : print subsubdirectory files(dir way, dir end with "/")

Note: shell print add one white "/" to dir compared to bash script
```
<br/>



### File Contents
#### cat
```
cat <filename> : concatenate and print (display) the content of files
```
#### less | more
```
// less if more than 'more'
less <file>

// 逐页显示文件
more <file>
```
**Ref:**  
[Linux less 命令 | 菜鸟教程 *](https://www.runoob.com/linux/linux-comm-less.html)  
[less Man Page - Linux - SS64.com *](https://ss64.com/bash/less.html)  
[command line - What are the differences between most, more and less? - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/81129/what-are-the-differences-between-most-more-and-less)  

#### tail
```
tail <filename> : output the last part of file
tail -f <filename> : print last 10 lines and update as the file changes
```
<br/>



### File Permission
#### chmod
```
// 给文件分配权限
chmod 755 <file>

// 给所有用户分配<dir>目录及其子目录全部权限
chmod 777 -R <dir>
```
<br/>



### Archive & Compress
#### tar
```
// 压缩和解压缩
// tar
tar -cf <archive>.tar <foo> <bar>  # Create archive.tar from files foo and bar.
tar -tvf <archive>.tar         # List all files in archive.tar verbosely.
tar -xf <archive>.tar          # Extract all files from archive.tar.

// tar.gz
tar -zcvf <compressd_filename>.tar.gz <filename1> <filename2> : pack
tar -zxvf <compressed_filename> : unpack
tar -zxvf <compressed_filename> <dir> : unpack to dir(dir must exist)
```
#### unzip
```
// zip (zip、jar)
unzip <archive>.zip : unpack
unzip -d <dir> <archive>.zip : unpack to dir
```
#### jar
```
// jar (provided by jdk)
// unpack to current dir(i.e. jar dir)
jar -xvf <archive>.jar
// unpack jar to other dir
cd <other_dir>
jar -xvf <jar_path>/<archive>.jar
```
<br/>



### Processes
#### ps
```
// 查找并杀死相应进程
// -e(or -A): all processes；查找所有线程
// -f: full-format, including command lines；包含命令行参数
ps -ef | grep <str>

// -o: -o, o, --format <format> : user-defined format
// 指定用户自定义格式，pid指定进程id，args指定命令行参数
ps -e -o pid,args

kill -9 pid_1 pid_2 ...
```
#### lsof
```
// 查找占用端口进程
lsof -i:<port>
```
#### pstree
```
// 显示进程树
pstree

// -a: 显示命令参数
// -p: 显示 pid
// -n: 按 pid 排序
pstree -apn
```
#### htop | top
```
// 安装 : yum install htop
// 操作更为方便，并且显示带颜色，更易查看
htop : Interactive Process viewer

top : Interactive Process viewer
```
**Ref:** [Compare top vs htop | CodeAhoy](https://codeahoy.com/compare/top-vs-htop)  
<br/>



### Pipes & Filters
#### grep
```
// 查找字符串
// grep --help
//  Usage: grep [OPTION]... PATTERN [FILE]...
//  Search for PATTERN in each FILE or standard input.
//  PATTERN is, by default, a basic regular expression (BRE).
//  Example: grep -i 'hello world' menu.h main.c
// -i, --ignore-case         ignore case distinctions
// -r, --recursive           like --directories=recurse

// 当前目录下的所有文件中查找字符串(可以没有引号，但是中间就不能有空格了)
grep 'xxxx' -r ./
grep 'xxxx' -C 30 -r ./  :前后30行
grep 'xxxx' -A 30 -r ./  :后30行
grep 'xxxx' -B 30 -r ./  :前30行
(e.g.  grep -C 100 BusinessException -r ./)
```
```
// 当前目录下的所有文件中查找字符串
find . | xargs grep -r 'xxxx' 
// ...并输出到文件
find . | xargs grep -r 'xxxx' > ../ccg.txt

// 当前目录下的所有文件中查找字符串,并且只打印出含有该字符串的文件名
find . | xargs grep -r 'xxxx' -l 
find . -name '*.xml' | xargs grep -r 'xxxx' -l
```
```
// 关于 linux 单引号 vs 双引号
// 双引号: soft quote，解析变量
ps -ef | grep "$JAVA_HOME"
// 单引号: hard quote，不解析变量
ps -ef | grep '$JAVA_HOME'
```

#### sed
```
// 采用流编辑模式，根据脚本命令来处理文本文件中的数据
// SED : Stream EDitor
// A stream editor is used to perform basic text transformations on an input stream (a file or input from a pipeline).
// While in some ways similar to an editor which permits scripted edits, SED works by making only one pass over the input(s), and is consequently more efficient. 
// But it is SED's ability to filter text in a pipeline which particularly distinguishes it from other types of editors.
sed [OPTION]... {script-only-if-no-other-script} [input-file]...
sed [选项] [脚本命令] 文件名

参数说明：
-e<script> --expression=<script> ：以选项中指定的script来处理输入的文本文件。
-f<script文件> --file=<script文件> ：以选项中指定的script文件来处理输入的文本文件。
-n ：默认情况下，sed 会在所有的脚本指定执行完毕后，会自动输出处理后的内容，而该选项会屏蔽启动输出，需使用 print 命令来完成输出。
-i ：此选项会直接修改源文件，要慎用。

----------

脚本命令格式：
[address]脚本命令
[address]{多个脚本命令}

脚本命令动作说明：
a ：新增，a 的后面可以接字串，而这些字串会在新的一行出现(目前的下一行)～
i ：插入，i 的后面可以接字串，而这些字串会在新的一行出现(目前的上一行)；
c ：取代，c 的后面可以接字串，这些字串可以取代 n1,n2 之间的行！
d ：删除，因为是删除啊，所以 d 后面通常不接任何东东；
p ：打印，亦即将某个选择的数据印出。通常 p 会与参数 sed -n 一起运行～
s ：取代，可以直接进行取代的工作哩！通常这个 s 的动作可以搭配正规表示法！例如 1,20s/old/new/g 就是啦！
y ：唯一可以处理单个字符的 sed 脚本命令，对字符进行一一替换；
r ：将一个独立文件的数据插入到当前数据流的指定位置；
q ：使 sed 命令在第一次匹配任务结束后，退出 sed 程序，不再进行对后续数据的处理

脚本命令寻址方式：
默认情况下，sed 命令会作用于文本数据的所有行。如果只想将命令作用于特定行或某些行，则必须写明 address 部分，表示的方法有以下 2 种：
1. 以数字形式指定行区间， e.g. 2,$<command>：匹配从第2行开始的所有行
2. 用文本模式指定具体行区间 : e.g. /<pattern>/<command>：匹配符合pattern的所有行
```
```
// sed a & i : a命令表示在指定行的后面附加一行，i命令表示在指定行的前面插入一行
[address]a(或i)\新文本内容
// 在 testfile 文件的第四行后添加一行“newLine”，并将结果输出到标准输出
sed -e 4a\newLine testfile
```
```
// sed s : 数据的查找与替换
sed '[address]s/pattern/replacement/flags'

s命令flags标记及功能：
n ：1~512 之间的数字，表示指定要替换的字符串出现第几次时才进行替换，例如，一行中有 3 个 A，但用户只想替换第二个 A，这是就用到这个标记；
g ：对数据中所有匹配到的内容进行替换，如果没有 g，则只会在第一次匹配成功时做替换操作。例如，一行数据中有 3 个 A，则只会替换第一个 A；
p ：会打印与替换命令中指定的模式匹配的行。此标记通常与 -n 选项一起使用。
w file ：将缓冲区中的内容写到指定的 file 文件中；
& ：用正则表达式匹配的内容进行替换；
\n ：匹配第 n 个子串，该子串之前在 pattern 中用 \(\) 指定。
\ ：转义（转义替换部分包含：&、\ 等）。

// 将 testfile 文件中每行第一次出现的 oo 用字符串 kk 替换，然后将该文件内容输出到标准输出:
sed -e 's/oo/kk/' testfile
```
**Ref:**  
[Linux sed 命令 | 菜鸟教程](https://www.runoob.com/linux/linux-comm-sed.html)  
[Linux sed命令完全攻略（超级详细）](http://c.biancheng.net/view/4028.html)  
[sed Man Page - Linux - SS64.com](https://ss64.com/bash/sed.html)  

#### awk
```
// 一个更强大的文本数据处理工具
// AWK: Find and Replace text, database sort/validate/index.
// The awk command is used for text processing in Linux. 
// Although, the sed command is also used for text processing, but it has some limitations, 
// so the awk command becomes a handy option for text processing. It provides powerful control to the data.
awk <options> 'Program' Input-File1 Input-File2 ...
awk [选项] '脚本命令' 文件名

-F fs ：指定以 fs 作为输入行的分隔符，awk 命令默认分隔符为空格或制表符。
-f file ：从脚本文件中读取 awk 脚本指令，以取代直接在命令行中输入指令。
-v var=val ：在执行处理过程之前，设置一个变量 var，并给其设备初始值为 val。

----------

脚本命令格式：
'匹配规则{执行命令}'
在 awk 程序执行时，如果没有指定执行命令，则默认会把匹配的行输出；
如果不指定匹配规则，则默认匹配文本中所有的行。

awk 的主要特性之一是其处理文本文件中数据的能力，它会自动给一行中的每个数据元素分配一个变量。
默认情况下，awk 会将如下变量分配给它在文本行中发现的数据字段：
$0 代表整个文本行；
$1 代表文本行中的第 1 个数据字段；
$2 代表文本行中的第 2 个数据字段；
$n 代表文本行中的第 n 个数据字段。
在 awk 中，默认的字段分隔符是任意的空白字符（例如空格或制表符）。
在文本行中，每个数据字段都是通过字段分隔符划分的。
awk 在读取一行文本时，会用预定义的字段分隔符划分每个数据字段。

// 读取文本文件，只显示第 1 个数据字段的值
awk '{print $1}' data.txt
// 第一条命令会给字段变量 $4 赋值。第二条命令会打印整个数据字段。
// 输出：My name is Christine
echo "My name is Rich" | awk '{$4="Christine"; print $0}'
```
**Ref:**  
[Linux awk命令详解](http://c.biancheng.net/view/4082.html)  
[Linux awk 命令 | 菜鸟教程](https://www.runoob.com/linux/linux-comm-awk.html)  
[awk gawk Man Page - Linux - SS64.com](https://ss64.com/bash/awk.html)  
[Linux AWK Command - javatpoint](https://www.javatpoint.com/linux-awk-command)  
<br/>



### Networking
#### ifconfig | ping
```
// 查看网络信息
ifconfig

// 检查网络连接
ping
```

#### netstat
```
// 查找占用端口
netstat : networking connections/stats
-a, --all : display all sockets (default: connected)
-n, --numeric : don't resolve names
-p, --programs : display PID/Program name for sockets

// 查看端口占用
netstat -an | grep <port>

// -p参数可查占用进程pid和程序名
netstat -anp|head -2;netstat -anp|grep <port>

// 查看谁连了服务器22端口
// <Socket>={-t|--tcp} {-u|--udp}
netstat -tunp | grep 22
```
```
// 安装 netstat
yum install net-tools
```

#### curl
```
// Transfer data from or to a server, using one of the protocols: 
// HTTP, HTTPS, FTP, FTPS, SCP, SFTP, TFTP, DICT, TELNET, LDAP or FILE. 
// (To transfer multiple files use wget or FTP.)
curl <url>

// Retrieve a web page and save to a file
curl https://ss64.com/bash/ -o ss64.html
```

#### wget
```
// Retrieve web pages or files via HTTP, HTTPS or FTP
// 相较于 curl，wget会默认保存文件
wget <url>

// -O 指定保存文件名
// -O,  --output-document=FILE : write documents to FILE.
// -o,  --output-file=FILE : log messages to FILE.
// -N,  --timestamping : don't re-retrieve files unless newer than local.
// --no-check-certificate : don't validate the server's certificate.
// -q,  --quiet : quiet (no output).
wget -N --no-check-certificate -q -O install.sh "https://raw.githubusercontent.com/ccgccc/Xray_onekey/nginx_forward/install.sh"
```

#### firewall-cmd
```
// 防火墙
1.启动防火墙
systemctl start firewalld 
2.禁用防火墙
systemctl stop firewalld
3.设置开机启动
systemctl enable firewalld
4.停止并禁用开机启动
systemctl disable firewalld
5.重启防火墙
firewall-cmd --reload
6.查看状态
systemctl status firewalld
firewall-cmd --state
7.查看版本
firewall-cmd --version
8.查看帮助
firewall-cmd --help

// 查看已开放端口
firewall-cmd --zone=public --list-ports
// 添加开放端口
firewall-cmd --zone=public --add-port=80/tcp --permanent
```
<br/>



### Communication
#### scp
```
// scp: secure copy
// 下载文件到本地
scp <user>@<ip>:<remote_file_path> <local_file_path>
// 上传文件到远程
scp <local_file_path> <user>@<ip>:<remote_file_path>
```
<br/>



### Tools
#### find
```
// 查找文件
find <path1> <path2> -name '*xxx*.jar' (default current directory)

// 查找文件并列出详细信息
find . -name *.java | xargs ls -l
find . -name *.java -exec ls -l {} \;
```
#### locate
```
// 定位文件
locate <reg_str>

// 定位目录下文件
locate /usr/*nginx*
or
locate nginx | grep ^/usr
```
```
// 安装 locate
yum install mlocate

// 更新 locate 数据库(默认每天更新一次)
updatedb
```
**Ref:**
[command line - Use "locate" under some specific directory? - Ask Ubuntu](https://askubuntu.com/questions/33280/use-locate-under-some-specific-directory)  
[Linux locate命令 | 菜鸟教程](https://www.runoob.com/linux/linux-comm-locate.html)
#### df
```
// 查看目录大小
// df: disk free
df -h : 查看磁盘空间

// du: disk use
du -h --max-depth 1 //单层目录
du -h --max-depth 1 | sort //单层目录并排序
```
#### date
```
date : Display current time
```
<br/>



### System Admin
#### reboot | shutdown
```
reboot : reboot the system
shutdown : shutdown or restart linux
```

#### System Info
##### uname
```
// 查看系统版本
uname -s
cat /proc/version
```
##### lsb_release
```
// linux发行版本
// -a : Display all information.
lsb_release -a
```
##### lscpu
```
// 查看cpu信息
lscpu
cat /proc/cpuinfo
```
##### free
```
// 显示内存信息
free -h
```
##### neofetch
```
// Neofetch displays information about your system next to an image, 
// your OS logo, or any ASCII file of your choice.
neofetch

// 安装 neofetch
yum install dnf
dnf install epel-relase
curl -o /etc/yum.repos.d/konimex-neofetch-epel-7.repo https://copr.fedorainfracloud.org/coprs/konimex/neofetch/repo/epel-7/konimex-neofetch-epel-7.repo
yum install neofetch
```
**Ref:**  
[Neofetch - Display Linux system Information In Terminal - OSTechNix](https://ostechnix.com/neofetch-display-linux-systems-information/)  
[How to Install and Use Neofetch on Linux ( RHEL /CentOS / Arch ) – LinuxWays](https://linuxways.net/centos/how-to-install-and-use-neofetch-on-linux-rhel-centos-arch/)  

#### Environment Variable
##### export | unset
```
// 查看、设置、清除环境变量
echo $var
export <var>=<value>
unset <var>
```
#### lsmod
```
lsmod (list modules) : show which loadable kernel modules are currently loaded
```
<br/>



### SELinux
#### semanage
```
// SELinux增加开放http端口
semanage port -a -t http_port_t -p tcp <port>
// SELinux列出开放http端口
semanage port -l | grep http_port_t
```
<br/>





## Solution
### Installation
#### Install Fonts
```
// linux安装字体
cd /usr/share/fonts/
mkdir myfonts
cd ./myfonts 并上传字体到此目录

mkfontscale
mkfontdir
fc-cache -fv

// 查看已安装字体
fc-list
```
#### Install Browser
```
// linux浏览器
yum install links
links www.google.com

yum install lynx
lynx www.google.com
```


### Clean Disk
```
// 清理空间
yum clean all  
删除 /var/log 下文件
```


### Change SSH Connect Port
```
sudo vi /etc/ssh/sshd_config
Locate line that read as follows: Port 22 or #Port 22
To set the port to 2222, add line and enter: Port 2222

// run this
semanage port -a -t ssh_port_t -p tcp 2222
// check
semanage port -l | grep ssh

sudo service sshd restart (sudo systemctl restart sshd)
```
**Ref:**  
[How to change the ssh port on Linux or Unix server - nixCraft](https://www.cyberciti.biz/faq/howto-change-ssh-port-on-linux-or-unix-server/)
[unix - How do I set up SSH so I don't have to type my password? - Super User](https://superuser.com/questions/8077/how-do-i-set-up-ssh-so-i-dont-have-to-type-my-password/545127#545127)  
[Linux添加或修改ssh端口 - 苦逼运维 - 博客园](https://www.cnblogs.com/diantong/p/9877132.html)  
<br/>



## Linux Information
### Common Info
```
// Common Unix and Linux ports
Port Protocol Service
20	tcp	ftp-data
21	tcp	ftp server
22	tcp	ssh server
23	tcp	telnet server
25	tcp	email server
53	tcp/udp	Domain name server
69	udp	tftp server
80	tcp	HTTP server
110	tcp/udp	POP3 server
123	tcp/udp	NTP server
443	tcp	HTTPS server
```


### Linux Config Files
```
/etc/sudoers : 配置sudo用户
```
