# 1. pip install package以后使用conda list命令找不对对应的package
```shell
$ conda create -n virtual-python-name python=3.x
```
## 1.1 产生的原因: conda install package与pip install package位置不一致
（1）conda install package 默认安装在根目录\~/.conda/site-packages下面
（2）pip install package 默认安装在\~/conda/envs/virtual-python-name/lib/python3.x/site-packages
> ~表示安装conda的根目录
>virtual-python-name 表示conda 创建的虚拟python环境的名称
>python3.x 表示安装的python版本

## 1.2 产生的问题
因此在使用**pip install package**以后, 使用**conda list**找不到安装的对应包, 无法实现统一管理

## 1.3 解决方案
需要修改配置项, 使得两种命令安装位置保持一致
(1) 配置项位于一个\~/conda/envs/virtual-python-name/lib/python3.x/site.py
site.py 头部的代码大概是下面这个样子
```python
import sys 
import os
import builtins
import _sitebuiltins
import io

PREFIXES = [sys.prefix, sys.exec_prefix]
ENABLE_USER_SITE = False
USER_SITE = None
USER_BASE = None
```
(2) 主要需要三个变量值
- ENABLE_USER_SITE
- USER_SITE
- USER_BASE

(3) 更改对应的变量值并保存即可
- ENABLE_USER_SITE = True
- USER_SITE = "\~/conda/envs/virtual-python-name/lib/python3.x/site-packages"
- USER_BASE = "\~/conda/envs/virtual-python-name/lib/python3.x"

## 1.4 更改site.py会产生的问题
(1) 使用**conda create -n env_name python=3.x**时, 如果python版本与之前更改过site.py的虚拟环境python版本一致时, 新的env_name会复用已被更改过site.py, 因此需要将site.py下面对应**USER_SITE**和**USER_BASE**更改为正确的值, 否则后续执行**conda install** 或者 **pip install**都会报错

## 1.5 对于Linux, 有如下脚本参考(可以直接完成对应的变量更改)
```bash
#!/bin/bash
#=====================================================================================================================#
## 设置conda的安装目录
LOCAL_CONDA_HOME="/home/user/hoshinory/conda"
## 设置安装的虚拟环境名称
VIRTUAL_ENV_NAME="test"
LIB_PATH="${LOCAL_CONDA_HOME}/envs/${VIRTUAL_ENV_NAME}/lib"
PYTHON3="${LOCAL_CONDA_HOME}/envs/${VIRTUAL_ENV_NAME}/bin/python3"

#先寻找python版本
PYTHON_VERSION=`ls ${LIB_PATH} | grep python | grep -v "lib"`
#然后就能拿到site.py
SITE_FILE="${LIB_PATH}/${PYTHON_VERSION}/site.py"

#需要替换的字符串
string1="ENABLE_USER_SITE \\= True"
string2="USER_SITE \\= ${LIB_PATH}/${PYTHON_VERSION}/site-packages"
string3="USER_BASE \\= ${LIB_PATH}/${PYTHON_VERSION}"
#=====================================================================================================================#

#拷贝SITE_FILE过来
cp ${SITE_FILE} ./
#找到对应的行，整行替换
num1=`cat site.py | grep -n "ENABLE_USER_SITE \\= " | head -n 1| awk -F":" '{print $1}'`
sed -i "${num1}c ENABLE_USER_SITE \\= True" site.py

num2=`cat site.py | grep -n "USER_SITE \\= " | grep -v "ENABLE_USER_SITE" | head -n 1| awk -F":" '{print $1}'`
sed -i "${num2}c USER_SITE \\= '${LIB_PATH}/${PYTHON_VERSION}/site-packages'" site.py
num3=`cat site.py | grep -n "USER_BASE \\= " | head -n 1| awk -F":" '{print $1}'`
sed -i "${num3}c USER_BASE \\= '${LIB_PATH}/${PYTHON_VERSION}'" site.py
cp ./site.py ${SITE_FILE}
rm -f ./site.py
```


