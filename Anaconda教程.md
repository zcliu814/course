

# Anaconda教程 

[TOC]

## 1.基础

进入cmd,

conda   查看是否安装成功，包括各种命令提示信息

**usage: `conda-script.py [-h] [-V] command ...`**

conda is a tool for managing and deploying applications, environments and packages.



### **Options:**

**positional arguments:**
  **command** ：    ***conda + command***
    clean                 Remove unused packages and caches.
    compare          Compare packages between conda environments.
    ***config***               Modify configuration values in .condarc. This is modeled after the git config  command. Writes to the user  .condarc file (C:\Users\Administrator\.condarc) by default. Use the --show-sources flag to display all identified configuration locations on your computer.
    ***create***               Create a new conda environment from a list of specified packages.
    info                   Display information about current conda install.
    init                     Initialize conda for shell interaction.
    ***install***                Installs a list of packages into a specified conda environment.
    list                     List installed packages in a conda environment.
    package           Low-level conda package utility. (EXPERIMENTAL)
    ***remove***            Remove a list of packages from a specified conda environment.
    rename            Renames an existing environment.
    run                    Run an executable in a conda environment.
    **search**              Search for packages and display associated information.The input is a MatchSpec, a query language for conda packages. See examples below.
    ***uninstall***           Alias for conda remove.
    ***update***             Updates conda packages to the latest compatible version.
    upgrade           Alias for conda update.
    notices             Retrieves latest channel notifications.



### <font color="#dd0000">**optional arguments:**</font>   

  **-h**, **--help**     Show this help message and exit.
  **-V**, **--version**  Show the conda version number and exit.

> ```shell
> conda xxx  --help      #查看xxx命令的帮助信息  
> conda config --show：  #查看conda所有的配置信息
> ```



**特别说明：Conda命令的一些选项开关有两种指定方式**

- 两个连接号“--”后跟选项名全程

- 一个连接号“-”后跟简称

- 比如说"-n"和"–-name"是等价的，但是要注意有些例外，比如说，“–version”对应的是“-V”（大写的V而不是小写的v）

  

### **conda commands available from other packages:**

  build
  content-trust
  convert
  debug
  develop
  env
  index
  inspect
  metapackage
  pack
  render
  repo
  server
  skeleton
  token
  verify



## 2. change tsinghua mirror

```shell
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/ 
conda config --set show_channel_urls yes
```





## 3. 常用命令

### 创建&查看&升级&激活

- 创建一个python版本x.x的conda环境xxx,且不需要询问yes or no 直接创建

```shell
conda create -n xxx python=x.x  -y  # --name   --yes
```



- 查看当前conda所有环境

```shell
conda info --envs    # -e
conda env list
```



- Anaconda版本升级

```shell
conda update -n base -c defaults conda    
```



- conda升级(与楼上有什么不同?)

``` shell
conda update conda          #将conda自身更新到最新版本
conda update anaconda       #将整个Anaconda都更新到确保稳定性和兼容性的最新版本
conda update anaconda-navigator    #update最新版本的anaconda-navigator`
```



### 激活&退出&删除

- 激活xxx环境

```shell
conda activate xxx
source activate xxx   #Linux环境中
activate  xxx
```



- 退出当前环境

```shell
conda activate            #缺省值 base
baseconda deactivate      #缺省值 当前虚拟环境名
```



- 删除xxx环境

```shell
conda remove -n xxx --all
```



### 包管理

- 查看环境中现有的包

```shell
conda list   #还显示关联环境下的已安装的package
pip list     #只显示当前虚拟环境的package
conda list -n xxx  #查看xxx环境下的package
```



- 在你的环境中用conda或者pip安装包

```shell
conda install 包名称            #效果基本一样，conda会安装相关依赖，pip更加简单
pip install 包名称              #有conda用它安装，没有的再用pip
```



- 包的安装&更新

```shell
conda install xxx           #在当前环境中安装一个包
conda install xxx=x.x.x     #指定xxx安装包的版本x.x.x
conda update xxx            #将xxx包更新到最新版本
```



- 清理包缓存

```shell
conda clean -p    #删除没有用的包
conda clean -t    #删除tar打包
```



### python管理

- python管理

```shell
python --version               #查看当前Python版本

conda install python=x.x       #将版本变更到指定版本
conda update python            #将python版本更新到最新版本

```



- 更新pip

```shell
python -m pip install --upgrade pip
```

