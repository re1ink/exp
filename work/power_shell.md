# power shell常用命令

查询端口占用情况，并根据进程PID结束进程

```shell
netstat -ano | findstr "8080"
taskkill /PID 36496 /F
```