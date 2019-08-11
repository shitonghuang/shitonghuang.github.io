---
layout: post
title: "Linux 使用技巧"
subtitle: 'Linux Tips'
author: "Shitohuang"
header-style: text
tags:
  - others
---
## Linux 使用技巧
[TOC]

### 查看脚本命令
**查看线程**
查看所有线程
```sh
top
```
查看检索的线程
```sh
ps -ef | grep xxx
```
**检索关键词**
广义上检索关键词
```sh
grep -rn "xxxx" *
```
指定文件类型检索关键词
```sh
grep -rn --include="*.py"  dv_dist_daily_load.ksh（关键词） ./
```

```sh
find ./ -name "*.py" | xargs grep 'monitoring_result_check'
```
**装包**
wget url

**切换成可执行文件**
```sh
chmod +x filename
```
### 传递参数
```sh

s = Template("""./TMRP_FACT_05_hoops_contact_base.bteq $starttime $endtime $weekday $cntry $start_dt $end_dt > /x/home/pp_di_sha_user/Shitohuang/HOOP_Change_Impact/log/generate_data_base_$id.log""")

script = s.safe_substitute(starttime=str(schedule.iloc[i,3]),endtime=str(schedule.iloc[i,4]),weekday = weekday[schedule.iloc[i,2]],cntry =schedule.iloc[i,1],start_dt = '2019-06-01',end_dt = '2019-06-30',id = str(i))

os.system(script)

```
**转义字符处理**
对于像' " 这样的字符如果想在传参时保留，则需要加\
```sh
str = \'a\',\'b\'
echo "the str is $str"

output： the str is 'a','b'
```
""显示字符串
```sh
str = "'a','b'"
echo "the str is $str"

output: the str is 'a','b'
```
如果加上双引号" " 则代表引号内的是字符串

### zip gz 文件
```sh
gunzip xxx.gz
```
