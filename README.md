# Linux的基础知识
## 文件系统原理
### 文件系统组成
#### inode:
一个文件占用一个Inode,记录文件的属性和文件内容所在block的编号
（区别于目录，建立目录会分配一个inode与至少一个block。 block记录的是目录下所有文件的inode编号以及文件名）
每个indoe大小固定为128字节
#### block:
记录文件的内容，文件大时会占用多个block
inode具有什么信息
权限(read,wirte,excute)
容量
拥有者与群组(owner,group)
建立或状态改变时间(ctime)
最近一次读取时间(atime)
最近修改时间(mtime)
定义文件特性的旗标(flag)
文件真正内容的指向(pointer)
### 硬链接和软连接的区别
#### 硬链接
A是B的硬链接(AB都是文件名)，则A的目录项中的indoe节点号与B的目录项中的inode节点号相同，即一个indoe节点对应两个不同的文件名
两个文件名指向同一个文件，A和B对文件系统来说是完全平等的
不能跨越文件系统、不能对目录进行链接
#### 软连接
A是B的软链接，A的目录项中的Indoe节点号与B目录项中的inode节点号不同，A和B指向两个不同的inode，继而指向不同的数据块
但是A的数据块中存放的只是B的路径名（根据其找到B的目录项）
### 僵尸进程和孤儿进程的区别
#### 孤儿进程
父进程退出，子进程还在运行
孤儿进程被init进程（进程号为1）收养，由init进程对他们完成状态收集工作
#### 僵尸进程
 一个子进程的进程描述符在子进程退出时不会释放，只有父进程通过wait()或waitpid()获取子进程信息才会释放
如果子进程退出，父进程没有调用wait()或waitpid() **，子进程进程描述符仍在系统中**，被称为僵尸进程
僵尸进程通过ps命令显示出来的状态为Z(zombie)
消灭僵尸进程，把父进程杀死，僵尸进程变成孤儿进程，从而被init收养
### linux终端指令
- ps:查看当前运行的进程
- df:查看所有磁盘使用情况
- du:查看当前指定文件或目录占用空间大小
- cat ：一次性显示整个文件的内容
- more 查看文件，按照页显示，只能下一页，百分比显示
- less 查看文件，翻页查看，提供上一页
- tail
- tail -n 1000:显示最后1000行
- tail -n +1000:从1000行开始显示
- tail -f:动态显示文件
- head
- head -n 1000:显示前面1000行
- grep ：文本搜索工具，适合单纯查找匹配文本
- sed -n '5,10p' filename 查看文件第5行到第10行，适合编辑匹配到的文本
- awk:处理文本，逐行读取输入文本，并根据指定的匹配模式进行查找，对文本复杂处理
- sed -n '5,10p' filename 查看文件第5行到第10行，适合编辑匹配到的文本
- find: 全盘扫描,目录结构中搜索文件， 通过-name -type -size进行筛选
- locate：从本机数据库中搜索 ，查不到最近变动的文件，要updatedb命令
- whereis:用于程序名的搜索，只搜索二进制文件
- which: PATH变量指定路径，搜索系统命令位置
- free：显示内存使用情况
- top：动态显示cpu和内存情况
- netstat:监控TCP/IP网络； 观察端口占用情况，加上grep 过滤端口号  netstat -apn|grep 8080
- lsof:查看一个进程打开了哪些文件
- chmod 777   三个数字7分别对应 文件所有者、群组用户、其他用户       7：111   r w x  可读可写可运行
