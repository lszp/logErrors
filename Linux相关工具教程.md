# 1. tmux

## tmux是什么

tmux (terminal multiplexer) 是Linux上的终端复用神器，可从一个屏幕上管理多个终端（准确说是伪终端）。使用该工具，用户可以连接或断开会话，而保持终端在后台运行。

## 基本结构

tmux结构包括会话（session）、窗口（window）和窗格（pane）三部分，会话实质是伪终端集合，每个窗格表示一个伪终端，多个窗格展现在一个屏幕上，这一屏幕就是窗口，基本结构及状态信息如下图所示：

<img src="G:\errors&amp;resolutions_log\tmux.png" style="zoom:80%;" />

## tmux基本操作

基本的操作无非就是对会话、窗口、窗格进行管理，包括创建、关闭、重命名、连接、分离、选择等等。

一般使用命令和快捷键进行操作，可在系统shell终端和tmux命令模式（类似vim的命令模式）下使用命令，或者在tmux终端使用快捷键。

tmux默认的快捷键前缀是**Ctrl+b**(下文用**prefix**指代)，按下前缀组合键后松开，再按下命令键进行快捷操作，比如使用**prefix d**分离会话（应该写作**prefix d**而不是**prefix+d，**因为**d**键不需要与**prefix**同时按下）。

快捷键可以自定义，比如将前缀改为**Ctrl+a**，但需要保留shell默认的**Ctrl+a**快捷键，按如下所示修改~/.tmux.conf文件：

```
1 set-option -g prefix C-a
2 unbind-key C-b
3 bind-key C-a send-prefix
4 bind-key R source-file ~/.tmux.conf \; display-message "~/.tmux.conf reloaded."
```

现在已将原先的**Ctrl+a**用**prefix Ctrl+a**取代，即需要按两次**Ctrl+a**生效。

第4行的作用是使用**prefix r**重新加载配置文件，并输出提示，否则需要关闭会话后配置文件才能生效，也可手动加载配置文件，在tmux终端输入"**prefix :"**进入命令模式，用**source-file**命令加载配置文件。**注意，将多个命令写在一起作为命令序列时，命令之间要用空格和分号分隔。** 

## 会话管理

### 常用命令

**tmux new**　　创建默认名称的会话（在tmux命令模式使用**new**命令可实现同样的功能，其他命令同理，后文不再列出tmux终端命令）

**tmux new -s mysession**　　创建名为mysession的会话

**tmux ls**　　显示会话列表

**tmux a**　　连接上一个会话

**tmux a -t mysession**　　连接指定会话

**tmux rename -t s1 s2**　　重命名会话s1为s2

**tmux kill-session**　　关闭上次打开的会话

**tmux kill-session -t s1**　　关闭会话s1

**tmux kill-session -a -t s1**　　关闭除s1外的所有会话

**tmux kill-server**　　关闭所有会话

### 常用快捷键

**prefix s**　　列出会话，可进行切换

**prefix $**　　重命名会话

**prefix d**　　分离当前会话

**prefix** **D**　　分离指定会话

## 窗口管理

**prefix c**　　创建一个新窗口

**prefix ,**　　重命名当前窗口

**prefix w**　　列出所有窗口，可进行切换

**prefix n**　　进入下一个窗口

**prefix p**　　进入上一个窗口

**prefix l**　　进入之前操作的窗口

**prefix 0~9**　　选择编号0~9对应的窗口

**prefix .**　　修改当前窗口索引编号

**prefix '**　　切换至指定编号（可大于9）的窗口

**prefix f**　　根据显示的内容搜索窗格

**prefix &**　　关闭当前窗口

## 窗格管理

**prefix %**　　水平方向创建窗格

**prefix "**　　垂直方向创建窗格

**prefix Up|Down|Left|Right**　　根据箭头方向切换窗格

**prefix q**　　显示窗格编号

**prefix o**　　顺时针切换窗格

**prefix }**　　与下一个窗格交换位置

**prefix {**　　与上一个窗格交换位置

**prefix x**　　关闭当前窗格

**prefix space(空格键)**　　重新排列当前窗口下的所有窗格

**prefix !**　　将当前窗格置于新窗口

**prefix Ctrl+o**　　逆时针旋转当前窗口的窗格

**prefix t**　　在当前窗格显示时间

**prefix z**　　放大当前窗格(再次按下将还原)

**prefix i**　　显示当前窗格信息

## 其他命令

**tmux list-key**　　列出所有绑定的键，等同于**prefix ?**

**tmux list-command**　　列出所有命令

# 2.vscode

## 2.1 配置python开发环境

###  1）准备工作

安装anaconda、git、VSCode

### 2) 安装扩展

安装Chinese (Simplified)中文简体语言包

安装Python扩展、Path Intellisense、vscode-python-docstring以及Settings Sync用于同步配置

安装完Python扩展后， 按

按Ctrl+Shift+P，输入python→选择解析器，会显示所有环境（conda、venv等），可以选择任何一个作为解析器

![vscode-python conda环境选择](https://code.visualstudio.com/assets/docs/python/environments/interpreters-list.png)

### 3) 配置文件与内置终端设置

对于编辑器、窗口以及扩展等，VSCode都提供了默认配置，用户也可**自定义配置**，具体操作如下。

依次点击 文件→首选项→设置，或者直接`Ctrl+,`打开配置界面，通过右上角的按钮切换到 配置文件（见下图），左侧为默认配置，右侧为用户自定义配置，也可为当前工作区专门配置（会在当前文件夹下创建.vscode/settings.json文件）。

内置终端修改：默认内置终端为powershell，这里改为git bash。在左侧的默认配置项上点击“铅笔”图标可以将当前项复制到右侧进行修改，这里将**内置终端**修改为**git bash**，修改"terminal.integrated.shell.windows"和"terminal.integrated.shellArgs.windows"，如下图所示。

![vscode-settings](https://s2.ax1x.com/2019/01/05/F7Quyn.png)

修改完之后重启VSCode，会发现内置终端变成了bash，就可以使用`ll`等命令、运行sh脚本了，如下图所示。

![F7l7E6.png](https://s2.ax1x.com/2019/01/05/F7l7E6.png)

但是还存在一个问题，cmd激活conda环境的命令是`activate envname`，bash激活conda环境的命令为`source activate envname`，vscode在调试python时会自动调用`activate envname`来激活相应的环境，将默认终端换为bash后，会导致**环境激活不成功**，修改方法是在bash的配置文件中为`source activate`设置别名，具体如下：

- 打开"C:\Program Files\Git\etc\bash.bashrc"

- 在文件末尾加入如下两行：

  alias activate=". $(which activate)" 

  alias deactivate=". $(which deactivate)"

重启vscode就可以了。

### 4）高级调试配置

即launch.json文件，在调试时，通常需要指定命令行参数或者临时环境变量等，这些都可以在launch.json文件中设置：

高级调试配置需要通过VSCode打开文件夹，而不是直接打开文件，具体做法是：

- 在待调试文件所在的文件夹**右键**，选择 **open with code**
- **调试→添加配置**，会在当前文件夹下生成.vscode文件夹以及**.vscode/launch.json**文件（与工作去设置文件是同一文件夹）

打开launch.json文件，默认配置如下

```python
{
    "name": "Python: Current File (Integrated Terminal)",
    "type": "python",
    "request": "launch",
    "program": "${file}",
    "console": "integratedTerminal"
},
```

默认调试当前文件，默认调试终端为Integrated Terminal，即在vscode内置终端中调试。也可指定要launch的文件，直接修改上面"program"的值，将${file}替换为要调试的文件。

此外，还可添加其他配置项，常用的配置选项如下：

- `env`：指定环境变量
- `envFile`：指定环境变量定义文件，参见[Environment variable definitions file](https://code.visualstudio.com/docs/python/environments#_environment-variable-definitions-file)查看文件格式
- `args`：指定命令行参数

```python
"env": {
    "CUDA_VISIBLE_DEVICES": "0"
},
"args": [
    "--port", "1593"
]
```



## 2.2  SSH Remote】failed to create hard link '/home/*/.vscode-server/bin/*/*' file exists

1. vscode 连接服务器时，循环要求输入密码，并显示如下错误信息：

   <img src="C:\Users\zp\AppData\Roaming\Typora\typora-user-images\image-20200509164326895.png" alt="image-20200509164326895" style="zoom:67%;" />

2. 解决方案

      解决方案是删除对应硬链接（上图第一段标绿链接）下的同名文件与同名target文件，如下红框部分（图示服务器不同，所以路径有所区别）

      <img src="C:\Users\zp\AppData\Roaming\Typora\typora-user-images\image-20200509164546550.png" alt="image-20200509164546550" style="zoom:80%;" />

      
      
      删除后，问题解决！

## 2.3显示空格和tab符号

### 打开setting,在搜索框中输入renderControlCharacters,选中勾选框,即可显示tab

### 在搜索框中输入renderWhitespace,选择all,即可显示空格.

## 2.4 VS Code 快捷键 

[参考博客](https://juejin.im/post/5cb87c6e6fb9a068a03af93a)

### 2.4.1 工作区快捷键

<img src="C:\Users\zp\AppData\Roaming\Typora\typora-user-images\image-20200623161757144.png" alt="image-20200623161757144" style="zoom:80%;" />

### 2.4.2 跳转操作

<img src="C:\Users\zp\AppData\Roaming\Typora\typora-user-images\image-20200623162155698.png" alt="image-20200623162155698" style="zoom:80%;" />

### 2.4.3 移动光标

<img src="C:\Users\zp\AppData\Roaming\Typora\typora-user-images\image-20200623162702846.png" alt="image-20200623162702846" style="zoom:80%;" />

### 2.4.4 编辑操作

<img src="C:\Users\zp\AppData\Roaming\Typora\typora-user-images\image-20200623162744333.png" alt="image-20200623162744333" style="zoom:80%;" />

### 2.4.5 多光标编辑

<img src="C:\Users\zp\AppData\Roaming\Typora\typora-user-images\image-20200623162840455.png" alt="image-20200623162840455" style="zoom:80%;" />

### 2.4.6 删除操作

<img src="C:\Users\zp\AppData\Roaming\Typora\typora-user-images\image-20200623162932400.png" alt="image-20200623162932400" style="zoom:80%;" />

### 2.4.7 编程语言相关

<img src="C:\Users\zp\AppData\Roaming\Typora\typora-user-images\image-20200623163029959.png" alt="image-20200623163029959" style="zoom:80%;" />

### 2.4.8 搜索相关

<img src="C:\Users\zp\AppData\Roaming\Typora\typora-user-images\image-20200623163054248.png" alt="image-20200623163054248" style="zoom:80%;" />

# 3.Git

Git是目前世界上最先进的分布式版本控制系统，采用C语言编写。

## 3.0 安装Git

**Linux**

sudo apt-get install git

**Windowss**

直接在Git官网下载，或者前往[国内镜像](https://pan.baidu.com/s/1kU5OCOB#list/path=%2Fpub%2Fgit)下载。安装完成后，鼠标右键-->Git Bash，弹出下面命令窗口，即安装成功！

<img src="C:\Users\zp\AppData\Roaming\Typora\typora-user-images\image-20200115165150969.png" alt="image-20200115165150969" style="zoom:80%;" />



## 3.1 设置姓名和邮箱

```bash
git config --global user.name "your name"
git config --global user.email "youremail@qq.com"
```

因为Git是分布式版本控制系统，所以，每个机器必须自报家门：名字和Email地址。

## 3.2 设置SSH Key，添加公开密钥

```bash
ssh-keygen -t rsa -C "youremail@qq.com"
```

邮箱为创建账户时的邮箱地址，密码需要在认证时输入id_rsa文件是私有秘钥，id_rsa.pub是公开秘钥

windows版本：将会在目录C:\Users\zp\.ssh/ 生成 id_rsa 和 id_rsa.pub 文件。

linux版本：将在用户目录下~/.ssh/ 生成 id_rsa 和 id_rsa.pub 文件。

在github账户设置中添加SSH Key, 在Title中输入适当名称，在Key部分粘贴id_rsa.pub文件里的内容。

## 3.3 创建本地仓库、提交远程仓库

1. git init  // 初始化仓库
2. git add "文件名"  // 添加文件到本地仓库
3. git commit -m “first commit” //添加文件描述信息
4. git remote add origin + 远程仓库地址 //链接远程仓库，创建主分支
5. git push -u origin master //把本地仓库的文件推送到远程仓库

若出现以下问题：

![这里写图片描述](https://img-blog.csdn.net/20180330091437163?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZ29uZ3FpbmdsaW4=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

要想解决以上错误，只需要在4，5之间使用git pull origin master，

1. git remote add origin + 远程仓库地址 //链接远程仓库，创建主分支
2. git pull origin master // 把本地仓库的变化连接到远程仓库主分支
3. git push -u origin master //把本地仓库的文件推送到远程仓库

或者，在确保本地没问题的话，直接用 git push -f origin master 强行上传。

## 3.4 常用命令

git status  // 查看仓库当前状态

git diff  // 查看文件变化

git log  // 显示最近到最远的提交日志

# 4.ubuntu

## 4.1. 桌面环境异常

### 问题

状态栏消失，桌面只剩下壁纸和鼠标指针等；

某一个用户登录进去出现问题， 而另一个用户登录进去桌面环境正常

### 原因

Compiz配置出了问题。Compiz是一套自由的桌面特效软件，能够基于Linux的桌面环境增加视觉特效。

### 解决方法

1. 删除compiz的配置文件

   ```bash
   $ rm -rf ~/.compiz* ~/.config/compiz* ~/.cache/compiz* ~/.gconf/apps/compiz* ~/.config/dconf ~/.cache/dconf ~/.cache/unity  #删除dconf配置信息
   $ dconf reset -f /org/compiz/  #重置Compiz
   $ setsid unity #重启Unity
   $ unity --reset-icons #重置Unity图标(可选)
   ```

2. 如果还不行，重装Ubuntu-desktop

   ```bash
   $ sudo apt-get install --reinstall ubuntu-desktop
   $ sudo service lightdm restart
   ```

## 4.2. 查看磁盘空间大小及所有用户占用空间情况

### 4.2.1  使用命令

```bash
df -hl
```

df 命令是linux系统上以磁盘分区为单位来查看文件系统的命令，后面可以加上不同的参数来查看磁盘的剩余空间信息。

### 4.2.2 显示格式

<pre name="code" class="plain">
文件系统              容量  已用  可用  已用% 挂载点　
Filesystem            Size Used Avail Use% Mounted on
/dev/hda2              45G   19G   24G 44% /
/dev/hda1             494M   19M 450M   4% /boot
/dev/hda6             4.9G 2.2G 2.5G 47% /home
/dev/hda5             9.7G 2.9G 6.4G 31% /opt
none                 1009M     0 1009M   0% /dev/shm
/dev/hda3             9.7G 7.2G 2.1G 78% /usr/local
/dev/hdb2              75G   75G     0 100% /

以最后一行为例，其中，hdb2表示第二个硬盘的第二个分区，容量为75G，已用75G，可用0,已用100%，挂载点为根分区目录（/）。

### 4.2.3 相关命令

<pre name="code" class="plain">df -hl 查看磁盘剩余空间
df -h 查看每个根路径的分区大小
du -sh [目录名] 返回该目录的大小
du -sm [文件夹] 返回该文件夹总M数
df --help 查看更多功能

### 4.2.4 其他命令

查看硬盘的分区      #sudo fdisk -l
查看IDE硬盘信息     #sudo hdparm -i /dev/hda
查看STAT硬盘信息    #sudo hdparm -I /dev/sda 或 #sudo apt-get install blktool #sudo blktool /dev/sda id
查看硬盘剩余空间    #df -h #df -H
查看目录占用空间    #du -hs 目录名
优盘没法卸载        #sync fuser -km /media/usbdisk

### 4.2.5 查看各个用户占用内存

#参考du -sh 目录
sudo du -sh /home/*

#执行结果：
root@Ubuntu2:/home/zhangkf sudo du -sh /home/*
40G	/home/chenzl
730M	/home/hugp
904M	/home/jiling
40G	/home/john
46G	/home/tanbing
16K	/home/ubuntu
3.7M	/home/wangxm
3.7G	/home/xiaoyao
8.6G	/home/xutl
4.7M	/home/zhang
6.8G	/home/zhanggz
38G	/home/zhangkf
3.3M	/home/zhenjz

# 5.Mysql

## 5.1 ubuntu 安装mysql

1) 命令行输入命令

```bash
sudo apt-get install mysql-server
```

![如何在Ubuntu14.04中安装mysql](http://p1.pstatp.com/large/pgc-image/153585556245159dcdebcf9)

2) 如果之前已经安装过MySQL的话，此时如果碰到有新版本的MySQL，会出现需要配置的情况，如下图所示； 这里如果不设置新密码的话，则密码和之前的MySQL一致；如果你想设置新的密码，则输入新密码即可。

![如何在Ubuntu14.04中安装mysql](http://p1.pstatp.com/large/pgc-image/1535855562387e539246fea)

3） 在这里重新设置新密码，接下来弹出再次输入新密码的窗口，如下图所示。设置完成之后，点击“ok”即可，

![如何在Ubuntu14.04中安装mysql](http://p1.pstatp.com/large/pgc-image/1535855562383114734f631)

4） 等待MySQL安装完成，完成之后，如下图所示。

![如何在Ubuntu14.04中安装mysql](http://p1.pstatp.com/large/pgc-image/15358555624895cc280f57d)

5） 此时通过命令：ps aux | grep mysqld，进行查看，看mysql是否已经启动。

![如何在Ubuntu14.04中安装mysql](http://p3.pstatp.com/large/pgc-image/15358555624846c4ca7f774)

6）mysql启动完成之后，可以在命令行中输入命令：mysql –u root –p，之后输入之前设置的密码，即可进入到MySQL数据库。

![如何在Ubuntu14.04中安装mysql](http://p1.pstatp.com/large/pgc-image/1535855562560fad569cd1d)

7）接下来就可以正常使用MySQL了，增删改查等操作都可以正常进行，如下图所示

![如何在Ubuntu14.04中安装mysql](http://p1.pstatp.com/large/pgc-image/1535855562572bdbb9be5d1)

8）如果想退出MySQL数据库，直接输入“exit”或者“quit”即可。

## 5.2 修改root 密

**已知密码**

1)  修改密码

```bash
sudo mysqladmin -u root -p password
```

2）重启mysql服务

```bash
sudo service mysql restart
```

**忘记密码，重置密码**

在介绍修改密码之前，先介绍一个文件/etc/mysql/debian.cnf.其主要内容如下图：

![img](https://img-blog.csdn.net/2018050114213631)

里面有一个debian-sys-maint用户，这个用户只有Debian或Ubuntu服务器才有，所以如果您的服务器是Debain或Ubuntu，debian-sys-maint是个Mysql安装之后自带的用户，具体作用是重启及运行mysql服务。所以如果忘了root密码，可以通过这个用户来重设密码。下面介绍具体操作：
1）进入/etc/mysql/目录，并用root权限打开debian.cnf文件

```bash
cd /etc/mysql
sudo vim debian.cnf
```

2）使用这个文件中的用户名和密码进入mysql

```bash
mysql -u debian-sys-maint -p
```

3）选择mysql数据库（用户名和密码均存储在此数据库的user表中）

```mysql
use mysql;
```

4）显示user表中的列

```mysql
show fields from user; 
```

![img](https://img-blog.csdn.net/20180501142229412)

authentication_string这列就是密码（注：以前的版本这个字段是password,如果是password下面的操作将authentication_string替换成password即可）

5) 修改密码（修改密码为：123456）

```mysql
update mysql.user set authentication_string=password('123456') where user='root'
```

6）退出sql，重启mysql

```mysql
exit
```

```bash
service mysql restart
```

## 5.3 修改port

```bash
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```

修改mysqld.cnf文件里的port即可

重启mysql服务

## 5.4 添加用户

1） 进入mysql

```mysql
sudo mysql -u root -p
```

2) 添加用户

```mysql
insert into mysql.user(Host,User,authentication_string) values("localhost","test",password("123456"));
```

## 5.5 安装 <mysql/mysql.h>头文件

```bash
sudo apt-get install libmysql++-dev
```

# FFmpeg

## 1. 解码视频为视频帧

```bash
ffmpeg -i input.mp4 -vf fps=1 ./frames/frame%d.png
```

按照每秒一帧的帧率提取视频帧

# 6  VIM

## 6.1 移动光标

h,j,k,l 上，下，左，右
ctrl-e 移动页面
ctrl-f 上翻一页
ctrl-b 下翻一页
ctrl-u 上翻半页
ctrl-d 下翻半页
w 跳到下一个字首，按标点或单词分割
W 跳到下一个字首，长跳，如end-of-line被认为是一个字
e 跳到下一个字尾
E 跳到下一个字尾，长跳
b 跳到上一个字
B 跳到上一个字，长跳
0 跳至行首，不管有无缩进，就是跳到第0个字符
^ 跳至行首的第一个字符
$ 跳至行尾
gg 跳至文首
G 调至文尾
5gg/5G 调至第5行
gd 跳至当前光标所在的变量的声明处
fx 在当前行中找x字符，找到了就跳转至
; 重复上一个f命令，而不用重复的输入fx
\* 查找光标所在处的单词，向下查找
\# 查找光标所在处的单词，向上查找

## 6.2 删除复制

dd 删除光标所在行
dw 删除一个字(word)
d/D删除到行末x删除当前字符X删除前一个字符yy复制一行yw复制一个字y/D删除到行末x删除当前字符X删除前一个字符yy复制一行yw复制一个字y/Y 复制到行末
p 粘贴粘贴板的内容到当前行的下面
P 粘贴粘贴板的内容到当前行的上面

## 6.3 插入模式

i 从当前光标处进入插入模式
I 进入插入模式，并置光标于行首
a 追加模式，置光标于当前光标之后
A 追加模式，置光标于行末
o 在当前行之下新加一行，并进入插入模式
O 在当前行之上新加一行，并进入插入模式
Esc 退出插入模式

## 6.4 编辑

J 将下一行和当前行连接为一行
cc 删除当前行并进入编辑模式
cw 删除当前字，并进入编辑模式
c$ 擦除从当前位置至行末的内容，并进入编辑模式
s 删除当前字符并进入编辑模式
S 删除光标所在行并进入编辑模式
xp 交换当前字符和下一个字符
u 撤销
ctrl+r 重做
~ 切换大小写，当前字符
\>> 将当前行右移一个单位
<< 将当前行左移一个单位(一个tab符)
== 自动缩进当前行

## 6.5 查找替换

/pattern 向后搜索字符串pattern
?pattern 向前搜索字符串pattern
"\c" 忽略大小写
"\C" 大小写敏感

n 下一个匹配(如果是/搜索，则是向下的下一个，?搜索则是向上的下一个)
N 上一个匹配(同上)
:%s/old/new/g 搜索整个文件，将所有的old替换为new
:%s/old/new/gc 搜索整个文件，将所有的old替换为new，每次都要你确认是否替换

## 6.6 退出编辑器

:w 将缓冲区写入文件，即保存修改
:wq 保存修改并退出
:x 保存修改并退出
:q 退出，如果对缓冲区进行过修改，则会提示
:q! 强制退出，放弃修改

## 6.7 多文件编辑

vim file1.. 同时打开多个文件
:args 显示当前编辑文件
:next 切换到下个文件
:prev 切换到前个文件
:next！ 不保存当前编辑文件并切换到下个文件
:prev！ 不保存当前编辑文件并切换到上个文件
:wnext 保存当前编辑文件并切换到下个文件
:wprev 保存当前编辑文件并切换到上个文件
:first 定位首文件
:last 定位尾文件
ctrl+^ 快速在最近打开的两个文件间切换
:split[sp] 把当前文件水平分割
:split file 把当前窗口水平分割, file
:vsplit[vsp] file 把当前窗口垂直分割, file
:new file 同split file
:close 关闭当前窗口
:only 只显示当前窗口, 关闭所有其他的窗口
:all 打开所有的窗口
:vertical all 打开所有的窗口, 垂直打开
:qall 对所有窗口执行：q操作
:qall! 对所有窗口执行：q!操作
:wall 对所有窗口执行：w操作
:wqall 对所有窗口执行：wq操作
ctrl-w h 跳转到左边的窗口
ctrl-w j 跳转到下面的窗口
ctrl-w k 跳转到上面的窗口
ctrl-w l 跳转到右边的窗口
ctrl-w t 跳转到最顶上的窗口
ctrl-w b 跳转到最底下的窗口

## 6.8 多标签编辑

:tabedit file 在新标签中打开文件file
:tab split file 在新标签中打开文件file
:tabp 切换到前一个标签
:tabn 切换到后一个标签
:tabc 关闭当前标签
:tabo 关闭其他标签
gt 到下一个tab
gT 到上一个tab
0gt 跳到第一个tab
5gt 跳到第五个tab

## 6.9 执行shell 命令

1、在命令模式下输入":sh"，可以运行相当于在字符模式下，到输入结束想回到VIM编辑器中用exit，ctrl+D返回VIM编辑器
2、可以"!command"，运行结束后自动回到VIM编辑器中
3、用“Ctrl+Z“回到shell，用fg返回编辑
4、:!make -> 直接在当前目录下运行make指令

## 6.10 VIM 启动项

-o[n] 以水平分屏的方式打开多个文件
-O[n] 以垂直分屏的方式打开多个文件

## 6.11 自动排版

在粘贴了一些代码之后，vim变得比较乱，只要执行gg=G就能搞定

## 6.12 在vim中编译程序

在vim中可以完成make,而且可以将编译的结果也显示在vim里，先执行 :copen 命令，将结果输出的窗口打开，然后执行 :make
编译后的结果就显示在了copen打开的小窗口里了，而且用鼠标双击错误信息，就会跳转到发生错误的行。

## 6.13 buffer 操作

1、buffer状态
\- （非活动的缓冲区）
a （当前被激活缓冲区）
h （隐藏的缓冲区）
% （当前的缓冲区）
\# （交换缓冲区）
= （只读缓冲区）
\+ （已经更改的缓冲区）

## 6.14 VIM操作目录

1.打开目录
vim .
vim a-path/

2.以下操作在操作目录时生效
p,P,t,u,U,x,v,o,r,s

c 使当前打开的目录成为当前目录
d 创建目录
% 创建文件
D 删除文件/目录
\- 转到上层目录
gb 转到上一个 bookmarked directory
i 改变目录文件列表方式
^l 刷新当前打开的目录

mf - 标记文件
mu - unmark all marked files
mz - Compress/decompress marked files
gh 显示/不显示隐藏文件( dot-files)
^h 编辑隐藏文件列表
a 转换显示模式, all - hide - unhide
qf diplay infomation about file
qb list the bookmarked directories and directory traversal history
gi Display information on file

mb
mc
md - 将标记的文件(mf标记文件)使用 diff 模式
me - 编辑标记的文件,只显示一个，其余放入 buffer 中
mh
mm - move marked files to marked-file target directory
mc - copy
mp
mr
mt

vim 中复制,移动文件
1, mt - 移动到的目录
2, mf - 标记要移动的文件
3, mc - 移动/复制

R 移动文件

打开当前编辑文件的目录
:Explore
:Hexplore
:Nexplore
:Pexplore
:Sexplore
:Texplore
:Vexplore



# C++

## 1. makefile

### 1.1  =、？=、+=、：=

```makefile
ifdef DEFINE_VRE
    VRE = “Hello World!”
else
endif

ifeq ($(OPT),define)
    VRE ?= “Hello World! First!”
endif

ifeq ($(OPT),add)
    VRE += “Kelly!”
endif

ifeq ($(OPT),recover)
    VRE := “Hello World! Again!”
endif

all:
    @echo $(VRE)
```

敲入以下make命令：
make DEFINE_VRE=true OPT=define 输出：Hello World!
make DEFINE_VRE=true OPT=add 输出：Hello World! Kelly!
make DEFINE_VRE=true OPT=recover 输出：Hello World! Again!
make DEFINE_VRE= OPT=define 输出：Hello World! First!
make DEFINE_VRE= OPT=add 输出：Kelly!
make DEFINE_VRE= OPT=recover 输出：Hello World! Again!

从上面的结果中我们可以清楚的看到他们的区别了
= 是最基本的赋值
:= 是覆盖之前的值
?= 是如果没有被赋值过就赋予等号后面的值
+= 是添加等号后面的值

**=”和“:=”的区别到底有什么区别?**

 1、“=”

   make会将整个makefile展开后，再决定变量的值。也就是说，变量的值将会是整个makefile中最后被指定的值。看例子：

```makefile
 x = foo
 y = $(x) bar
 x = xyz
```

   在上例中，y的值将会是 xyz bar ，而不是 foo bar 。

   2、“:=”

   “:=”表示变量的值决定于它在makefile中的位置，而不是整个makefile展开后的最终值。

```makefile
x := foo
y := $(x) bar
x := xyz
```

   在上例中，y的值将会是 foo bar ，而不是 xyz bar 了。

### 1.2  $ @ ^ < ?

\$@ 表示目标文件
​\$^ 表示所有的依赖文件
​\$< 表示第一个依赖文件
$? 表示比目标还要新的依赖文件列表

## 2. 常用头文件

### C

\#include <assert.h>　　　　//设定插入点
\#include <ctype.h>　　　　 //字符处理
\#include <errno.h>　　　　 //定义错误码
\#include <float.h>　　　　 //浮点数处理
\#include <iso646.h>    //对应各种运算符的宏
\#include <limits.h>　　　　//定义各种数据类型最值的常量
\#include <locale.h>　　　　//定义本地化C函数
\#include <math.h>　　　　　//定义数学函数
\#include <setjmp.h>    //异常处理支持
\#include <signal.h>    //信号机制支持
\#include <stdarg.h>    //不定参数列表支持
\#include <stddef.h>    //常用常量
\#include <stdio.h>　　　　 //定义输入／输出函数
\#include <stdlib.h>　　　　//定义杂项函数及内存分配函数
\#include <string.h>　　　　//字符串处理
\#include <time.h>　　　　　//定义关于时间的函数
\#include <wchar.h>　　　　 //宽字符处理及输入／输出
\#include <wctype.h>　　　　//宽字符分类

传统C++

\#include <fstream.h>　　　 //改用<fstream>
\#include <iomanip.h>　　　 //改用<iomainip>
\#include <iostream.h>　　　//改用<iostream>
\#include <strstrea.h>　　　//该类不再支持，改用<sstream>中的stringstream

### C++

标准C++　

\#include <algorithm>　　　 //STL 通用算法
\#include <bitset>　　　　　//STL 位集容器
\#include <cctype>     //字符处理
\#include <cerrno> 　　　　 //定义错误码
\#include <cfloat>　　　　 //浮点数处理
\#include <ciso646>     //对应各种运算符的宏
\#include <climits> 　　　　//定义各种数据类型最值的常量
\#include <clocale> 　　　　//定义本地化函数
\#include <cmath> 　　　　　//定义数学函数
\#include <complex>　　　　 //复数类
\#include <csignal>     //信号机制支持
\#include <csetjmp>     //异常处理支持
\#include <cstdarg>     //不定参数列表支持
\#include <cstddef>     //常用常量
\#include <cstdio> 　　　　 //定义输入／输出函数
\#include <cstdlib> 　　　　//定义杂项函数及内存分配函数
\#include <cstring> 　　　　//字符串处理
\#include <ctime> 　　　　　//定义关于时间的函数
\#include <cwchar> 　　　　 //宽字符处理及输入／输出
\#include <cwctype> 　　　　//宽字符分类
\#include <deque>　　　　　 //STL 双端队列容器
\#include <exception>　　　 //异常处理类
\#include <fstream> 　　　 //文件输入／输出
\#include <functional>　　　//STL 定义运算函数（代替运算符）
\#include <limits> 　　　　 //定义各种数据类型最值常量
\#include <list>　　　　　　//STL 线性列表容器
\#include <locale>     //本地化特定信息
\#include <map>　　　　　　 //STL 映射容器
\#include <memory>     //STL通过分配器进行的内存分配
\#include<new>      //动态内存分配
\#include <numeric>     //STL常用的数字操作
\#include <iomanip> 　　　 //参数化输入／输出
\#include <ios>　　　　　　 //基本输入／输出支持
\#include <iosfwd>　　　　　//输入／输出系统使用的前置声明
\#include <iostream> 　　　//数据流输入／输出
\#include <istream>　　　　 //基本输入流
\#include <iterator>    //STL迭代器
\#include <ostream>　　　　 //基本输出流
\#include <queue>　　　　　 //STL 队列容器
\#include <set>　　　　　　 //STL 集合容器
\#include <sstream>　　　　 //基于字符串的流
\#include <stack>　　　　　 //STL 堆栈容器
\#include <stdexcept>　　　 //标准异常类
\#include <streambuf>　　　 //底层输入／输出支持
\#include <string>　　　　　//字符串类
\#include <typeinfo>    //运行期间类型信息
\#include <utility>　　　　 //STL 通用模板类
\#include <valarray>    //对包含值的数组的操作
\#include <vector>　　　　　//STL 动态数组容器

# python

## 1. pdb

在python中使用pdb模块可以进行调试

```python
import pdb
pdb.set_trace()
```

也可以使用python -m pdb mysqcript.py这样的方式

(Pdb) 会自动停在第一行，等待调试,这时你可以看看 帮助
(Pdb) h
说明下这几个关键 命令

**断点设置**
(Pdb)b 10 #断点设置在本py的第10行
或(Pdb)b ots.py:20 #断点设置到 ots.py第20行
删除断点（Pdb）b #查看断点编号
(Pdb)cl 2 #删除第2个断点

**运行**
(Pdb)n #单步运行
(Pdb)s #细点运行 也就是会下到，方法
(Pdb)c #跳到下个断点
**查看**
(Pdb)p param #查看当前 变量值
(Pdb)l #查看运行到某处代码
(Pdb)a #查看全部栈内变量
(Pdb)w 列出目前call stack 中的所在层。
(Pdb)d 在call stack中往下移一层
(Pdb)u 在call stack中往上移一层。如果在上移一层之后按下 n ,则会在上移之后的一层执行下一个叙述,之前的 function call 就自动返回。
(Pdb)cl 清除指定的断点。如果没有带参数,则清除所有断点。
(Pdb)disable 取消所有断点的功能,但仍然保留这些断点。
(Pdb)enable 恢复断点的功能。
(Pdb)ignore 设定断点的忽略次数。如果没指定 count,其初始 为 0。当 count 为 0 时,断点会正常动作。若有指定 count,则每次执行到该中断, count 就少 1,直到 count 数为 0。
(Pdb)condition bpnumber [condition]
(Pdb)j(ump) lineNo. 跳到某行执行。只有在 call stack 的最底部才能作用。
(Pdb)l 列出目前所在档案中的位置。连续地 l 命令会一直列到档案结尾,可以使用指定行数或范围来打印。
(Pdb)pp 和 p 命令类似,但是使用 pprint module(没用过 pprint,详情请参考 Python Library Reference)。
(Pdb)alias 以一个"别名"代替"一群除错命令",有点类似 c/c++ 的 macro(详情请参考 Python Library Reference)。

# docker

## 1 基础命令

- 容器操作

  docker create # 创建一个容器但是不启动它
  docker run # 创建并启动一个容器
  docker stop # 停止容器运行，发送信号SIGTERM
  docker start # 启动一个停止状态的容器
  docker restart # 重启一个容器
  docker rm # 删除一个容器
  docker kill # 发送信号给容器，默认SIGKILL
  docker attach # 连接(进入)到一个正在运行的容器
  docker wait # 阻塞一个容器，直到容器停止运行 

- 获取容器信息

  docker ps # 显示状态为运行（Up）的容器
  docker ps -a # 显示所有容器,包括运行中（Up）的和退出的(Exited)
  docker inspect # 深入容器内部获取容器所有信息
  docker logs # 查看容器的日志(stdout/stderr)
  docker events # 得到docker服务器的实时的事件
  docker port # 显示容器的端口映射
  docker top # 显示容器的进程信息
  docker diff # 显示容器文件系统的前后变化 

- 镜像操作

  docker images # 显示本地所有的镜像列表
  docker import # 从一个tar包创建一个镜像，往往和export结合使用
  docker build # 使用Dockerfile创建镜像（推荐）
  docker commit # 从容器创建镜像
  docker rmi # 删除一个镜像
  docker load # 从一个tar包创建一个镜像，和save配合使用
  docker save # 将一个镜像保存为一个tar包，带layers和tag信息
  docker history # 显示生成一个镜像的历史命令
  docker tag # 为镜像起一个别名 

## 2 实例

- 首先启动docker/nvidia-docker

  ```shell
  sudo start nvidia-docker/docker
  ```

- 基于"tsn"镜像创建容器"myflow"，并将本次目录/test挂载到容器目录/temp。

  docker run -i -t -v /test:/temp --name myflow tsn /bin/bash

  -t 表示在新容器内指定一个伪终端或终端；

  -i表示允许我们对容器内的 (STDIN) 进行交互；

  /bin/bash 这将在容器内启动 bash shell；

- 进入已有容器

  docker exec -it myflow /bin/bash

