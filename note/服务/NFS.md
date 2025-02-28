
这里讲解对于nfs服务的渗透动作

通过rpcinfo命令来确定主机上是否运行或挂载了NFS服务:
```bash
rpcinfo -p IP
```

检索给定主机的导出文件夹列表，这些信息将被用于访问这些文件夹:
```bash
showmount -e IP
```

创建一个文件夹将目录挂载在本地：
```bash
mkdir DIR
sudo mount -t nfs IP:/FILE/PATH DIR
```
然后就饿可以直接进入共享文件夹查看文件了。


