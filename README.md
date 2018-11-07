### think-backup
> 【特别鸣谢】https://github.com/tp5er/tp5-databackup

>`think-backup`扩展是在`https://github.com/tp5er/tp5-databackup`的基础上，做了部分调整。
>目前可以满足个人需求。

主要功能有：
* 数据表的备份（单表备份/多表备份）
* 数据备份的还原
* 数据表的优化（支持多表）
* 数据表的修复（支持多表）
* 备份文件的下载
* 清空表（支持多表清空）
### 使用composer进行安装
~~~
     composer require yifeng/think-backup
~~~

### 使用composer update进行安装
~~~
    "require": {
        "yifeng/think-backup": "1.*"
    },
~~~

### 引入类文件
~~~
use think\Backup;
~~~

### 配置文件
~~~
$config=array(
    'path'     => './Data/',//数据库备份路径
    'part'     => 20971520,//数据库备份卷大小
    'compress' => 0,//数据库备份文件是否启用压缩 0不压缩 1 压缩
    'level'    => 9 //数据库备份文件压缩级别 1普通 4 一般  9最高
);
~~~

### 实例化
~~~
 $Backup= new Backup($config);
~~~

### 文件命名规则，请严格遵守（温馨提示）
~~~
$file=['name'=>date('Ymd-His'),'part'=>1]
~~~

### 数据类表列表
~~~
$Backup->dataList();
~~~
### 备份文件列表
~~~
$Backup->fileList();
~~~


### 备份表-单表备份
~~~
 $table="数据库表1";
 $start= $Backup->setFile($file)->backup($table, 0);
 当$start返回0的时候就表示备份成功
~~~
### 备份表-多表备份
~~~
 $tables=array();
 $file=['name'=>date('Ymd-His'),'part'=>1]
 $start=1;
 foreach ($tables as $table){
    $start= $Backup->setFile($file)->backup($table, 0);
 }
 当$start返回0的时候就表示备份成功
~~~
### 导入表
~~~
 $start=0;
 $start= $Backup->import($start);
~~~

### 删除备份文件
~~~
    $Backup->delFile($time);
~~~

### 下载备份文件
~~~
    $Backup->downloadFile($time);
~~~

### 修复表
~~~
    $Backup->repair($tables)
~~~

### 优化表
~~~
    $Backup->optimize($tables)
~~~



### 大数据备份采取措施1
~~~
如果备份数据比较大的情况下，需要修改如下参数
//默认php代码能够申请到的最大内存字节数就是134217728 bytes，如果代码执行的时候再需要更多的内存,根据情况定义指定字节数
memory_limit = 1024M
//默认php代码申请到的超时时间是20s，如果代码执行需要更长的时间，根据代码执行的超时时间定义版本运行超时时间
max_execution_time =1000
~~~

### 大数据备份采取措施2

~~~
    自由设置超时时间。支持连贯操作，该方法主要使用在表备份和还原中，防止备份还原和备份不完整
    //备份
    $time=0//表示不限制超时时间，直到程序结束，(慎用)
    $Backup->setTimeout($time)->setFile($file)->backup($tables[$id], 0);
    //还原
    $Backup->setTimeout($time)->setFile($file)->import($start);
~~~
## 特别感谢
https://github.com/tp5er/tp5-databackup