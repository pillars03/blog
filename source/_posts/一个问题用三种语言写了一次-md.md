---
title: 一个问题用三种语言写了一次.md
date: 2018-07-16 14:41:10
tags:
---

```py
#!/usr/bin/python
# -*- coding: UTF-8 -*-

import os

def getAllFile(dir,file_arr=None):
    if file_arr is None:
        file_arr = []
    all_file = os.listdir(dir)
    for file in all_file:
        path = os.path.join(dir, file)
        if os.path.isdir(path):
            getAllFile(path,file_arr)
        else:
            file_arr.append(path)
    return file_arr
cc = getAllFile('20180703')
file = open('/tmp/testpy.log','a')
for log in cc:
    log_file = open(log)
    str = log_file.read()
    log_file.close()
    file.write(str)
file.close
```

```php
function get_allfiles($path, &$files)
{
    if (is_dir($path)) {
        $dp = dir($path);
        while ($file = $dp->read()) {
            if ($file != "." && $file != "..") {
                get_allfiles($path . "/" . $file, $files);
            }
        }
        $dp->close();
    }
    if (is_file($path)) {
        $files[] = $path;
    }
}
//新加文件名
$dir = date('Ymd', strtotime("-1 day"));
//要删除的文件名字
$delete_dir = date('Ymd',strtotime("-7 day"));
unlink("/tmp/seolog/{$delete_dir}.log");

function get_filenamesbydir($dir)
{
    $files = array();
    get_allfiles($dir, $files);
    return $files;
}
$filenames = get_filenamesbydir($dir);
//打印所有文件名，包括路径  
$log = fopen("/tmp/seolog/{$dir}.log", 'a+');
foreach ($filenames as $value) {
    $str = file_get_contents($value);
    fwrite($log, $str);
}
fclose($log);
echo $dir."done\n";
```
