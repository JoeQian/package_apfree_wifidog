## 在LEDE环境编译apfree wifidog

### 前期准备工作

```shell
#使用主目录
cd ~
#从GitHub拉取lede代码，放在lede目录下
git clone https://github.com/lede-project/source.git lede
git checkout -b v17.0.4 v17.0.4
#返回上级目录
cd ..
#下载apfree_wifidog源码
git clone https://github.com/liudf0716/package_apfree_wifidog.git 
#将apfree的packag复制到lede目录下
cp -r package_apfree_wifidog/apfree_wifidog ~/lede/package/
#替换LEDE编译环境的libevent2
git clone https://github.com/KunTengRom/package_kunteng_libevent2.git
cp -fr package_kunteng_libevent2/libevent2 ~/lede/package/libs/
#进入lede目录
cd lede/
```

### 更新包

```shell
#可能你需要先安装zlib
sudo dnf install zlib-devel
#更新最新的包定义
./scripts/feeds update
#安装所有的包
./scripts/feeds install -a
```

### 编译配置

```shell
make menuconfig
```

1. 你需要选择你的目标硬件平台`Target System`
2. 在`> Network > Captive Portals`中选定`< > apfree_wifidog`
3. 按两下空格变`<*> apfree_wifidog`
4. 选择`< Save >`保存配置文件
5. 按两次`ESC`退出

### 编译

```shell
# -j4 表示4线程编译，速度会比较快
# V=s 表示显示错误信息
make -j4 V=s
```

编译完成之后目标文件在`./bin/packages/$targets/base`

```shell
#列出目录以及文件
tree ./bin/ 
#查找apfree
find ./bin/ | grep apfree
```

