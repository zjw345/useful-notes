
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
