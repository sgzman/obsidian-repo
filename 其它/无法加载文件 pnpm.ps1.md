
前些日子在IDEA的终端中执行pnpm安装的时候，提示

```
pnpm : 无法加载文件 C:\Users\sugz\AppData\Roaming\npm\pnpm.ps1，因为在此系统上禁止运行脚本。
```

在某度上搜索看到已有前人给出了解决方案，在此记录一下

使用管理员权限打开CMD，在终端中执行下面的命令回车即可

```
set-ExecutionPolicy RemoteSigned
```
