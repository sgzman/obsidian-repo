> [[2023-11-15]]
### 前言
	本地环境：Windows11、VS2022
	编译项目：ASP.NET CORE Web项目，框架为NET7
### 安装捆绑包
	本地环境IIS部署时只需要安装ASP .NET CORE 7 捆绑包（项目编译时框架版本最好保持一致）后，Web网站即可启动，安装地址[.NET 下载(Linux、macOS 和 Windows) (microsoft.com)](https://dotnet.microsoft.com/zh-cn/download/dotnet)

	![[image-20231115094208622.png]]


### 问题及解决

	在Windows Server 2012中安装上述Bundle后，项目仍然无法启动。
	在查看[Asp.net core IIS部署错误 ，Failed to start application '/LM/W3SVC/22/ROOT', ErrorCode '0x8000ffff'. - youliCC - 博客园 (cnblogs.com)](https://www.cnblogs.com/youlicc/p/17396942.html)此博客后，便前往事件查看器查看错误日志，错误日志如下图
	
	![[image-20231115095132833.png]]


``
```C#
//另一个报错
Could not find 'aspnetcorev2_inprocess.dll'. Exception message:
You must install or update .NET to run this application.

App: C:\Web\onePage\OnePage.dll
Architecture: x64
Framework: 'Microsoft.NETCore.App', version '6.0.0' (x64)
.NET location: C:\Program Files\dotnet\

The following frameworks were found:
  7.0.13 at [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]

Learn about framework resolution:
https://aka.ms/dotnet/app-launch-failed

To install missing framework, download:
https://aka.ms/dotnet-core-applaunch?framework=Microsoft.NETCore.App&framework_version=6.0.0&arch=x64&rid=win81-x64

```

于是便下载aspnetcorev2_inprocess.dll，并通过搜索引擎得知放置位置为C:\Windows\System32文件夹下，为了保险起见我还在别的位置（C:\Windows\SysWOW64）也放了这个dll，然后仍然无法启动，并且是空白页面。

此时心态有点崩了，而且事件管理器里面也没有报错了，那么应该就是IIS设置的问题了，于是又找到了一篇博客https://blog.51cto.com/u_4283119/6145886，经提示把应用池中32位应用程序开关打开就好了。

### 总结

实际解决的步骤也不一定是上述的办法，也有可能迷迷糊糊之中就解决了，也有可能是缺少某个Windows KB补丁包，但是好在有了这个记录日志，希望以后能更加顺利的解决。



--2023-11-21更新
为了同时启用身份验证，需要参考如下链接进行设置
[在 ASP.NET Core 中配置 Windows 身份验证 | Microsoft Learn](https://learn.microsoft.com/zh-cn/aspnet/core/security/authentication/windowsauth?view=aspnetcore-7.0&tabs=visual-studio)