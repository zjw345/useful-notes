
## Dockerfile for linux arm64 mattermost
mattermost version: 10.10.1

## Setup steps
```sh
cd ~/build/mattermost
wget https://raw.githubusercontent.com/mattermost/mattermost/master/server/build/entrypoint.sh
wget https://raw.githubusercontent.com/zjw345/useful-notes/refs/heads/v10.10.1/mattermost/Dockerfile
```

Build arm64 docker image
```
docker build . -t mattermost-arm:10.10.1
```

---

Mattermost working directory setup
```bash
cd /opt/mattermost
wget https://raw.githubusercontent.com/zjw345/useful-notes/refs/heads/v10.10.1/mattermost/compose.yaml
mkdir -p mattermost/{config,data,logs,plugins,bleve-indexes}
mkdir -p mattermost/client/plugins
chown 2000:2000 -R .
```

Spin up container
```
docker compose up
```
If it looks okay, `Ctrl+C` then run the container in detach mode.
```
docker compose up -d
```

Cheers!





宝塔反向代理配置：
```
#PROXY-START/
# 添加 WebSocket 专用处理块（关键修复）
location ~ /api/v[0-9]+/(users/)?websocket {
    proxy_pass http://127.0.0.1:8065;
    proxy_set_header Host $http_host; # 修正为 $http_host
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme; # 添加协议头
    proxy_set_header Upgrade $http_upgrade; # WebSocket 必需
    proxy_set_header Connection "upgrade"; # 固定值（关键修复）
    proxy_http_version 1.1;
    # WebSocket 需要较长的超时时间
    proxy_read_timeout 600s;
}
# 主代理配置（修正 Host 头）
location ^~ /
{
    proxy_pass http://127.0.0.1:8065;
    proxy_set_header Host $http_host; # 关键修复：改为 $http_host
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme; # 添加协议头
    proxy_set_header REMOTE-HOST $remote_addr;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection $connection_upgrade;
    proxy_http_version 1.1;
    add_header X-Cache $upstream_cache_status;
    #Set Nginx Cache
    set $static_fileICqKmhWv 0;
    if ( $uri ~* "\.(gif|png|jpg|css|js|woff|woff2)$" )
    {
     set $static_fileICqKmhWv 1;
     expires 1m;
    }
    if ( $static_fileICqKmhWv = 0 )
    {
    add_header Cache-Control no-cache;
    }
}
#PROXY-END/
```
