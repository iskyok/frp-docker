# frp-docker
https://github.com/fatedier/frp
# 服务器端安装
```
git clone https://github.com/myjacksun/frp-docker
```

### 制作自己的frp-docker镜像
1.修改服务器配置文件（frps.ini）   
2.执行
```
bash build.sh
```
 
### 启动容器
1.修改服务器配置文件：docker-compose.yml   
2.启动命令:  
```
docker-compose up -d
```

## 测试
打开浏览器通过 http://[server_addr]:7500 访问 dashboard 界面，用户名密码默认为 xxxx。

## 配置nginx端口代理
```
 cp nginx_frp.conf /etc/nginx/sites-enabled/frp.conf
 sudo /etc/init.d/nginx restart
```
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

## 其他
### Docker 批量删除中间镜像缓存
```
docker ps -a | grep "Exited" | awk '{print $1 }'|xargs docker stop
docker ps -a | grep "Exited" | awk '{print $1 }'|xargs docker rm
docker images|grep none|awk '{print $3 }'|xargs docker rmi
```