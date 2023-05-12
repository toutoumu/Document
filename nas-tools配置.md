## [alist安装]

```shell
docker run -d \
--name alist-1 \
--restart unless-stopped \
-v /share/Container/alist/data:/opt/alist/data \
-e WEBUI_PORT=5244 \
-p 5244:5244 \
xhofe/alist:latest
```

## [onelist安装]

[nas-tools之外的另一种追剧方式](https://sleele.com/2020/03/16/%E9%AB%98%E9%98%B6%E6%95%99%E7%A8%8B-%E8%BF%BD%E5%89%A7%E5%85%A8%E6%B5%81%E7%A8%8B%E8%87%AA%E5%8A%A8%E5%8C%96/comment-page-4/)

```shell
docker run -d \
--name onelist-1 \
-e PUID=0 \
-e PGID=0 \
-e TZ=Asia/Shanghai \
-e WEBUI_PORT=5245 \
-p 5245:5245 \
-v /share/Container/onelist/config:/config \
--add-host api.themoviedb.org:13.224.161.90 \
msterzhang/onelist:latest
```

## [nas-tools 套件配置](https://zhuanlan.zhihu.com/p/563493451)

[最受欢迎的十个BT种子下载站](https://www.tianyuu.com/article/detail/id/1564.html)

1. 如果运行报错试试这个

```shell
pip3 install pikpakapi
```

2. 如果5015处理器播放失败请关掉 VPP色调映射 参考以下网址

[jellyfin N5105 硬解配置【解决】该客户端与媒体不兼容，服务器未发送兼容的媒体格式](https://www.bilibili.com/read/cv20154506)

[jellyfin N5105 硬解配置【解决】该客户端与媒体不兼容，服务器未发送兼容的媒体格式](https://www.bilibili.com/read/cv20154506)

## [安装nas-tools](https://hub.docker.com/r/diluka/nas-tools)

*请使用2.9.1版本 admin/password

1. 创建目录

```shell
mkdir nasTools/config
mkdir video #这个目录 nas-tools,qbittorrent,jackett共用
```

### 安装nas-tools
* 账号 admin/password

```shell
docker run -d\
--name nas-tools-1 \
--hostname nas-tools \
-p 3000:3000 \
-v /share/Container/nastools/config:/config \
-v /video:/video \
-e WEBUI_PORT=3000\
-e PUID=501 \#使用 id admin 命令查询GID和UID
-e PGID=20 \#使用 id admin 命令查询GID和UID
-e UMASK=000 \
-e NASTOOL_AUTO_UPDATE=false \#是否自动更新\
-e NASTOOL_CN_UPDATE=false   \#是否使用国内镜像\
diluka/nas-tools:latest
```

3. 配置

* TMDB Api key 
06bd93b81405efd92fcc86a3b6bc5bd1

* 媒体库配置
/video/links/movie/
/video/links/tv/

目录同步
/video/movie(源目录) /video/links/movie(目标目录)
/video/tv(源目录) /video/links/tv(目标目录)

## [qbittorrent安装](https://hub.docker.com/r/linuxserver/qbittorrent)

* 账号 admin/adminadmin

1. 创建目录

```shell
mkdir qbittorrent/config
```

```shell
docker run -d \
--name=qbittorrent-1 \
-e PUID=501\
-e PGID=20\
-e TZ=Etc/UTC\
-e WEBUI_PORT=8080\
-p 8080:8080\
-p 6880:6880\
-p 6880:6880/udp\ 
-v /share/Container/Qbittorrent/config:/config #配置文件目录\ 
-v /video:/downloads #下载目录\ 
--restart unless-stopped\ 
linuxserver/qbittorrent:latest
```

# [jackett](https://hub.docker.com/r/linuxserver/jackett)

1. 创建目录

```shell
mkdir jackett/config
```

2. 安装

```shell
docker run -d --name=jackett-1 \
-e PUID=501 \
-e PGID=20 \
-e TZ=Etc/UTC \
-e AUTO_UPDATE=true \
-p 9117:9117 \
-v /share/Container/jackett/config:/config \
-v /video:/downloads \
--restart unless-stopped \
linuxserver/jackett:latest
```

# [flaresolverr安装](https://hub.docker.com/r/flaresolverr/flaresolverr)

配合jackett使用

```shell
docker run -d \
  --name=flaresolverr-1 \
  -p 8191:8191 \
  -e LOG_LEVEL=info \
  --restart unless-stopped \
  flaresolverr/flaresolverr:latest
```

# [jellyfin 安装](https://post.smzdm.com/p/apv8gg72/)

nyanmisaka/jellyfin 镜像支持硬解

1. 创建目录

```shell
mkdir jellyfin/config
mkdir jellyfin/cache
```

2. 安装jellyfin

```shell
docker run -d\
 --name jellyfin-1 #容器名字\
 --privileged=true #特殊权限必须开启,否则无法硬解\
 --volume /share/Container/jellyfin/config:/config #配置文件路径\
 --volume /share/Container/jellyfin/cache:/cache   #缓存路径\
 --volume /share/:/media #媒体文件路径,决定了刮削范围\
 --add-host=api.themoviedb.org:13.225.174.30 #host配置解决\
 --add-host=image.tmdb.org:13.227.73.57      #host配置解决\
 --add-host=www.themoviedb.org:54.192.22.105 #host配置解决\
 --net=bridge #默认为bridge,也可改为host,不能映射端口\
 --expose 8096 #导出端口\
 -e WEBUI_PORT=8095 #webUI端口\
 -p 8095:8096 #端口映射,解决端口冲突,因为emby也用了8096\
 -p 8920:8920 #端口映射\
 --restart unless-stopped #启动策略\
 --device /dev/dri/renderD128:/dev/dri/renderD128 #硬解驱动\
 --device /dev/dri/card0:/dev/dri/card0 #硬解驱动\
 nyanmisaka/jellyfin:latest #哪个镜像
 
 ```

3. 配置豆瓣刮削,安装`douban-openapi-server`插件, 并配置插件
   [插件库地址](https://github.com/caryyu/jellyfin-plugin-repo/raw/master/manifest-us.json)
```shell
docker run --rm -d \
--name douban-openapi-server-1 \
-p 6000:5000 \
--restart unless-stopped \
caryyu/douban-openapi-server:latest
```

# docker 镜像备份

Docker支持使用docker save和docker load命令来实现镜像备份和恢复。具体步骤如下：

1. 使用docker save命令将需要备份的镜像保存至tar文件。
   docker save -o <备份文件名>.tar <镜像名>:<镜像标签>
   例如：
   docker save -o myimage_backup.tar myimage:latest
2. 将备份文件传送到保存位置。
   可以使用FTP、SCP等文件传输协议。
3. 使用docker load命令从tar文件中还原镜像。
   docker load -i <备份文件名>.tar
   例如：
   docker load -i myimage_backup.tar
   注意事项：
1. 备份文件中保存的是完整的镜像，包括镜像的所有层和元数据。
2. 备份文件可以压缩以减少文件大小，例如可以使用gzip命令将备份文件压缩为.tar.gz文件。
3. 镜像备份需要占用一定的存储空间，在备份和恢复时需要考虑存储空间的限制。
