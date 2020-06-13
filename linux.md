## 走近Linux系统

### 开机登录开机会

开机会启动许多系统。他们在window叫做“**服务**”（Service），在Linux中叫做“**守护进程**”（daemon）

一般来说，用户登录的方式有三种：

- 命令行登录
- ssh登录
- 图像界面登录

最高权限账户为root，可以操作一切！

### 关机

在linux领域内大多用在服务器上，很少遇到关机的操作。毕竟服务器桑跑一个服务是永无止境的，除非特殊情况下，不得已才会关机。

关机指令为：shutdown

```bash
sync #将数据由内存同步到硬盘中

shutdown #关机指令，你可以man shutdown 来看一下帮助文档。例如你可以运行如下命令关机

shutdown -h 10 #这个命令高数大家，计算机将在10分钟后关机

shutdown -h now #立马关机

shutdown -h 20:55 #系统会在今天20:55关机

shutdown -h +10 #10分钟后关机

shutdown -r now #系统立马重启

shutdown -r —10 #系统在10分钟后重启

reboot #就是重启，等同于shutdown -r now

halt #关闭系统，等同于shutdown -h now 和poweroff
```

最后总结一下，不管是重启还是关闭系统，首先要运行sync命令，把内存中的数据存到磁盘中





### 系统的目录结构

1、一切皆文件

2、根目录/，所有的文件都挂载在这个节点上

登录系统后，在当前命令窗口下输入命令：

```linux
ls /
```

会看到下图：

<img src="../../AppData/Roaming/Typora/typora-user-images/image-20200526115000874.png" alt="image-20200526115000874" style="zoom: 200%;" />





#### 树状目录结构：

![image-20200526115414359](../../AppData/Roaming/Typora/typora-user-images/image-20200526115414359.png)



##### 以下是对这些目录的解释：

- /bin：bin是Binary的缩写，这个目录存放着最经常使用的命令
- /boot：这里存放的是启动linux是使用的一些核心文件，包括一些连接文件以及镜像文件（不要动）
- /dev：dev是Device（设备）的缩写，存放的是Linux的外部设备，在Linux中访问设备的方式和访问文件的方式是相同的
- **/etc：这个目录用来存放所有的系统管理锁需要的配置文件和子目录**
- **/home：用户的主目录，在Linux中，每个用户都有一个自己的目录，一般改目录名是以用户的账号命名的**
- /lib： 这个目录里存放着系统基本的动态连接共享库，其作用类似于Windows里的DLL文件。 （不要动） 
- /lost+found： 这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。（存放 突然关机的一些文件） 
- /media：linux系统会自动识别一些设备，例如U盘、光驱等等，当识别后，linux会把识别的设备 挂载到这个目录下。 
- /mnt：系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在/mnt/上， 然后进入该目录就可以查看光驱里的内容了。（我们后面会把一些本地文件挂载在这个目录下） 
- **/opt：这是给主机额外安装软件所摆放的目录。比如你安装一个ORACLE数据库则就可以放到这个 目录下。默认是空的。** 
- /proc： 这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获 取系统信息。（不用管） 
- **/root：该目录为系统管理员，也称作超级权限者的用户主目录。** 
- /sbin：s就是Super User的意思，这里存放的是系统管理员使用的系统管理程序。
-  /srv：该目录存放一些服务启动之后需要提取的数据。
-  /sys：这是linux2.6内核的一个很大的变化。该目录下安装了2.6内核中新出现的一个文件系统 sysfs 
- **/tmp：这个目录是用来存放一些临时文件的。用完即丢的文件，可以放在这个目录下，安装包！**
- **/usr：这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似于windows 下的program ﬁles目录。** 
- /usr/bin： 系统用户使用的应用程序。
-  /usr/sbin： 超级用户使用的比较高级的管理程序和系统守护程序。Super 
- /usr/src： 内核源代码默认的放置目录。
-  **/var：这个目录中存放着在不断扩充着的东西，我们习惯将那些经常被修改的目录放在这个目录 下。包括各种日志文件。**
-  /run：是一个临时文件系统，存储系统启动以来的信息。当系统重启时，这个目录下的文件应该被 删掉或清除。 
- **/www：存放服务器网站相关的资源，环境，网站的项目**

![image-20200526151246424](../../AppData/Roaming/Typora/typora-user-images/image-20200526151246424.png)



## 常用的基本命令

绝对路径，相对路径

绝对路径的全称：C:\ProgramData\360safe\xxx.xx



### cd  ：切换目录命令

- ./  ：当前目录

- cd ..  ：返回上一级目录

![image-20200526152953829](../../AppData/Roaming/Typora/typora-user-images/image-20200526152953829.png)



### ls（列出目录）

在Linux中ls可能是最常被使用的

- -a参数：all，查看所有的文件，包括隐藏文件
- -l参数：列出所有的文件，包含文件的属性和权限，没有隐藏文件

所有的Linux命令可以组合使用

![image-20200526152710917](../../AppData/Roaming/Typora/typora-user-images/image-20200526152710917.png)



### pwd 显示当前用户所在的目录

```bash
[root@localhost .config]# pwd
/home/hc/.config
```

### mkdir 创建一个目录

- -p：创建多级目录

```bash
[root@localhost hc]# mkdir test
[root@localhost hc]# ls
test
[root@localhost hc]# mkdir -p test1/test2/test3
[root@localhost hc]# ls
test  test1
[root@localhost hc]# cd test1
[root@localhost test1]# cd test2/
[root@localhost test2]# cd t
-bash: cd: t: 没有那个文件或目录
[root@localhost test2]# cd test3/
[root@localhost test3]# ls
[root@localhost test3]# ^C
[root@localhost test3]# 
```



### rmdir 删除目录

rmdir 只能删除空的目录，如果下面存在文件，需要先删除文件，递归删除多个目录 -p 参数即可

- -p ：递归删除多个目录

![image-20200526154414754](../../AppData/Roaming/Typora/typora-user-images/image-20200526154414754.png)



### cp 复制文件或者目录

```bash
[root@localhost home]# cp apache-tomcat-9.0.35.tar.gz hc  #拷贝文件至目录
[root@localhost home]# ls
apache-tomcat-9.0.35.tar.gz  hc  jdk-8u251-linux-x64.rpm
[root@localhost home]# cd hc
[root@localhost hc]# ls
apache-tomcat-9.0.35.tar.gz  test  test1
[root@localhost hc]# 
[root@localhost home]# cp apache-tomcat-9.0.35.tar.gz hc  #如果文件重复，就选择覆盖（y）或者放弃（n）
cp：是否覆盖"hc/apache-tomcat-9.0.35.tar.gz"？ 
```



### rm 移除文件或目录

- -f：忽略不存在的文件，不会出现警告，强制删除
- -r：递归删除目录！
- -i：互动， 删除时询问是否删除

```bash
rm -rf    #递归删除所有的文件
```



### mv 移动文件或目录

- -f：强制移动
- -u：只替换以及更新过的文件 

```bash
mv hc hc2 #重命名文件夹
```



## 基本属性 

十个字母 ；1 类型

> 看懂文件属性  root   hc

Linux系统是一种典型的多用户系统，不同的用户处于不同的地位，拥有不同的权限。为了保护系统的安 全性，Linux系统对不同的用户访问同一文件（包括目录文件）的权限做了不同的规定。

 在Linux中我们可以使用 **ll** 或者 **ls –l** 命令来显示一个文件的属性以及文件所属的用户和组， 如：

![image-20200526160657706](../../AppData/Roaming/Typora/typora-user-images/image-20200526160657706.png)

实例中，boot文件的第一个属性用"d"表示。"d"在Linux中代表该文件是一个目录文件。

在Linux中第一个字符代表这个文件是目录、文件或链接文件等等：

- **当为[ d ]则是目录 当为[ - ]则是文件；**
-  **若是[ l ]则表示为链接文档 ( link ﬁle )；**
-  **若是[ b ]则表示为装置文件里面的可供储存的接口设备 ( 可随机存取装置 )；**
-  若是[ c ]则表示为装置文件里面的串行端口设备，例如键盘、鼠标 ( 一次性读取装置 )。

 接下来的字符中，以三个为一组，且均为『rwx』 的三个参数的组合。

其中，[ r ]代表可读(read)、[ w ]代表可写(write)、[ x ]代表可执行(execute)。

 要注意的是，这三个权限的位置不会改变，如果没有权限，就会出现减号[ - ]而已。

 每个文件的属性由左边第一部分的10个字符来确定（如下图）：

![image-20200526160905300](../../AppData/Roaming/Typora/typora-user-images/image-20200526160905300.png)

从左至右用0-9这些数字来表示。

第0位确定文件类型，第1-3位确定属主（该文件的所有者）拥有该文件的权限。第4-6位确定属组（所有 者的同组用户）拥有该文件的权限，第7-9位确定其他用户拥有该文件的权限。

其中：

第1、4、7位表示读权限，如果用"r"字符表示，则有读权限，如果用"-"字符表示，则没有读权限；

第2、5、8位表示写权限，如果用"w"字符表示，则有写权限，如果用"-"字符表示没有写权限；

第3、6、9位表示可执行权限，如果用"x"字符表示，则有执行权限，如果用"-"字符表示，则没有执行权 限。

对于文件来说，它都有一个特定的所有者，也就是对该文件具有所有权的用户。

同时，在Linux系统中，用户是按组分类的，一个用户属于一个或多个组。

文件所有者以外的用户又可以分为文件所有者的同组用户和其他用户。

因此，Linux系统按文件所有者、文件所有者同组用户和其他用户来规定了不同的文件访问权限。

在以上实例中，boot 文件是一个目录文件，属主和属组都为 root。

### 修改文件属性

#### 1、chgrp：更改文件属组

```bash
chgrp [-R] 属组名 文件名
```

-R：递归更改文件属组，就是在更改某个目录文件的属组时，如果加上-R的参数，那么该目录下的所有 文件的属组都会更改。

#### 2、chown：更改文件属主，也可以同时更改文件属组

```bash
chown [–R] 属主名 文件名
chown [-R] 属主名：属组名 文件名
```

#### 3、chmod：更改文件9个属性（必须要掌握）

你没有权限操作此文件！

```bash
chmod [-R] xyz 文件或目录
```

Linux文件属性有两种设置方法，一种是数字（常用的是数字），一种是符号。

Linux文件的基本权限就有九个，分别是owner/group/others三种身份各有自己的read/write/execute 权限。

先复习一下刚刚上面提到的数据：文件的权限字符为：『-rwxrwxrwx』， 这九个权限是三个三个一组 的！其中，我们可以使用数字来代表各个权限，各权限的分数对照表如下：

```bash
r:4     		w:2         x:1
可读可写不可执行    rw-    6 
可读可写不课执行    rwx    7
chomd 777  文件赋予所有用户可读可执行！
```

每种身份(owner/group/others)各自的三个权限(r/w/x)分数是需要累加的，例如当权限为：[-rwxrwx---] 分数则是：

- owner = rwx = 4+2+1 = 7 
- group = rwx = 4+2+1 = 7 
- others= --- = 0+0+0 = 0

```bash
chmod 770 filename
```

可以自己下去多进行测试！

![image-20200526162153016](../../AppData/Roaming/Typora/typora-user-images/image-20200526162153016.png)



### 文件内容查看 

我们会经常使用到文件查看！
Linux系统中使用以下命令来查看文件的内容：

#### cat

由第一行开始显示文件内容，用来读文章，或者读取配置文件啊，都使用cat名

#### tac

从后一行开始显示，可以看出 tac 是 cat 的倒着写！

![image-20200526162932050](../../AppData/Roaming/Typora/typora-user-images/image-20200526162932050.png)

#### nl 

显示的时候，顺道输出行号！  看代码的时候，希望显示行号！ 常用

![image-20200526162943587](../../AppData/Roaming/Typora/typora-user-images/image-20200526162943587.png)

#### more 

一页一页的显示文件内容，带余下内容的（空格代表翻页，enter 代表向下看一行， :f 行 号）

![image-20200526162955461](../../AppData/Roaming/Typora/typora-user-images/image-20200526162955461.png)

#### less

与 more 类似，但是比 more 更好的是，他可以往前翻页！ （空格下翻页，pageDown， pageUp键代表翻动页面！退出 q 命令，查找字符串 /要查询的字符向下查询，向上查询使用？要 查询的字符串，n 继续搜寻下一个，N 上寻找！）

![image-20200526163006759](../../AppData/Roaming/Typora/typora-user-images/image-20200526163006759.png)

#### head

只看头几行 通过 -n 参数来控制显示几行！

![image-20200526163015004](../../AppData/Roaming/Typora/typora-user-images/image-20200526163015004.png)

#### tail 

只看尾巴几行 -n 参数 要查看几行！

![image-20200526163024777](../../AppData/Roaming/Typora/typora-user-images/image-20200526163024777.png)

你可以使用 man [命令]来查看各个命令的使用文档，如 ：man cp。

### 网络配置目录：

ifconﬁg 命令查看网络配置！

 **cd /etc/sysconfig/network-scripts**

![image-20200526163330001](../../AppData/Roaming/Typora/typora-user-images/image-20200526163330001.png)



### 拓展：Linux 链接的概念（了解即可！）

Linux的链接分为两种：硬链接、软链接！

**硬链接**：A---B，假设B是A的硬链接，那么他们两个指向了同一个文件！允许一个文件拥有多个路径，用 户可以通过这种机制建立硬链接到一些重要文件上，防止误删！

**软链接**： 类似Window下的快捷方式，删除的源文件，快捷方式也访问不了！

**创建连接  ln 命令！**

touch 命令创建文件！

echo 输入字符串,也可以输入到文件中！

```bash
[root@kuangshen home]# touch f1         # 创建一个f1文件
[root@kuangshen home]# ls
f1  install.sh  kuangshen  www
[root@kuangshen home]# ln f1 f2         # 创建一个硬链接 f2
[root@kuangshen home]# ls f1  f2  install.sh  kuangshen  www
[root@kuangshen home]# ln -s f1 f3       # 创建一个软链接（符号连接） f3
[root@kuangshen home]# ls f1  f2  f3  install.sh  kuangshen  www
[root@kuangshen home]# ll
total 28
-rw-r--r-- 2 root root     0 Mar 24 20:17 f1
-rw-r--r-- 2 root root     0 Mar 24 20:17 f2
lrwxrwxrwx 1 root root     2 Mar 24 20:18 f3 -> f1
-rw-r--r-- 1 root root 20078 Mar  4 16:48 install.sh
drwxr-xr-x 2 root root  4096 Mar 23 21:25 kuangshen
drwxrw---x 2 www  www   4096 Mar 23 12:46 www
[root@kuangshen home]# echo "i love kuangshen" >>f1   # 给f1文件中写入一些字符串！
[root@kuangshen home]# ls f1  f2  f3  install.sh  kuangshen  www
[root@kuangshen home]# clear
[root@kuangshen home]# ll
total 36
-rw-r--r-- 2 root root    17 Mar 24 20:19 f1
-rw-r--r-- 2 root root    17 Mar 24 20:19 f2
lrwxrwxrwx 1 root root     2 Mar 24 20:18 f3 -> f1
-rw-r--r-- 1 root root 20078 Mar  4 16:48 install.sh
drwxr-xr-x 2 root root  4096 Mar 23 21:25 kuangshen
drwxrw---x 2 www  www   4096 Mar 23 12:46 www
[root@kuangshen home]# cat f1   # 查看f1 i love kuangshen
[root@kuangshen home]# cat f2   # 查看f2 i love kuangshen
[root@kuangshen home]# cat f3   # 查看f3 i love kuangshen 
```

删除f1之后，查看f2 和 f3 的区别





### Vim 编辑器 

#### 什么是Vim编辑器

vim 通过一些插件可以实现和IDE一样的功能！

Vim是从 vi 发展出来的一个文本编辑器。代码补完、编译及错误跳转等方便编程的功能特别丰富，在程 序员中被广泛使用。尤其是Linux中，必须要会使用Vim（查看内容，编辑内容，保存内容！）

简单的来说， vi 是老式的字处理器，不过功能已经很齐全了，但是还是有可以进步的地方。

vim 则可以说是程序开发者的一项很好用的工具。

所有的 Unix Like 系统都会内建 vi 文书编辑器，其他的文书编辑器则不一定会存在。

 连 vim 的官方网站 (http://www.vim.org) 自己也说 vim 是一个程序开发工具而不是文字处理软件。
vim 键盘图：

![image-20200526171628439](../../AppData/Roaming/Typora/typora-user-images/image-20200526171628439.png)

#### 三种使用模式

基本上 vi/vim 共分为三种模式，分别是**命令模式**（Command mode），**输入模式（**Insert mode）和 **底线命令模式**（Last line mode）。这三种模式的作用分别是：

##### 命令模式：

用户刚刚启动 vi/vim，便进入了命令模式。

![image-20200526171742539](../../AppData/Roaming/Typora/typora-user-images/image-20200526171742539.png)

此状态下敲击键盘动作会被Vim识别为命令，而非输入字符。比如我们此时按下i，并不会输入一个字 符，i被当作了一个命令。

以下是常用的几个命令：

- i 切换到输入模式，以输入字符。
- x 删除当前光标所在处的字符。 :
- : 切换到底线命令模式，以在底一行输入命令。 如果是编辑模式，需要先退出编辑模式！ESC

若想要编辑文本：启动Vim，进入了命令模式，按下i，切换到输入模式。

命令模式只有一些基本的命令，因此仍要依靠底线命令模式输入更多命令。

##### 输入模式：

在命令模式下按下i就进入了输入模式。

![image-20200526171926799](../../AppData/Roaming/Typora/typora-user-images/image-20200526171926799.png)

在输入模式中，可以使用以下按键：

- 字符按键以及Shift组合，输入字符
- ENTER，回车键，换行
- BACK SPACE，退格键，删除光标前一个字符
- DEL，删除键，删除光标后一个字符 方向键，在文本中移动光标
- HOME/END，移动光标到行首/行尾
- Page Up/Page Down，上/下翻页
- Insert，切换光标为输入/替换模式，光标将变成竖线/下划线
- ESC，退出输入模式，切换到命令模式

底线命令模式

在命令模式下按下:（英文冒号）就进入了底线命令模式。光标就移动到了底下，就可以在这里输入一 些底线命令了！

![image-20200526172102662](../../AppData/Roaming/Typora/typora-user-images/image-20200526172102662.png)

底线命令模式可以输入单个或多个字符的命令，可用的命令非常多。

在底线命令模式中，基本的命令有（已经省略了冒号）：wq 

- q 退出程序
- w 保存文件

![image-20200526172112127](../../AppData/Roaming/Typora/typora-user-images/image-20200526172112127.png)

按ESC键可随时退出底线命令模式。

![image-20200526172131398](../../AppData/Roaming/Typora/typora-user-images/image-20200526172131398.png)

简单的说，我们可以将这三个模式想成底下的图标来表示：

![image-20200526172141767](../../AppData/Roaming/Typora/typora-user-images/image-20200526172141767.png)

##### 完整的演示说明

新建或者编辑文件，按 i 进入编辑模式，编写内容，编写完成后退出编辑模式，esc，退出之后进入底线 命令模式 ： wq 保存退出！  

##### Vim 按键说明

除了上面简易范例的 i, Esc, :wq 之外，其实 vim 还有非常多的按键可以使用。

###### **第一部分：一般模式可用的光标移动、复制粘贴、搜索替换等**

| 移动光标的方法      |                                                              |
| ------------------- | ------------------------------------------------------------ |
| h 或 向左箭头 键(←) | 光标向左移动一个字符                                         |
| j 或 向下箭头键 (↓) | 光标向下移动一个字符                                         |
| k 或 向上箭头 键(↑) | 光标向上移动一个字符                                         |
| l 或 向右箭头键 (→) | 光标向右移动一个字符                                         |
| [Ctrl] + [f]        | 屏幕『向下』移动一页，相当于 [Page Down]按键 (常用)          |
| [Ctrl] + [b]        | 屏幕『向上』移动一页，相当于 [Page Up] 按键 (常用)           |
| [Ctrl] + [d]        | 屏幕『向下』移动半页                                         |
| [Ctrl] + [u]        | 屏幕『向上』移动半页                                         |
| +                   | 光标移动到非空格符的下一行                                   |
| -                   | 光标移动到非空格符的上一行，配置文件中空格较多！             |
| **数字 < space>**   | 那个 n 表示『数字』，例如 20 。快捷切换光标， 数字 + 空格    |
| 0 或功能键 [Home]   | 这是数字『 0 』：移动到这一行的前面字符处 (常用)             |
| $ 或功能键 [End]    | 移动到这一行的后面字符处(常用)                               |
| H                   | 光标移动到这个屏幕的上方那一行的第一个字符                   |
| M                   | 光标移动到这个屏幕的中央那一行的第一个字符                   |
| G                   | 移动到这个档案的后一行(常用)                                 |
| L                   | 光标移动到这个屏幕的下方那一行的第一个字符                   |
| 数字< Enter>        | n 为数字。光标向下移动 n 行(常用)                            |
| /word               | 向光标之下寻找一个名称为 word 的字符串。例如要在档案内搜寻 vbird 这个字符串， 就输入 /vbird 即可！(常用) |
| n                   | 这个 n 是英文按键。代表重复前一个搜寻的动作。举例来说， 如果刚刚我们执行 /vbird 去向下搜寻 vbird 这个字符串，则按下 n 后，会向下继续搜寻下一个名称为 vbird 的字 符串。如果是执行 ?vbird 的话，那么按下 n 则会向上继续搜寻名称为 vbird 的字符串！ |
| N                   | 这个 N 是英文按键。与 n 刚好相反，为『反向』进行前一个搜寻动作。例如 /vbird 后，按下 N 则表示『向上』搜寻 vbird。 |
| u                   | 复原前一个动作。(常用)                                       |

###### 第二部分：一般模式切换到编辑模式的可用的按钮说明 

| i, I  | 进入输入模式(Insert mode)：i 为『从目前光标所在处输入』， I 为『在目前所在 行的第一个非空格符处开始输入』。(常用) |
| ----- | ------------------------------------------------------------ |
| [Esc] | 退出编辑模式，回到一般模式中(常用)                           |

###### 第三部分：一般模式切换到指令行模式的可用的按钮说明

| :wq                                   | 储存后离开，若为 :wq! 则为强制储存后离开 (常用)    |
| ------------------------------------- | -------------------------------------------------- |
| :set nu 设置行号，代码中经常 会使用！ | 显示行号，设定之后，会在每一行的前缀显示该行的行号 |



## 账号管理 

你一般在公司中，用的应该都不是 root 账户！

### 简介

Linux系统是一个多用户多任务的分时操作系统，任何一个要使用系统资源的用户，都必须首先向系统管 理员申请一个账号，然后以这个账号的身份进入系统。

用户的账号一方面可以帮助系统管理员对使用系统的用户进行跟踪，并控制他们对系统资源的访问；另 一方面也可以帮助用户组织文件，并为用户提供安全性保护。

每个用户账号都拥有一个唯一的用户名和各自的口令。

用户在登录时键入正确的用户名和口令后，就能够进入系统和自己的主目录。

实现用户账号的管理，要完成的工作主要有如下几个方面：

- 用户账号的添加、删除与修改。 
- 用户口令的管理。
-  用户组的管理。



### 用户账号的管理

用户账号的管理工作主要涉及到用户账号的添加、修改和删除。

添加用户账号就是在系统中创建一个新账号，然后为新账号分配用户号、用户组、主目录和登录Shell等 资源。

属主，属组

#### useradd 命令 添加用户

useradd -选项 用户名 

-m： 自动创建这个用户的主目录 /home/qinjiang 

-G : 给用户分配组！

```bash
[root@kuangshen home]# useradd -m qinjiang   创建一个用户！
[root@kuangshen home]# ls
install.sh  kuangshen  qinjiang  www
```

理解一下本质：Linux中一切皆文件，这里的添加用户说白了就是往某一个文件中写入用户的信息了！ /etc/passwd

#### 删除用户 userdel 

userdel -r qinjiang  删除用户的时候将他的目录页一并删掉！

```bash
[root@kuangshen home]# userdel -r qinjiang
[root@kuangshen home]# ls
install.sh  kuangshen  www
```

#### 修改用户  usermod 

修改用户 usermod  对应修改的内容  修改那个用户

```
[root@kuangshen home]# usermod -d /home/233 qinjiang
```

修改完毕之后查看配置文件即可！

#### 切换用户！

root用户

![image-20200526231544792](../../AppData/Roaming/Typora/typora-user-images/image-20200526231544792.png)

1.切换用户的命令为：**su username** 【username是你的用户名哦】

2.从普通用户切换到root用户，还可以使用命令：**sudo su**

![image-20200526231611548](../../AppData/Roaming/Typora/typora-user-images/image-20200526231611548.png)

3.在终端输入exit或logout或使用快捷方式ctrl+d，可以退回到原来用户，其实ctrl+d也是执行的exit命令

![image-20200526231630989](../../AppData/Roaming/Typora/typora-user-images/image-20200526231630989.png)

4.在切换用户时，如果想在切换用户之后使用新用户的工作环境，可以在su和username之间加-，例 如：【su - root】

$表示普通用户

#表示超级用户，也就是root用户

有的小伙伴在阿里云买完服务器后，主机名是一个随机字符串！

![image-20200526231708571](../../AppData/Roaming/Typora/typora-user-images/image-20200526231708571.png)



#### 用户的密码设置问题！

我们一般通过root创建用户的时候！要配置密码！

Linux上输入密码是不会显示的，你正常输入就可以了，并不是系统的问题！

在公司中，你们一般拿不到公司服务器的 root 权限，都是一些分配的账号！

如果是超级用户的话：

```bash
passwd username：
new password：
re password
```

如果是普通用户：

```
passwd
(current) UNIX password:
new password： # 密码不能太过于简单！
re password
```

#### 锁定账户！

root，比如张三辞职了！冻结这个账号，一旦冻结，这个人就登录不上系统了！

```bash
passwd -l qinjiang # 锁定之后这个用户就不能登录了！
passwd -d qinjiang # 没有密码也不能登录！
```

![image-20200526231918140](../../AppData/Roaming/Typora/typora-user-images/image-20200526231918140.png)

在公司中，你一般触及不到 root 用户！作为一个开发一般你拿不到!

这以上的基本命令，大家必须要掌握！但是自己玩的时候可以使用来学习！Linux是一个多用户的系统！ 



## 用户组管理 

属主、属组

每个用户都有一个用户组，系统可以对一个用户组中的所有用户进行集中管理（开发、测试、运维、 root）。不同Linux 系统对用户组的规定有所不同，如Linux下的用户属于与它同名的用户组，这个用户 组在创建用户时同时创建。

用户组的管理涉及用户组的添加、删除和修改。组的增加、删除和修改实际上就是对/etc/group文件的 更新。

### groupadd 创建一个用户组  

![image-20200527131928414](../../AppData/Roaming/Typora/typora-user-images/image-20200527131928414.png)

创建完用户组后可以得到一个组的id，这个id是可以指定的！ **-g 520** ， 若果不指定就是自增1

### groupdel  删除用户组    

![image-20200527132009770](../../AppData/Roaming/Typora/typora-user-images/image-20200527132009770.png)

###  groupmod -g -n 修改用户组的权限信息和名字

![image-20200527132057708](../../AppData/Roaming/Typora/typora-user-images/image-20200527132057708.png)



### 用户如果要切换用户组怎么办呢？

```bash
# 登录当前用户  qinjiang 
$ newgrp root
```

### 拓展：文件的查看！（了解即可）

/etc/passwd 

```bash
用户名:口令(登录密码，我们不可见):用户标识号:组标识号:注释性描述:主目录:登录Shell
```

这个文件中的每一行都代表这一个用户，我们可以从这里看出这个用户的主目录在那里，可以看到属于 哪一个组！

登录口令：把真正的加密后的用户口令字存放到/etc/shadow文件中，保证我们密码的安全性！

用户组的所有信息都存放在/etc/group文件中。



## 磁盘管理 

### df （列出文件系统整体的磁盘使用量）

![image-20200527133416341](../../AppData/Roaming/Typora/typora-user-images/image-20200527133416341.png)

### du（检查磁盘空间使用量！）

![image-20200527133432026](../../AppData/Roaming/Typora/typora-user-images/image-20200527133432026.png)

![image-20200527133442331](../../AppData/Roaming/Typora/typora-user-images/image-20200527133442331.png)

![image-20200527133452896](../../AppData/Roaming/Typora/typora-user-images/image-20200527133452896.png)



### Mac 或者想使用Linux 挂载我们的一些本地磁盘或者文件！

挂载：mount

![image-20200527133532743](../../AppData/Roaming/Typora/typora-user-images/image-20200527133532743.png)

卸载：umount -f  [挂载位置] 强制卸载  

除了这个之外，以后我们安装了JDK ，其实可以使用java中的一些命令来查看信息！ 



## 进程管理 

Linux中一切皆文件

**（文件：读写执行（查看，创建，删除，移动，复制，编辑），权限（用户、用户组）。系统：（磁 盘，进程））**

对于我们开发人员来说，其实Linux更多偏向于使用即可！

### 基本概念！

1、在Linux中，每一个程序都是有自己的一个进程，每一个进程都有一个id号！

2、每一个进程呢，都会有一个父进程！

3、进程可以有两种存在方式：前台！后台运行！

4、一般的话服务都是后台运行的，基本的程序都是前台运行的！

### 命令

#### ps ：查看当前系统中正在执行的各种进程的信息！

ps -xx ：

- -a  显示当前终端运行的所有的进程信息（当前的进程一个）

- -u 以用户的信息显示进程 

- -x 显示后台运行进程的参数！ 

```bash
# ps -aux 查看所有的进程 
ps -aux|grep mysql 

# |  在Linux这个叫做管道符    A|B 
# grep 查找文件中符合条件的字符串！
```

对于我们来说，这里目前只需要记住一个命令即可   ps -xx|grep 进程名字！ 过滤进程信息！

#### ps -ef：可以查看到父进程的信息

```bash
ps -ef|grep mysql # 看父进程我们一般可以通过目录树结构来查看！
# 进程树！
pstree -pu
	-p  显示父id
    -u  显示用户组
```

![image-20200527134918970](../../AppData/Roaming/Typora/typora-user-images/image-20200527134918970.png)

结束进程：杀掉进程，等价于window结束任务！

#### kill -9 进程的id

但是啊，我们平时写的一个Java代码死循环了，可以选择结束进程！杀进程

```bash
kill -9 进程的id
```

表示强制结束该进程！



将Java程序打包发的时候讲解！ **nohup** ，代表后台执行程序