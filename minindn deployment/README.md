minindn主页：https://minindn.memphis.edu/introduction.html

# 服务器部署minindn




```bash
git clone https://github.com/named-data/mini-ndn
# 执行install.sh
./install.sh # 选项用--no-wifi，因为ppa暂时不支持
# 我当时git clone依赖的时候一直clone不下来，于是手动加到了dl下，所以执行完之后仍然有问题，解决办法是再一个个编译安装，不清楚如果脚本里clone成功会不会自动编译安装
```





报错：Checking for 'libndn-cxx'                  : not found 
```bash
# 在ndn-cxx下
chmod +x ./waf
./waf configure
./waf
sudo ./waf install

# 如果出现库文件没有被安装到系统文件中，还要执行：

sudo cp build/libndn-cxx.so* /usr/local/lib
```
报错：Checking supported LINKFLAGS             : not found 
```bash
sudo apt update
sudo apt install lld
```

Package 'pep8' has no installation candidate. 在新版本的ubuntu中，pep8已经被弃用，使用pycodestyle。将install.h(不止一个)中的pep8全改为pycodestyle


新版本的Ubuntu会出现python由外部管理的问题，最粗暴的方式（不推荐但有效）：
```bash

sudo mv /usr/lib/python3.x/EXTERNALLY-MANAGED /usr/lib/python3.x/EXTERNALLY-MANAGED.bk
```

