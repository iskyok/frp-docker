# frp-docker
https://github.com/fatedier/frp

# 客户端安装
## mac安装
### 拷贝二进制文件
```code
wget https://github.com/fatedier/frp/releases/download/v0.21.0/frp_0.21.0_darwin_amd64.tar.gz   
tar xzvf frp_0.21.0_darwin_amd64.tar.gz
cd frp_0.21.0_darwin_amd64/
cp frps /usr/local/bin/
chmod +x frps
```
### 拷贝配置文件(进入修改域名和端口) 
```code
mkdir   /etc/frpc/ 
cp frpc.ini /etc/frpc/frpc.ini
```

## 客户端启动
```code
nohub frpc -c /etc/frpc/frpc.ini &
```

## mac设置开机自启动
```code
cp  com.frpc.plist ~/Library/LaunchAgents
sudo launchctl load  ~/Library/LaunchAgents/com.frpc.plist
sudo launchctl start com.frpc
```