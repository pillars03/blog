---
title: 写了一个shell脚本用于同步日志
date: 2018-07-04 11:50:23
tags:
---
```sh
#!/bin/bash
# author:孙国栋
function read_dir(){
    `touch /tmp/seolog/$2.log`
    for file in `ls $1`       #注意此处这是两个反引号，表示运行系统命令
    do
        if [ -d $1"/"$file ]  #注意此处之间一定要加上空格，否则会报错
        then
            read_dir $1"/"$file $2
        else
            `cat $1"/"$file  >> "/tmp/seolog/$2.log"`
        fi
    done
} 
pre_date=0
date=0
os_name=$(uname -s)
if [[ "$os_name" == "Linux" ]]; then
    #statements
    pre_date=$(date +%Y%m%d --date='-7 day')
    date=$(date +%Y%m%d --date='-1 day')
elif [[ "$os_name" == "Darwin" ]]; then
    pre_date=$(date -v -7d +%Y%m%d)
    date=$(date -v -1d +%Y%m%d)
fi
_DIR=`pwd`
_dir="$_DIR/$date/"

if [ ! -d $_dir ]
then
  echo "no file $_dir" 
  exit
fi
#读取第一个参数
read_dir $_dir $date
`rm /tmp/seolog/$pre_date.log`
```
