# 1. tmux

## tmux是什么

tmux (terminal multiplexer) 是Linux上的终端复用神器，可从一个屏幕上管理多个终端（准确说是伪终端）。使用该工具，用户可以连接或断开会话，而保持终端在后台运行。

## 基本结构

tmux结构包括会话（session）、窗口（window）和窗格（pane）三部分，会话实质是伪终端集合，每个窗格表示一个伪终端，多个窗格展现在一个屏幕上，这一屏幕就是窗口，基本结构及状态信息如下图所示：

![](G:\errors&resolutions_log\tmux.png)

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

## 显示空格和tab符号

### 打开setting,在搜索框中输入renderControlCharacters,选中勾选框,即可显示tab

### 在搜索框中输入renderWhitespace,选择all,即可显示空格.

# 3.git

## 3.1 设置姓名和邮箱

```bash
git config --global user.name "your name"
git config --global user.email "youremail@qq.com"
```

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