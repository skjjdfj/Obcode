---
tags:
  - 技巧
  - docker
  - 逃逸
cssclasses:
---
如何检查机器是否在docker内部：
[linux - 如何确定某个进程是否在 lxc/Docker 内部运行？ - 堆栈内存溢出 --- linux - How to determine if a process runs inside lxc/Docker? - Stack Overflow](https://stackoverflow.com/questions/20010199/how-to-determine-if-a-process-runs-inside-lxc-docker#:~:text=The%20most%20reliable%20way%20is,name%20of%20the%20anchor%20point.)
最可靠的方法是检查`/proc/1/cgroup` 。它将告诉您 init 进程的控制组，当您_不在_容器中时，对于所有层次结构，它将是`/` 。当您在容器_内_时，您将看到锚点的名称。对于 LXC/Docker 容器，它将分别类似于`/lxc/<containerid>`或`/docker/<containerid>` 。

使用`mount`命令查看挂载点：
```bash
mount
```
使用`df`命令查看磁盘使用情况(看看挂载在哪个文件夹下)
```bahs
df
```

如果输出中包含 `docker`、`kubepods` 或其他容器相关的标识（如 `container`），则表明当前处于容器环境:
```bash
cat /proc/self/cgroup
```

Docker 容器通常会生成唯一的主机名（包含容器ID）：
```bash
hostname
```
容器内的特征 ：  输出可能类似 `e3b1a2f4c5d6`（容器ID的前12位），而宿主机的主机名通常为固定名称

