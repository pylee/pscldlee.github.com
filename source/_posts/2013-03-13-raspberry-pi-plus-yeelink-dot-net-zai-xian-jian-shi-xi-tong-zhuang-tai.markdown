---
layout: post
title: "Raspberry Pi+Yeelink.net 在线监视系统状态"
date: 2013-03-13 16:28
comments: true
categories: [Raspberry Pi] 
---
{% img /images/AE427C62-2777-4143-8E4C-E71F5A8BEF49.png %}
[Raspberry Pi](http://www.yeelink.net/devices/2047)

上图为在Yeelink.net生成的曲线图，数据来源于家中的Raspberry Pi一分钟一次的数据上传，分别上传了CPU，内存，剩余空间的数据。

上传脚本如下：
```bash
#! /bin/sh
# File path: ~/Yeelink/rpi_sensors.sh

apikey=<your api key>
url_cpu=http://api.yeelink.net/v1.0/device/<device no.>/sensor/<sensor no.>/datapoints
url_mem=http://api.yeelink.net/v1.0/device/<device no.>/sensor/<sensor no.>/datapoints
url_disk=http://api.yeelink.net/v1.0/device/<device no.>/sensor/<sensor no.>/datapoints

mem_used=$(free -m | grep "Mem" | egrep "[[:digit:]]+" -o | head -n 2 | tail -n 1)
cpu_load=$(iostat -c | tail -n 2 | egrep "[[:digit:].]+" -o | head -n 1)
disk_avaliable=$(df -l | grep "/dev/sda5" | egrep "[[:digit:]]+" -o | tail -n 3 | head -n 1)

disk_ava=`echo "$disk_avaliable/1024"|bc`
echo $disk_ava

curl -d "{\"value\":$mem_used}" -H "U-ApiKey: $apikey" $url_mem
curl -d "{\"value\":$cpu_load}" -H "U-ApiKey: $apikey" $url_cpu
curl -d "{\"value\":$disk_ava}" -H "U-Apikey: $apikey" $url_disk

exit 0
```
上面的脚本获取了CPU，内存，剩余空间的值，然后通过Yeelink提供的api地址上传上去，但是还需要通过:
```bash	
crontab -e
```
来定时运行，内容看起来像这样：
```bash
# Edit this file to introduce tasks to be run by cron.
#
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
#
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').#
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
#
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
#
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
#
# For more information see the manual pages of crontab(5) and cron(8)
#
# m h  dom mon dow   command

*/1 * * * * ~/Yeelink/rpi_sensors.sh
```
上述代码为每隔1分钟运行一次rpi_sensors.sh，这样就能不间断上传数据了，如果crontab没有修改过默认编辑器的话，建议先修改为vim。
tips：对于上传间隔Yeelink有个限制，不能小于10s。

#### 引用
[[linux]在Yeelink云端架设笔记本运行监视器](http://aguegu.net/?p=1590)

---
