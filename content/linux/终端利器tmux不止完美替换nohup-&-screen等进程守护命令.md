---
title: "终端利器tmux不止完美替换nohup-&-screen等进程守护命令 "
date: 2017-01-31T12:49:08+08:00
categories: ["All","Linux","efficiency"]
tags: ["Linux","efficiency"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["Linux","efficiency","Tmux","terminal" ]
description: "终端利器tmux不止完美替换nohup-&-screen等进程守护命令"
---

> “君子生非异也，善假于物也” 。–语出《荀子·劝学》

如果记得没错的话，《荀子·劝学》我们这一代高中的时候应该都读过这篇文章。原意大概是君子的资质与一般人没有什么区别，君子之所以高于一般人，是因为他能善于利用外物。对于技术人来说，好工具的选择和使用往往可以省去很多不必要的麻烦，好的开源工具很多，看你如何去使用。对于终端复用工具这里推荐使用tmux，当然此类工具比较好的还有screen，不过相对screen 这里我更倾向于推荐tmux[强悍的分屏等]。

如果仅仅只是多标签的功能，那么putty结合一些工具也可以做到，或者干脆使用xshell，当然tmux此类工具不仅仅是那么简单。另外一个选择使用tmux/screen工具的原因是，有时我们会经常需要SSH或者telent远程登录到Linux服务器，有一些任务需要长时间运行，比如系统备份、数据传输等。通常情况下我们都是开一个远程终端窗口，因为执行时间比较长，一般需要等待它执行完毕，在此过程中不能关闭窗口或者网络原因断开连接，断开之后就Game Over了。这个功能就有点类似nohup和setsid命令的实现了，而tmux/screen则集nohup/setsid和多标签于一身。废话少说，我们接下来看如何安装使用它。

## 安装

安装的话这里就不过说明了，不同的Linux发行版相应的包管理机制不同，安装tmux包即可。

### 使用技巧

**几个名词**

tmux主要包括以下几个模块：

模块 | 解释
------- | -------
session | 会话:一个服务器可以包含多个会话
window | 窗口:一个会话可以包含多个窗口
pane | 面板:一个窗口可以包含多个面板[强悍的分屏]

#### 小试牛刀
列出了`tmux`的几个基本模块之后，就要来点实实在在的干货了，和`screen`默认激活控制台的`Ctrl+a`不同，`tmux`默认的是`Ctrl+b`，使用快捷键之后就可以执行一些相应的指令了。当然如果你不习惯使用`Ctrl+b`，也可以在`~/.tmux`文件中加入以下内容把快捷键变为`Ctrl+a`：

```
# Set prefix key to Ctrl-a
unbind-key C-b
set-option -g prefix C-a

```
以下所有的操作都是激活控制台之后，即键入`Ctrl+b`前提下才可以使用的命令【这里假设快捷键没改，改了的话则用`Ctrl+b`】。

##### 基本操作：

操作符 | 解释
------- | -------
? | 列出所有快捷键；按q返回
d | 脱离当前会话,可暂时返回Shell界面，输入tmux attach能够重新进入之前会话
s | 选择并切换会话；在同时开启了多个会话时使用
D | 选择要脱离的会话；在同时开启了多个会话时使用
: | 进入命令行模式；此时可输入支持的命令，例如kill-server所有tmux会话
[ | 复制模式，光标移动到复制内容位置，空格键开始，方向键选择复制，回车确认，q/Esc退出
] | 进入粘贴模式，粘贴之前复制的内容，按q/Esc退出
~ | 列出提示信息缓存；其中包含了之前tmux返回的各种提示信息
t | 显示当前的时间

**Ctrl+z	挂起当前会话**

##### 窗口操作:

操作符 | 解释
------- | -------
c | 创建新窗口
& | 关闭当前窗口
数字键 | 切换到指定窗口
p | 切换至上一窗口
n | 切换至下一窗口
l | 前后窗口间互相切换
w | 通过窗口列表切换窗口
, | 重命名当前窗口，便于识别
. | 修改当前窗口编号，相当于重新排序
f | 在所有窗口中查找关键词，便于窗口多了切换

##### 面板操作:

操作符 | 解释
------- | -------
“ | 将当前面板上下分屏
% | 将当前面板左右分屏
x | 关闭当前分屏
! | 将当前面板置于新窗口,即新建一个窗口,其中仅包含当前面板
Ctrl+方向键 | 以1个单元格为单位移动边缘以调整当前面板大小
Alt+方向键 | 以5个单元格为单位移动边缘以调整当前面板大小
空格键 | 可以在默认面板布局中切换，试试就知道了
q | 显示面板编号
o | 选择当前窗口中下一个面板
方向键 | 移动光标选择对应面板
{ | 向前置换当前面板
} | 向后置换当前面板
Alt+o | 逆时针旋转当前窗口的面板
Ctrl+o | 顺时针旋转当前窗口的面板
z | tmux 1.8新特性，最大化当前所在面板

**.tmux.conf基本配置**
软件到手了，自己怎么舒服就怎么用。定制主要还是在于.tmux.conf配置文件的配置，以下列出我的配置文件：

```

# Set prefix key to Ctrl-a
unbind-key C-b
set-option -g prefix C-a
bind-key C-a last-window # 方便切换，个人习惯
bind-key a send-prefix
# shell下的Ctrl+a切换到行首在此配置下失效，此处设置之后Ctrl+a再按a即可切换至shell行首
# reload settings # 重新读取加载配置文件
bind R source-file ~/.tmux.conf \; display-message "Config reloaded..."
# Ctrl-Left/Right cycles thru windows (no prefix)
# 不使用prefix键，使用Ctrl和左右方向键方便切换窗口
bind-key -n "C-Left" select-window -t :-
bind-key -n "C-Right" select-window -t :+
# displays
bind-key * list-clients
set -g default-terminal "screen-256color" # use 256 colors
set -g display-time 5000 # status line messages display
set -g status-utf8 on # enable utf-8
set -g history-limit 100000 # scrollback buffer n lines
setw -g mode-keys vi # use vi mode
# start window indexing at one instead of zero 使窗口从1开始，默认从0开始
set -g base-index 1
# key bindings for horizontal and vertical panes
unbind %
bind | split-window -h # 使用|竖屏，方便分屏
unbind '"'
bind - split-window -v # 使用-横屏，方便分屏
# window title string (uses statusbar variables)
set -g set-titles-string '#T'
# status bar with load and time
set -g status-bg blue
set -g status-fg '#bbbbbb'
set -g status-left-fg green
set -g status-left-bg blue
set -g status-right-fg green
set -g status-right-bg blue
set -g status-left-length 90
set -g status-right-length 90
set -g status-left '[#(whoami)]'
set -g status-right '[#(date +" %m-%d %H:%M ")]'
set -g status-justify "centre"
set -g window-status-format '#I #W'
set -g window-status-current-format ' #I #W '
setw -g window-status-current-bg blue
setw -g window-status-current-fg green
# pane border colors
set -g pane-active-border-fg '#55ff55'
set -g pane-border-fg '#555555'

```

**开启批量执行**

如果已经修改`prefix`键位`Ctrl+a`，则`Ctrl+a`[默认`Ctrl+b`]后输入`:set synchronize-panes` ，输入`:set sync` `[TAB]`键可自动补齐

取消批量执行模式重复之前操作即可

**脚本化启动**
把以下脚本内容加入到~/.bashrc，即可每次登录进入到tmux

```
tmux_init()
{
tmux new-session -s "kumu" -d -n "local" # 开启一个会话
tmux new-window -n "other" # 开启一个窗口
tmux split-window -h # 开启一个竖屏
tmux split-window -v "top" # 开启一个横屏,并执行top命令
tmux -2 attach-session -d # tmux -2强制启用256color，连接已开启的tmux
}
# 判断是否已有开启的tmux会话，没有则开启
if which tmux 2>&1 >/dev/null; then
test -z "$TMUX" && (tmux attach || tmux_init)
fi
```

### 其他介绍

tmux是一个优秀的终端复用软件，类似GNU Screen，但来自于OpenBSD，采用BSD授权。使用它最直观的好处就是，通过一个终端登录远程主机并运行tmux后，在其中可以开启多个控制台而无需再“浪费”多余的终端来连接这台远程主机；当然其功能远不止于此。

ubuntu可以使用sudo apt-get install tmux来安装。安装完成后输入命令tmux即可打开软件，界面十分简单，类似一个下方带有状态栏的终端控制台；但根据tmux的定义，在开启了tmux服务器后，会首先创建一个会话，而这个会话则会首先创建一个窗口，其中仅包含一个面板；也就是说，这里看到的所谓终端控制台应该称作tmux的一个面板，虽然其使用方法与终端控制台完全相同。

tmux使用C/S模型构建，主要包括以下单元模块：

概念 | 解释
------- | -------
server | 服务器。输入tmux命令时就开启了一个服务器。
session | 会话。一个服务器可以包含多个会话。
window | 窗口。一个会话可以包含多个窗口。
pane | 面板。一个窗口可以包含多个面板。
 

**会话**
```
tmux new -s session #建立会话
tmux new -s session -d #在后台建立会话 
tmux ls #列出会话 
tmux attach -t session #进入某个会话 

``` 


**配置**
tmux的系统级配置文件为/etc/tmux.conf，用户级配置文件为~/.tmux.conf。没有该文件新建一个即可。配置文件实际上就是tmux的命令集合，也就是说每行配置均可在进入命令行模式后输入生效。

注意：更改配置之后需要重启tmux或者进入命令模式（按按键前缀后再按: )输入 source-file ~/.tmux.conf即可。

```
#将ctrl-r键设置为加载配置文件，并显示"reloaded!"信息
bind C-r source-file ~/.tmux.conf \; display "Reloaded!"

``` 

**copy-mode(复制模式)**

a.PREFIX [ 进入复制模式 
b.按 space 开始复制，移动光标选择复制区域 
c.按 Enter 复制并退出copy-mode。 
d.将光标移动到指定位置，按 PREIFX ] 粘贴 

在~/.tmux.conf中加入如下行，否则在VIM中复制模式无法完成操作 

`setw -g mode-keys vi`

重新连接到之前的窗口

`tmux attach`

不过如果没有会话会提示：

`no sessions`

修改Tmux配置如果无分离终端则新建，在.tmux.conf加入如下行：

new-session
 

**我的.tmux.conf:**

```

#将ctrl-r键设置为加载配置文件，并显示"reloaded!"信息
bind C-r source-file ~/.tmux.conf \; display "Reloaded!"

#tmux attach 如果无分离终端则新建
new-session

#此类配置可以在命令行模式中输入show-options -g查询
set-option -g status-keys vi        #操作状态栏时的默认键盘布局；可以设置为vi或emacs
set-option -g status-utf8 on        #开启状态栏的UTF-8支持

#此类设置可以在命令行模式中输入show-window-options -g查询
set-window-option -g mode-keys vi    #复制模式中的默认键盘布局；可以设置为vi或emacs
set-window-option -g utf8 on        #开启窗口的UTF-8支持

#选择分割的窗格,不需要松开ctrl键，多次跳转更方便
bind C-k selectp -U            # 选择上窗格
bind C-j selectp -D            # 选择下窗格
bind C-h selectp -L            # 选择左窗格
bind C-l selectp -R            # 选择右窗格

#执行命令，比如 Manpage
bind m command-prompt "splitw -h 'exec man %%'"

```
 
**Start tmux on every shell login**
`vi ~/.bashrc`

添加：
```

# If not running interactively, do not do anything
[[ $- != *i* ]] && return
[[ $TERM != screen* ]] && exec tmux

```

详细参考：https://wiki.archlinux.org/index.php/Tmux#Start_tmux_on_every_shell_login

**注：mac下会不能这样配置，否则会打开终端后秒退**

这样做，会使每一个该用户下的终端都进入tmux。如果需要ssh到其他机子上，进入其上的tmux 或 screen，会导致快捷键冲突以及显示混乱。