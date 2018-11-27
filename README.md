
##
由于团队之前用的ngrok经常断网，抽空就换成了七牛团队开发的frp，目前用起来比较稳定。以下是docker版本服务器安装和本地配置方式。


## frp-docker
https://github.com/fatedier/frp
# 服务器端安装
```
git clone https://github.com/myjacksun/frp-docker
```

### 制作自己的frp-docker镜像
1. 修改服务器配置文件frps.ini的subdomain_host
2. 修改docker-compose.yml中的VIRTUAL_HOST
3. 执行
```
bash build.sh
```
 
### 启动容器
1. 修改服务器配置文件：docker-compose.yml
2. 启动命令:
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
## 配置域名解析
```
A	frp      指向IP地址
A	*.frp    指向IP地址
```

# 客户端安装
## mac安装
### 拷贝二进制文件
```code
wget https://github.com/fatedier/frp/releases/download/v0.21.0/frp_0.21.0_darwin_amd64.tar.gz   
tar xzvf frp_0.21.0_darwin_amd64.tar.gz
cd frp_0.21.0_darwin_amd64/
cp frpc /usr/local/bin/
chmod +x frpc
```
### 拷贝配置文件
```code
mkdir   /etc/frpc/ 
cp frpc.ini /etc/frpc/frpc.ini
```
进入修改域名和端口
```
[web-jacksun] #不同用户修改不同的服务名称
type = http
local_port = 3000 #不同用户映射不同端口
subdomain = jacksun #不同用户不同二级域名
```

## 客户端启动
```code
nohup frpc -c /etc/frpc/frpc.ini &
```

## mac设置开机自启动
```code
cp  com.frpc.plist ~/Library/LaunchAgents
sudo launchctl load  ~/Library/LaunchAgents/com.frpc.plist
sudo launchctl start com.frpc
```

## 测试访问
名称.frp.域名xxx.com

## 其他
### Docker 批量删除中间镜像缓存
```
docker ps -a | grep "Exited" | awk '{print $1 }'|xargs docker stop
docker ps -a | grep "Exited" | awk '{print $1 }'|xargs docker rm
docker images|grep none|awk '{print $3 }'|xargs docker rmi
```
## 更新日志
增加支持https访问