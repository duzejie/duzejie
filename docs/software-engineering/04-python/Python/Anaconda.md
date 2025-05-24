
# Anaconda
## 安装anaconda
1. 安装
- 添加至系统变量

2.更换国内镜像源：
推荐清华的镜像源， https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/ ，可以点击查看具体信息。

在cmd或PowerShell环境内输入以下指令
```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
```

3.安装额外包

```sh
conda install pkg_name # 安装一个叫pkg_name的包，
conda install pkg_name1 pkg_name2 pkg_name3   # 安装多个包，空格分隔
conda install pkg_name=version  # 安装指定版本号的包，如conda install tensorflow=1.13.1
conda remove pkg_name   # 移除指定包
conda update pkg_name   # 升级指定包
```


4. 配置 PowerShell

Win10的PowerShell在使用 `conda activate` 环境名激活环境时无效，而CMD则可以（这里**前提必须将 Anaconda 写入环境变量**，否则在PowerShell 输入 `conda` 的任何命令都会无法识别）。
解决方法如下：

-   用Win + X 组合键调出PowerShell 管理员模式；
-   输入命令 `conda init powershell` ；
-   关闭当前powershell窗口，重新打开一个powershell窗口输入 `conda activate 环境名` 测试。

CMD 的话只需把上面三步中的powershell 改为cmd.exe 即可。

_**如果不想每次一启动Shell 就自动激活Base 环境**_

在终端输入 `conda config --set auto_activate_base false` ，即可。

如果又反悔了，想显示了：

`conda config --set auto_activate_base true`




## 使用Anaconda

1.创建环境

```sh
#  conda环境管理命令
python -V         			#所处Python环境
conda create -n env_name python=3.8
conda create --name python27 python=2.7    #创建一个名为python27的环境， 指定Python版本是2.7
                            #（不用管是2.7.x，conda会为我们自动寻找2.7.x中的最新版本）
conda info -e   			#会列出当前安装的所有pyhon环境
conda info --envs    # 显示当前所有的Anaconda环境 。

conda create -n bunnies python=3 Astroid Babel  #创建新环境，并安装包 Astroid 和 Babel
conda create -n dalao --clone nb     # 这个指令将创建一个叫dalao的环境，这个环境与nb一样。
# 注意，最初Anaconda使用的环境叫base，也就是说你可以conda create -n dalao --clone base。

conda deactivate            #回到之前的环境
conda remove --name python34 --all         #删除一个已有的环境
conda activate env_name & deactivate  # 前者用来激活一个环境，后者用来退出一个环境。注意：conda activate需要带上环境名，conda deactivate不用，退出后回到base环境。

```


2. 包管理

```shell
#管理包
conda --version             #返回你安装Anaconda软件的版本
conda update conda          #升级当前版本的conda
conda list                  #查看该环境中包和其版本的列表
conda search beautifulsoup4 #查找一个包
conda install --name bunnies beautifulsoup4  #指定安装环境（--name bunnies | -n bunies）
conda install beautifulsoup4 # 安装到当前环境
conda remove -n bunnies iopro #指定环境，移除包 
conda config --add channels    \  https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/conda config --set  \ show_channel_urls yes       # 设置国内镜像
```
