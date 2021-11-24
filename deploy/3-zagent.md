# 部署ZAgent应用服务

1. 从[这里](https://github.com/easysoft/zagent/releases)下载ZAgent应用，程序分为3个部分，分别为：
   - 服务端程序 zagent-server
   - 宿主机代理 zagent-host
   - 虚拟机代理 zagent-vm

2. 复制zagent-server到服务器，执行；

  ```
  ./zagent-server
  ```

3. 复制zagent-host到宿主机，执行；

  ```
   ./zagent-host -t host -s http://服务器IP:服务器端口 -i 本机IP -p 本机端口
  ```

4. 启动前面安装的虚拟机，在用户根目录下创建kvm文件夹，得到形如c:\Users\aaron\kvm的目录；
5. 复制zagent-vm文件到c:\Users\aaron\kvm\agent目录；
6. 创建以下内容的文件，保存为c:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp\zagent.bat。Windows StartUp目录的可执行文件，在系统启动时会自动被运行。

  ```
  @echo on
  
  :start
  
  ping /n 3 114.114.114.114 | findstr "TTL=" && goto next || goto start
  
  :next
  cmd /k "cd /d c:\Users\aaron\kvm\agent && zagent-vm -t vm -s 服务器IP:服务器端口"
  
  pause
  ```