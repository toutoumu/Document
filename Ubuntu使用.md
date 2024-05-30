# 软件安装

## 0. Ulauncher安装软件启动器

[官网](https://github.com/Ulauncher)
[快捷键设置](https://github.com/Ulauncher/Ulauncher/wiki/Hotkey-In-Wayland)

```shell
# 打开官网下载安装包
sudo dpkg -i 安装包名称

# 配置快捷键
sudo apt install wmctrl
# 进入 设置 >> 键盘 >> 键盘快捷键 >> 添加自定义快捷键
# 命令填写 ulauncher-toggle

#  设置开机启动（可在优化里添加）
systemctl --user enable --now ulauncher

```

## 1. albert软件启动器(不支持wayland 不建议使用)

[官网](https://albertlauncher.github.io/setup/)
[安装方法](https://blog.csdn.net/weixin_42405819/article/details/135025334)
[QT安装]( https://blog.csdn.net/admin280/article/details/134476901)
error while loading shared libraries 的解决方案
https://blog.csdn.net/fengyuyeguirenenen/article/details/130864664

```shell
   # 依赖安装
   sudo apt install cmake pybind11-dev libmuparser-dev libqalculate-dev libxcb-cursor0 libgl1-mesa-dev
  
   # 安装Qt6
   # 前置安装
   sudo apt install build-essential libgl1-mesa-dev
   # 安装qt6
   sudo apt install qt6*
   # 验证
   qmake --version
   # 可能遇到的问题
   qmake: could not find a Qt installation of ‘’
   # 方案1：所有用户可用
   qtchooser -install qt6 $(which qmake6)
   sudo mv ~/.config/qtchooser/qt6.conf /usr/share/qtchooser/qt6.conf
   sudo mkdir -p /usr/lib/$(uname -p)-linux-gnu/qt-default/qtchooser
   sudo ln -n /usr/share/qtchooser/qt6.conf /usr/lib/$(uname -p)-linux-gnu/qt-default/qtchooser/default.conf
   # 方案2：当前用户可用
   qtchooser -install qt6 $(which qmake6)
   export QT_SELECT=qt6
   
   # 安装albert
   git clone --recursive https://github.com/albertlauncher/albert.git
   cd albert
   # 若 albert/plugins clone失败，则重新跳转复制链接克隆
   rm -rf plugins
   git clone --recursive https://github.com/albertlauncher/plugins.git
   cd lib
   rm -rf QHotkey
   git clone --recursive https://github.com/Skycoder42/QHotkey.git
   export Qt6_DIR=/home/codingai/Qt/6.5.0/gcc_64/lib/cmake/Qt6
   
   # 构建安装
   cd ..
   cmake -B build -DCMAKE_BUILD_TYPE=Debug
   cmake --build build -j32
   sudo cmake --install build
   
   # 配置环境
   export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/workspace/env/albert/build/lib:/home/codingai/Qt/6.5.0/gcc_64/lib

```

## 2. keyd 键盘映射

[官网](https://github.com/rvaiya/keyd)

```shell
    # 安装
    git clone https://github.com/rvaiya/keyd
    cd keyd
    make && sudo make install
    # 启动
    sudo systemctl enable keyd && sudo systemctl start keyd
    # 或者
    sudo add-apt-repository ppa:keyd-team/ppa
    sudo apt update
    sudo apt-get install keyd
    
    # 启动
    sudo systemctl enable keyd && sudo systemctl start keyd
    # 编辑i配置文件
    /etc/keyd/default.conf
    # 检查配置文件是否正确
    sudo systemctl enable keyd
    # 重新加载配置文件
    sudo keyd reload 
```

## 3. gone-tweaks 优化

```shell
    sudo apt update
    sudo apt install gnome-tweaks
```

## 4. libinput 滚动速度等手势修改

[官网](https://gitlab.com/warningnonpotablewater/libinput-config)

```shell
   # 依赖
   sudo apt install meson
   
   git clone https://gitlab.com/warningnonpotablewater/libinput-config.git
   meson build
   cd build
   # meson configure -Dnon_glibc=true
   # meson configure -Dshitty_sandboxing=true
   ninja
   sudo ninja install
   
   # 配置 
   sudo vi /etc/libinput.conf

   # 卸载
   cd build
   sudo ninja pre-uninstall uninstall
```

## 5. Chrome浏览器 设置wayland
   打开[设置](chrome://flags/) ，搜索 `Preferred Ozone platform`，设置为 `wayland`。

## 6. 微信
   [下载地址](https://software.openkylin.top/openkylin/yangtze/pool/all/)
```shell
   # 下载
   wget https://software.openkylin.top/openkylin/yangtze/pool/all/wechat-beta_1.0.0.238_amd64.deb
   # 安装
   sudo dpkg -i wechat-beta_1.0.0.238_amd64.deb
```

## 7. fusuma手势设置
   [官网](https://github.com/iberianpig/fusuma)
   [配置](https://www.cnblogs.com/hh9515/p/17692258.html)

## 8. 输入法设置
   ubuntu22.04安装 Fcitx5输入法，并解决 chrome启用wayland后无法输入中文问题。
   https://blog.p2hp.com/archives/11752