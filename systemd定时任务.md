### systemd 定时任务服务
systemd 实际是通过定义一个timer单元来创建一个定时任务。它包含了两大内容，首先定义一个service单元，这个service单元定义了我们想定时执行的任务。然后再定义一个timer单元，通过timer单元定义定时规则去执行之前的service单元。

#### 1. 创建定时脚本

首先我们创建一个脚本叫做 now_time.sh，脚本内容就是输出当时的时间：
```bash
cat <<EOF >/opt/now_time.sh
#!/bin/bash
echo "$(date)" >>/tmp/time_record.log
EOF
chmod 755 /opt/now_time.sh  # 注意一定要更改权限，否则后面会由于没有执行权限而报错
```

####2. 创建 Service 单元

然后需要创建一个 Service单元 timerecord
```bash
cat <<EOF >/usr/lib/systemd/system/timerecord.service 
[Unit]
Description=now time service

[Service]
ExecStart=/opt/now_time.sh

[Install]
WantedBy=multi-user.target
EOF
```
然后把 timerecord 作为系统服务。

systemctl start timerecord

#### 3. 创建 Timer 单元

- 在创建 Timer单元前，我们需要先了解下如何配置 Timer
    - [Unit] # 定义元数据
    - [Timer] #定义定时器
        - OnActiveSec：定时器生效后，多少时间开始执行任务
        - OnBootSec：系统启动后，多少时间开始执行任务
        - OnStartupSec：Systemd 进程启动后，多少时间开始执行任务
        - OnUnitActiveSec：该单元上次执行后，等多少时间再次执行
        - OnUnitInactiveSec： 定时器上次关闭后多少时间，再次执行
        - OnCalendar：基于绝对时间，而不是相对时间执行,用于和 crond 类似的定时任务 ,以实际时间执行。
        - AccuracySec：如果因为各种原因，任务必须推迟执行，推迟的最大秒数，默认是60秒
        - Unit：真正要执行的任务，默认是同名的带有.service后缀的单元
        - Persistent：如果设置了该字段，即使定时器到时没有启动，也会自动执行相应的单元
        - WakeSystem：如果系统休眠，是否自动唤醒系统
    
示例：

- 在系统启动15分钟后启动，并在系统运行时，每周启动一次。

```bash
[Unit]
Description=Run foo weekly and on boot

[Timer]
OnBootSec=15min
OnUnitActiveSec=1w 

[Install]
WantedBy=timers.target
```

- 每周执行一次(周一凌晨0点)。激活后，如果错过了上次启动时间，它会立即触发服务，例如由于系统断电：
```bash
[Unit]
Description=Run foo weekly

[Timer]
OnCalendar=weekly
Persistent=true

[Install]
WantedBy=timers.target
```

- 每天周一下午3点10分运行

如果需要具体的时间，我们可以给OnCalendar 设定具体时间值，形如 Year-Month-Day Hour:Minute:Second

```bash
[Unit]
Description=Run timerecord on Monday 3:10

[Timer]
OnCalendar=Mon *-*-* 15:10:00
Unit=timerecord

[Install]
WantedBy=timers.target
```

- 每隔5秒执行一次
```bash
[Unit]
Description=Run timerecord  every 5s

[Timer]
OnUnitActiveSec=5s # 可以设置为 5m,1h
Unit=timerecord

[Install]
WantedBy=timers.target
```

- 每个小时的每隔10分钟 进行备份服务
```bash
[Unit]
Description=backup

[Timer]
OnCalendar=*-*-* 0/1:0/10:00
Unit=backup

[Install]
WantedBy=multi-user.target
```

- 创建 Timer 单元 record：每隔1分钟执行一次

```bash
cat <<EOF > /usr/lib/systemd/system/timerecord.timer
[Unit]
Description=Run timerecord  every 5s

[Timer]
OnUnitActiveSec=1m # 可以设置为 5m,1h
Unit=timerecord #指定 Service 服务名

[Install]
WantedBy=timers.target
EOF
```

#### 启用定时任务
```bash
systemctl  daemon-reload  #重新加载配置
systemctl  start  timerecord.timer # 启动定时任务
常用命令：

systemctl  start  timerecord.timer# 启动定时任务
systemctl  stop  timerecord.timer# 暂停定时任务
systemctl  status  timerecord.timer# 查看定时任务服务状态
systemctl  restart  timerecord.timer# 重启定时任务状态
systemctl list-timers --all # 查看定时任务列表
systemctl  daemon-reload  # 更改了配置文件后，需要重新加载
journalctl -u mytimer.timer # 查看 mytimer.timer 的日志
journalctl -u mytimer # 查看 mytimer.timer 和 mytimer.service 的日志
journalctl -f # 从结尾开始查看最新日志
journalctl -f -u timer.timer #  从结尾开始查看 mytimer.timer 的日志
```
