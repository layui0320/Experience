# 命令
## !! 上一条命令
```sh
sudo !!
PYTHONIOENCODING=utf-8 !!
```
## cat 输出内容
```sh
cat 文件 
```
### 查看cpu型号及逻辑处理器数
```sh
cat /proc/cpuinfo | grep name | cut -f2 -d: | uniq -c
```
## cd 切换目录
```sh
cd 目录
```
### 转到用户目录
```sh
cd
```
### 转到上级目录
```sh
cd ..
```
## chgrp 设置所有组
```sh
chgrp [参数] 用户组名 文件
```
参数|意义
-|-
-R|递归设置
## chmod 设置权限
普通用户只能设置自己的文件  
u所有者 g用户组 o其他人 a所有
r4 w2 x1  
=设置权限 +授予权限 -收回权限  
### 惯例
```sh
chmod 755 目录
chmod 644 文件
```
### 皆可读
```sh
chmod ugo+r 文件
chmod a+r 文件
```
### u可执行，g只读，o不可写
```sh
chmod u+x,g=r,o-w 文件
```
### 递归设置权限
```sh
chmod -R 644 * # 当前文件夹下的文件
chmod -R 644 . # 当前文件夹及其文件
# chmod -R 000 / # 禁令
```
## chown 设置所有者
```sh
chown [参数] 用户名[:用户组名] 文件
```
参数|意义
-|-
-R|递归设置
## clear 清屏
```sh
clear
```
## cowsay 奶牛说话
```sh
cowsay 文字
```
## df 查看硬盘使用情况
```sh
df [参数]
```
参数|意义|
-|-
-h|说人话
## du 查看目录大小
```sh
du [参数] [目录]
```
参数|意义|
-|-
-h|说人话
--max-depth|子目录层数
### 说人话且只显示一层子目录
```sh
du -h --max-depth=1 [目录]
```
## echo 输出并换行
```
echo 文字
echo $变量名
echo ${变量名}
echo "abc${变量名}def"
```
### 转义
无引号时，2n、2n+1个\\输出为n个\\  
单引号时，n个\\输出为n个\\  
双引号时，2n-1、2n个\\输出为n个\\  
-e时，将无-e时的输出进行转义
## find 查找文件
```sh
find 目录 参数 模式
```
参数|意义|
-|-
-name|文件名
-iname|文件名忽略大小写
## grep 查找符合条件的行
```sh
grep [参数] 正则表达式 文件
```
参数|意义
-|-
-n|显示行号
-r|递归查找
## groupadd 添加用户组
```sh
groupadd 组名
```
## groupdel 删除用户组
```sh
groupdel [-f] 组名
```
-f强制删除，组内用户所属组只有组编号
## groupmod 修改用户组
```sh
groupmod -n 新组名 组名
```
## groups 查看用户所属用户组
```sh
groups [用户名]
```
默认当前用户  
输出用户名:用户组
## ifconfig 输出网络配置
```sh
ifconfig
```
## kill 结束进程
```sh
kill [参数] 进程号
```
参数|意义
-|-
-9|强行结束进程
--|结束进程树，进程号前加-
kill父进程，子进程会被pid=1的init接管
## ls 列出文件
```sh
ls [参数] [目录]
```
参数|意义
-|-
-a|包括隐藏的文件
-ct|按修改时间降序
-l|分行显示详细信息
-r|反序
-R|递归输出
-S|按文件大小降序
-X|按扩展名升序
### 输出python脚本
```sh
ls *.py
```
## lsusb 显示usb设备
```sh
lsusb
```
## man 查看命令手册
```sh
man 命令
```
## mv 移动或重命名
```sh
mv 文件 新文件
```
## nice 更改优先级执行
```sh
nice [-n 相对优先级] 命令
```
相对优先级-20~19，默认10，越低越优先，非管理员不得赋予负值
## nohup 忽略sighup执行
```sh
nohup 命令 [&]
```
nohup和&同时使用时，退出bash和Ctrl-C都不结束程序，只是父进程从bash变成了init
## passwd 设置密码
```sh
passwd [用户名]
```
默认为当前用户
## ps 列出用户进程
```sh
ps [参数]
```
参数|意义
-|-
-e|所有进程
-f|全格式
-l|长格式

标题|意义
-|-
F|标志
S|状态
UID|用户ID
PID|进程号
PPID|父进程号
C|CPU使用率
PRI|调度优先级，数值越小越优先
NI|Nice值，对基准优先级的调整
ADDR|通常情况下，包含进程栈的段号；如果为内核进程，那么为预处理数据区的地址
SZ|占用内存KB
WCHAN|wait channel，正在因为哪个内核函数而休眠
STIME|启动时间
TTY|用户终端位置
TIME|CPU时间
CMD|启动命令

标志|意义
-|-
1|forked but didn't exec
4|used super-user privileges

状态|意义
-|-
D|Uninterruptible sleep (usually IO)
R|Running or runnable (on run queue)
S|Interruptible sleep (waiting for an event to complete)
T|Stopped, either by a job control signal or because it is being traced.
W|paging (not valid since the 2.6.xx kernel)
X|dead (should never be seen)
Z|Defunct ("zombie") process, terminated but not reaped by its parent.
## pwd 输出当前目录
```sh
pwd
```
## rm 删除
```sh
rm [参数] 文件或目录
```
参数|意义
-|-
-f|强制删除
-r|递归删除
## service 服务启停
```sh
service 服务 命令
```
命令|意义
-|-
start|启动
restart|重启
stop|停止
## ssh 远程连接
```sh
ssh 用户名@IP地址
```
## source 在当前shell执行脚本
```sh
source 脚本
```
## su 切换到root
```sh
su
```
## tail 查看文件末尾
```sh
tail [参数] 文件
```
参数|意义
-|-
-f|实时监视
-n|行数，默认10行
## tar 压缩解压
```sh
tar 参数 压缩文件 [被压缩文件]
```
参数|意义
-|-
-c|压缩
-x|解压
-z|使用gzip
-v|输出细节
-f|后跟压缩文件
## top 任务管理器
```sh
top
```
## uname 查看系统信息
```sh
uname [参数]
```
参数|意义
-|-
-a|全部
-m|硬件平台
-r|内核版本
## useradd 添加用户
```sh
useradd [-g 组名] [-m] 用户名
```
-m自动建立用户目录/home/用户名  
默认组与用户同名，若同名组已存在则必须指定组名
## userdel 删除用户
```sh
userdel [参数] 用户名
```
参数|意义
-|-
-f|强制删除
-r|同时删除用户目录、mail spool

被删用户已登录时，可以强制删除，但不会自动登出，新建的文件没有用户名只有用户编号  
被删用户所属文件的所有者变为编号，若新增用户的编号等于此编号，则继承此文件  
被删用户若与所属用户组同名且是组里唯一用户，会同时删除用户组
## usermod 修改用户
```sh
usermod [-g 新所属组] [-l 新用户名] 用户名
```
## wget 下载文件
```sh
wget URI
```
# 权限
## 文件权限
### r 读取文件
包括用解释器执行文件
### w 写入文件
### x 执行文件
./形式执行
## 目录权限
### r 列出目录下的文件
### w 修改目录
新建文件、重命名文件、删除文件（无论文件所有者与权限）
### x 进入目录