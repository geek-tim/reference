
Github Action进行自动化部署前端项目
-----

## 前言

常见的web静态网站部署流程：
本地代码 ==> build 成上线的dist包 ==> 打包 or scp命令 上传到服务器
服务器的nginx 配置好文件夹 ==> 域名配置文件(域名映射)

## 原理
github 支持push驱动，启动一个小的虚拟机，帮助我们build scp 的过程



