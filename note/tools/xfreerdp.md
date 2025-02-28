
**我使用的kali linux 2024.3的xfreerdp更新到3版本，需要使用xfreerdp3使用**

1 接收不到连接请求回应信息[0x00020006]
解决方法如下：
删除 ./config/freerdp 下的内容。

2 接收到了连接请求信息，但是激活超时Timeout waiting for activation[0x0002000B]
解决方法如下：
xfreerdp指令后设置 /timeout:60000
这个方法会导致xfreerdp很慢

3 实在不行使用thm官方的机器

