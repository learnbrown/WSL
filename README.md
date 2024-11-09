# Windows Subsystem for Linux(WSL) 的安装及配置教程
[Microsoft Learn中的WSL帮助文档](https://learn.microsoft.com/zh-cn/windows/wsl/)
## 使用wsl指令安装linux
使用cmd或powershell输入以下指令后，windows系统将自动开始下载ubuntu并开始安装 <br />
在指令后添加 --web-download 可以加速国内网络下的下载速度 <br />
```shell
wsl --install --web-download
```
默认安装为ubuntu系统，也可自己选择其他版本进行安装<br />
这里我已经安装过ubuntu，所以选择安装ubuntu24.04版本进行演示<br /> 
```shell
wsl --list --online #查询可以安装的系统版本
wsl --install <系统名（省略则默认安装ubuntu）> --web-download
```
![img1.png](/img/241029/img1.png)<br />
下载安装完成后，将自动在终端中打开ubuntu子系统并开始初始化 <br />![img2.png](/img/241029/img2.png)<br />
按照提示输入用户名及密码来设置账户，在linux中输入密码时看上去不会有任何显示，但实际上是有在正常地输入<br />![img3.png](/img/241029/img3.png)<br />
创建完成账户后子系统的安装便结束了，此后就直接进入了ubuntu的命令行界面<br />

在Ubuntu的使用中，常会用到apt来安装应用或其他工具，但新的系统apt可能无法直接使用，下面来进行apt的配置<br />
## apt的使用
apt是用于管理Ubuntu中软件包的命令行工具，ubuntu中应用的安装都需要使用到他，不过apt的默认源为国外的网站，将其更换为国内镜像源使用起来会更为便利<br />
### 为apt换源为国内镜像站
首先我们需要先打开apt的配置文件所在目录<br />
```bash
cd /etc/apt/sources.list
```
![img4.png](/img/241029/img4.png) <br />
使用```ls```命令可以查看当前目录下的文件，```sources.list```即为apt的配置文件<br />
由于```sources.list```默认的访问权限为只读，我们还需要修改他的访问权限，在指令前加```sudo```则表示后面的指令使用管理员权限，设置为777是对所有用户都开放读写权限<br />
```bash
sudo chmod 777 sources.list
```
修改完访问权限后我们先备份原来的配置文件<br />
```bash
sudo cp sources.list sources.list.copy
```
![img5.png](/img/241029/img5.png)<br />
此时就将原来的配置文件拷贝为了```sources.list.copy```文件<br /><br />
然后开始修改配置文件，使用vim打开该配置文件<br />
```bash
sudo vim sources.list
```
![img6.png](/img/241029/img6.png)<br />
进入vim后，初始为命令模式，可以使用指令```dd```对一整行进行剪切操作，这里我们使用该命令快速地将原来的内容清除，之后按下```i```键进入插入模式，此时就能像使用记事本一样在文件中输入字符，我们复制以下内容后按下右键即可复制到文件中<br />
```
deb https://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse
deb-src https://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse

deb https://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse
deb-src https://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse

deb https://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse
deb-src https://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse

# deb https://mirrors.aliyun.com/ubuntu/ jammy-proposed main restricted universe multiverse
# deb-src https://mirrors.aliyun.com/ubuntu/ jammy-proposed main restricted universe multiverse

deb https://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse
deb-src https://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse

```
然后按下```esc```退出插入模式，输入```:wq```回车保存并退出文件<br />
这里其实是将原来apt的源更换为国内的阿里云镜像源<br /><br />
![img7.png](/img/241029/img7.png)
换源后还需更新apt的列表
```bash
sudo apt update
```
![img8.png](/img/241029/img8.png)
更新apt列表后才能正常的开始使用apt下载应用<br /><br />

### apt常用指令
以下是apt中常用的指令
```bash
# 更新系统中的应用
sudo apt upgrade
```
![img9.png](/img/241029/img9.png)
```bash
# 下载应用（例如下载net-tools）
sudo apt install net-tools
```
![img10.png](/img/241029/img10.png)

## 设置默认语言为中文

```bash
# 安装中文语言包
sudo apt install language-pack-zh-hans
# 设置默认语言
sudo update-locale LANG=zh_CN.UTF-8
```

之后在powershell或cmd中重启wsl后就能生效
```shell
wsl --shutdown
```