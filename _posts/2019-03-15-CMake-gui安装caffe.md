---
layout:     post
title:      "通过CMake-Gui安装caffe"
subtitle:   "CMake-Gui编译caffe"
date:       2019-03-11 12:00:00
author:     "Rainweic"
header-img: "img/linux/linux_cmake_gui_caffe.jpg"
tags:
    - caffe
---

和清华小哥哥折腾了一天,他放弃了...
“这台服务器有毒...”
嗯,然后我不死心,尝试用CMake-Gui编译安装试试..

<br>
ubuntu16.04 python2(python3应该也是可以import caffe?我没试过emmm)
CMake GUI安装方法(Ubuntu 16)`sudo apt-get install cmake-qt-gui`

<br>
> 网上看需要的环境依赖,自行解决下
```
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install libboost-all-dev
sudo apt-get install libopenblas-dev liblapack-dev libatlas-base-dev
sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
sudo apt-get install git build-essential
sudo apt-get install python-pip python-numpy
sudo pip install protobuf
sudo pip install scikit-image
```

<br>
先git clone一下[caffe源码](https://github.com/BVLC/caffe) 数据量略大,慢慢等待 打把PUBG就结束了,**除非你很菜落地成盒~**
```
git clone https://github.com/BVLC/caffe.git
```

<br>
进入caffe文件夹,创建build文件夹.再打开cmake-gui
```
cd caffe && mkdir build
cmake-gui
```
`where is the source code`选择caffe目录
`where to build the binaries`选择新建的build目录
点击`Configure`选择`Unix Makefiles`下的`Use default native compilers`
Then `Finish` 
Then `Generate` (单GPU这样就ok了)
会看到输出框里有`Configuring done``Generating done`就ok了
(我这里出现了一个Warning 不是error就不管它了~~)

<br>
进入build文件夹,执行编译
```
cd build 
# -j20是20核编译 劳资服务器20核~ 刷刷的快啊
make -j20
```

<br>
编译python接口
```
make pycaffe
```

<br>
最后 caffe下有个python文件夹 用`pwd`把路径复制下来
整出一句:`export PYTHONPATH="/path/to/caffe/python"`
复制这句到`~/.bashrc`文件末尾保存,并`source ~/.bashrc`
**!如果你是用zsh 则复制到`~/.zshrc`文件末尾保存,并`source ~/.bashrc`**

<br>
测试:
```
python
import caffe
```

<br>
* `make -j20`报错:
```
CMakeFiles/Makefile2:647: recipe for target 'tools/CMakeFiles/caffe.bin.dir/all' failed
```
解决:没去理他...不知道咋做...

* `import caffe`报错
```
libcaffe.so.1.0.0: undefined symbol: _ZN6google4base21CheckOpMessageBuilder9NewStringB5cxx11Ev
```
解决: `conda install -c defaults protobuf libprotobuf`
若还没解决,[点击这里有详细的方法](https://github.com/conda/conda/issues/7141)

    ```
    ImportError: /usr/lib/x86_64-linux-gnu/libboost_python-py35.so.1.58.0: undefined symbol: PyUnicode_AsUTF8String
    ```
    解决:没去理他...不知道咋做...
    

