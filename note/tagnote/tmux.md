---
tags:
  - 笔记
  - shell
---
首先，tmux的快捷键是以ctrl+b为前缀的，但是，要按住ctrl+b松手后再按其他键，一起按是不行的

**打开tmux**
```
tmux
tmux new -s <session-name>
```
**接入会话**
```
tmux attach -t 0
tmux attach -t <session-name>
```
**杀死会话**
```
tmux kill-session -t 0
tmux kill-session -t <session-name>
```
**分离会话**
```
tmux detach
ctrl+b d
```
**查看所有tmux会话**
```
tmux ls
Ctrl+b s
```

### 窗格操作
Ctrl+b %：划分左右两个窗格。
Ctrl+b "：划分上下两个窗格。
Ctrl+b ：光标切换到其他窗格。是指向要切换到的窗格的方向键，比如切换到下方窗格，就按方向键↓。
Ctrl+b ;：光标切换到上一个窗格。
Ctrl+b o：光标切换到下一个窗格。
Ctrl+b {：当前窗格左移。
Ctrl+b }：当前窗格右移。
Ctrl+b Ctrl+o：当前窗格上移。
Ctrl+b Alt+o：当前窗格下移。
Ctrl+b x：关闭当前窗格。
Ctrl+b !：将当前窗格拆分为一个独立窗口。
Ctrl+b z：当前窗格全屏显示，再使用一次会变回原来大小。
Ctrl+b Ctrl+：按箭头方向调整窗格大小。
Ctrl+b q：显示窗格编号。
### 窗口操作
,(逗号)：重命名窗口
c :创建新窗口
p: 前一个窗口
n: 后一个窗口

### 复制模式

