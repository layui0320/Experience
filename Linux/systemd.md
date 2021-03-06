# 配置
## 服务.service
一般位于`/etc/systemd/system`或`/usr/lib/systemd/system`
```
[Unit]
Description=介绍
Documentation=文档
After=建议在谁之后启动，空格分隔
Before=建议在谁之前启动
Conflicts=与谁冲突
Requires=必须在谁之后启动
Wants=启动后启动谁

[Service]
User=执行用户
Type=forking或oneshot
EnvironmentFile=从文件读取环境变量，先导-表示文件不存在时不报错
WorkingDirectory=工作目录
ExecStart=启动命令
ExecStop=停止命令
ExecReload=重新加载配置文件命令
RestartSec=重启前等待时间，无单位则为秒，默认100ms

[Install]
WantedBy=目标.target
Also=改变自启状态时同时将改变谁的自启状态
```
* `man systemd.service`
* Type为forking意味着ExecStart的进程fork子进程后退出，子进程成为主进程
* 若没有\[Install\]，is-enabled输出static，无法自启
* 为自启需要将WantedBy设为默认目标或其依赖；enable在目标的启动服务目录下创建指向此文件的软链接
* 服务名带有`@`时为模板，`@`到`.`之间的部分可用`%i`或`%I`（不转义）指代
## 服务.timer
```sh
[Timer]
OnActiveSec=timer单元启动多长时间后执行
OnBootSec=开机后多长时间执行
OnStartupSec=服务管理器启动后多长时间执行
OnUnitActiveSec=上次服务启动后多长时间执行
OnUnitInactiveSec=上次服务结束后多长时间执行
OnCalendar=具体时刻执行
AccuracySec=时钟精度，默认1min
```
* `man systemd.timer`
* 需要同名的service存在
# 命令
## systemctl 管理systemd
```sh
systemctl [选项] [命令 [单元[.类型]]]
systemctl 系统命令
```
命令|意义
-|-
start|启动服务
status|服务状态
stop|停止服务
enable|自启
is-enabled|是否自启
disable|禁止自启
mask|禁用
unmask|解除禁用
daemon-reload|重新加载系统配置
get-default|默认目标
set-default|设置默认目标
isolate|切换目标
list-dependencies|目标、服务间依赖关系，`--reverse`反向查找
list-timers|列出定时器
list-units|列出各单元
list-unit-files|列出各单元文件及其自启状态
show|查看属性

选项|意义
-|-
-a|显示全部，否则只显示active
-t 类型|device、mount、service、socket、target
-p 属性|配置文件中的属性名，`,`分隔

系统命令|意义
-|-
emergency|紧急救援模式，只运行systemd及shell
hibernate|休眠
poweroff|关机
rescue|救援模式，运行systemd及服务，自动挂载
suspend|睡眠
## systemd-analyze 启动分析
```sh
systemd-analyze [项目] [参数]
```
### time 启动时间
* 固件、内核、initrd、用户空间
* 进入用户空间后，默认目标完成的时间
### blame 各服务初始化时间排序
### critical-chain [目标或服务] 初始化时间依赖
## systemd-resolve 域名解析管理
```sh
systemd-resolve [选项]
```
选项|意义
-|-
--flush-caches|清空缓存
--statistics|查看统计信息
# multi-user.target
## basic.target
## rc-local.service 执行rc.local
* 若`/etc/rc.d/rc.local`可执行，则自动属于multi-user.target
## getty.target 终端
```sh
systemctl start getty@tty8.service
```
# basic.target
## sysinit.target
## timers.target 系统定时器
# sysinit.target
## dev-mqueue.mount 挂载消息队列
## plymouth-start.service 开机过程提供图形界面
## systemd-journald.service systemd日志
```sh
journalctl [选项] [匹配]
```
选项|意义
-|-
-e|-n1000并跳到尾部
-f|实时监视
-n 行数|显示最后几行
-p 等级|指定等级，同rsyslog的priority
-r|反序，由新到旧
-S 时间|起始时间
-U 时间|结束时间
-x|根据类别增加解释信息

匹配|意义
-|-
_COMM=程序|该程序的日志
_PID=?|该进程的日志
_SYSTEMD_UNIT=服务.service|该服务的日志
_UID=?|该用户的日志
SYSLOG_FACILITY=序号|syslog.h的facility
* 匹配见`man systemd.journal-fields`
## systemd-modules-load.service 加载内核模块
## systemd-random-seed.service 存取随机数种子
## systemd-sysctl.service 开机时配置内核参数
## systemd-timesyncd.service 网络时间同步
```sh
timedatectl set-ntp 0或1
```
* 设为1则同时start和enable
## local-fs.target 挂载本地文件系统
## swap.target 开启swap
# local-fs.target
## systemd-remount-fs.service 重新挂载文件系统
* 按`/etc/fstab`中的配置重新挂载`/`、`/usr`和虚拟文件系统
# 其他服务
## systemd-hostnamed.service 查看/修改主机名
* 服务按需启动停止
```sh
hostnamectl [set-hostname 主机名]
```
## systemd-localed.service
* 服务按需启动停止
### 查看当前本地化
```sh
localectl [status]
```
### 列出可用的本地化
```sh
localectl list-locales
```
### 设置本地化
```sh
localectl set-locale LOCALE # LANG=
localectl set-locale 变量1=LOCALE1 [...]
```
* 写入`/etc/locale.conf`
## systemd-timedated.service 系统时间
### 查看当前设置
```sh
timedatectl [status]
```
### 列出时区
```sh
timedatectl list-timezones
```
### 修改时区
```sh
timedatectl set-timezone 时区
```
### 设置时间
```sh
timedatectl set-time "时间"
```
* 同时设置系统时间和硬件时间
### 设置硬件时钟时区
```sh
timedatectl set-local-rtc 0或1
```
* 0为UTC，1为本地时间，建议0
* 修改`/etc/adjtime`第三行
