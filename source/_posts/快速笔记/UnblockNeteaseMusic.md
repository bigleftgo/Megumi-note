---
title: 网易云灰色音乐解锁教程（代理模式）
toc: true
math: true
---

# 服务器端基础准备

- 安装nodejs

  - 此处不做教学

- 下载项目
  
  ```shell
git clone https://github.com/nondanee/UnblockNeteaseMusic.git #克隆项目仓库
  cd UnblockNeteaseMusic #进入项目根目录
  ```

> 启动对HTTPS支持(mac与iPhone客户端仅支持https)
> ```shell
> 生成 CA 私钥
> openssl genrsa -out ca.key 2048
> 生成 CA 证书 ("YOURNAME" 处填上你自己的名字)
> openssl req -x509 -new -nodes -key ca.key -sha256 -days 1825 -out ca.crt -subj "/C=CN/CN=UnblockNeteaseMusic Root CA/O=YOURNAME"
> 生成服务器私钥
> openssl genrsa -out server.key 2048
> 生成证书签发请求
> openssl req -new -sha256 -key server.key -out server.csr -subj "/C=CN/L=Hangzhou/O=NetEase (Hangzhou) Network Co., Ltd/OU=IT Dept./CN=*.music.163.com"
> 使用 CA 签发服务器证书
> openssl x509 -req -extfile <(printf "extendedKeyUsage=serverAuth\nsubjectAltName=DNS:music.163.com,DNS:*.music.163.com") -sha256 -days 365 -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt
> ```
> 上述步骤完成后将服务器私钥 (server.key) 和服务器证书 (server.crt) 拷贝到仓库中覆盖原有文件

- 启动服务

  - 在刚才目录下执行

  ```shell
  node app.js -p <http端口号>:<https端口号> 
  ```

  > 此处HTTP和HTTPS端口号自行填写，保证不要冲突和相同即可

# 客户端设置

- 在Clash/Shadowsocks/Surge中新增代理服务器

  > 以surge举例：
  >
  > ![image-20201201134314343](http://222.65.137.121:9702/images/2020/12/01/20201201134314.png)
  >
  > 此处如果在服务器端和客户端都启用了证书就可以启用TLS认证

- 设置应用代理

  - ![image-20201201134519840](http://222.65.137.121:9702/images/2020/12/01/20201201134519.png)

- 安装客户端证书（仅mac与iPhone）

  > mac安装证书
  >
  > 1. 打开钥匙串访问
  >
  >    ![image-20201201134736983](http://222.65.137.121:9702/images/2020/12/01/20201201134737.png)
  >
  > 2. 给出权限
  >
  >    ![image-20201201134843505](http://222.65.137.121:9702/images/2020/12/01/20201201134843.png)
  >
  > iPhone配置证书
  >
  > 1. https://raw.githubusercontent.com/nondanee/UnblockNeteaseMusic/master/ca.crt
  >
  >    用手机直接点开此链接安装证书
  >
  > 2. 在设置-通用-证书中选择刚刚下载的证书安装
  >
  >    ![IMG_6971](http://222.65.137.121:9702/images/2020/12/01/20201201135922.png)
  >
  > 3. 在设置-通用-关于本机-证书信任设置中给出权限
  >
  >    ![CDF1A305BB53EAC09BCC4366DAA2F4C4](http://222.65.137.121:9702/images/2020/12/01/20201201135933.png)

- 此时启动网易云客户端就可以播放灰色歌曲了

> 由于原理是替换灰色歌曲源为其他平台，可能会导致歌曲不匹配现象

