title: Linux命令
author: Asia Cui
tags:
  - Linux
categories:
  - Linux命令
date: 2019-01-18 09:37:00
---
## 文件操作

*   文件移动、删除、复制

```bash
//查看倒数100行的文件内容
tail -100f <file_name>

//移动a.text文件到/home/app目录
mv a.txt /home/app 

//移动a.txt并把名字改成b.txt
mv a.txt /home/app/b.txt

//把当前目录下的a.txt复制到/home/app 目录下
cp a.txt /home/app

//删除文件夹或文件
rm -rf  /asia 
-r:强制删除文件或目录
-f：删除的时候递归处理，目录下所有子目录里的都删除

```

*   解压和压缩

```bash
//解压tar包，file.tar到当前目录
tar –xzvf file.tar 

//解压zip包，project.war解压到指定文件夹asia
unzip -oq  project.war -d /asia

//压缩tar包单个文件压缩，把file1压缩成一个名字是abc.tar包中
tar -czvf abc.tar file_name
//多个文件压缩
tar -czvf abc.tar file1 file2
//压缩目录,把目录dir1压缩成abc.tar中
tar -czvf abc.tar dir1
//压缩多个目录,把目录dir1和dir2压缩成abc.tar中
tar -czvf abc.tar dir1 dir2

//压缩成zip包,单个文件压缩，把file1压缩成一个名字是abc.zip包中
zip -r abc.zip file1
//多个文件压缩,把文件file1和file2压缩到一个名字是abc.zip包中
zip -r abc.zip file1 file2
//单个目录压缩,把目录dir1压缩成abc.zip中
zip -r abc.zip dir1
//多个目录压缩把目录dir1和dir2压缩成abc.zip中
zip -r abc.zip dir1 dir2

```

*   日志查看

```bash
tail -f test.log //打印最后几行，同时如果有也会滚动显示最新的日志
tail -F test.log //和f区别：例如test.log文件大小达到上限，自动创建一个test.log.1区存储日志，F能自动跟踪到新的文件显示日志
tail -n 100 test.log //显示最后100行
tail -n +100 test.log //从100行开始显示
tail -n -100 test.log //显示100行之前的

cat test.log  //一次显示日志全部内容
cat -n test.log
参数：
-n 或 --number 由 1 开始对所有输出的行数编号
-b 或 --number-nonblank 和 -n 相似，只不过对于空白行不编号
-s 或 --squeeze-blank 当遇到有连续两行以上的空白行，就代换为一行的空白行
-v 或 --show-nonprinting

more test.log  //分页查看，一次显示一页
空格表示显示下一页 ，b返回上一页，退出按q 

less test.log   //一行或一页的查看(中文显示可能有问题)，另带搜索功能
使用上下键控制上一行和下一行，使用空格和b可以一页一页的翻
按’/’+匹配字符，然后回车搜索，再按n表示下一个匹配到的位置

grep 13253638359 test.log  //在test.log文件中搜索这个手机号字段

```

## 权限

```bash
#修改file文件的权限为最高级
修改文件权限： chmod 777 file_name   
#给文件增加执行权限
chmod +X file_name

```

## 系统操作

```bash
//存储查看
df -h

//查看内存使用情况
top
cat /proc/meminfo |more

//查看指定目录的存储大小
du                   不指定目录的话，默认是当前目录下所有目录和目录下的子目录的存储大小，单位是kb
du /logs            查看当前目录下的logs目录的存储大小，如果logs目录下有子目录的话，也会显示出子目录的存储大小
du -sh  *           查看当前目录下的各个文件或目录的大小，不显示子目录，单位是Mb,  *可以替换成指定目录 

//切换用户
su  root //切换到root用户

//查看Java进程
jps  //输出结果是：端口+进程名称

//查看应用对应端口
netstat -nltp

```