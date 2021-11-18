# clash使用方法

以下内容仅用来学习计算机知识！

## windows平台：

从[clash_for_windows_pkg](https://github.com/Fndroid/clash_for_windows_pkg)下载，解压 » 打开软件

General：开启代理

Proxies：选择代理节点

 Profiles：导入自己的配置脚本（或链接）

## Linux平台：

从[ clash](https://github.com/Dreamacro/clash)下载，然后在下载的文件夹里打开终端，执行

```bash
gunzip clash-linux-armv7-xxx.gz
sudo mv clash-linux-armv7-xxx /usr/local/bin/clash
chmod +x /usr/local/bin/clash
```

 每次运行的时候，终端执行

```bash
nohup clash &
```

执行一次 `clash` 命令，Clash 会自动创建 `$HOME/.config/clash/` 配置目录，并新建默认配置文件 config.yaml 和名为 Country.mmdb（需要等待下载） 的 GeoIP 数据库。
接着修改 config,yaml 配置文件

clash配置规则：

```yaml
## 配置文件需要放置在 $HOME/.config/clash/config.yml
# HTTP 代理端口
port: 7890
# SOCKS5 代理端口
socks-port: 7891
# 流量重定向端口,以处理局域网内其他设备所转发的流量。
redir-port: 7892
# 允许局域网的连接（可用来共享代理）
allow-lan: false
# 规则模式：Rule（规则） / Global（全局代理）/ Direct（全局直连）
mode: Rule
# 设置日志输出级别 (默认级别：silent，即不输出任何内容
# 5 个级别：silent / info / warning / error / debug
log-level: silent
# Clash 的 RESTful API
external-controller: '0.0.0.0:9090'
# RESTful API 的口令
secret:  ''
dns:
  enable: true		#启用DNS
  ipv6: false  		#不用ipv6
  listen: 0.0.0.0:53
  enhanced-mode: fake-ip # 或 redir-host
  fake-ip-range: 198.18.0.1/16 # 如果你不知道这个参数的作用，请勿修改
  fake-ip-filter: # fake-ip 白名单列表
  nameserver:
    - 1.1.1.1
  fallback:
    - 'tls://223.5.5.5:853'
  fallback-filter:
    geoip:  true
    ipcidr:
      - 240.0.0.0/4
    
   #下面添节点信息，根据机场文件添加
```

### 添加桌面快捷方式

桌面新建**clash.desktop**文件

```
[Desktop Entry]
Encoding=UTF-8
Name=启动clash
Type=Application
StartupNotify=true
Terminal=true
Exec=nohup clash &
Icon=/home/tiaoteek/Soft/clash/logo.clash.png		#这里是自定义的软件logo地址，可以不要
```



## 树莓派：

使用方法同linux，下载armv7l 版本，可以作为其他设备的透明代理

经测试不需要设置iptables也可以转发流量



