## 使用 Docker-cli 部署

### 新版本提醒

AutoBangumi 2.6 版本后支持直接在 WebUI 中配置，你可以选择直接拉起容器再在 WebUI 中配置。老版本的环境变量配置参数会自动迁移

### 创建数据和配置文件夹

为了保证 AB 在每次更新之后数据和配置不丢失，推荐使用 Docker volume 或者 bind mount 进行数据和配置的持久化。

```shell
# 使用 Docker volume
docker volume create AutoBangumi_config
docker volume create AutoBangumi_data

# 使用 bind mount
mkdir "AutoBangumi"
cd "AutoBangumi"
mkdir -p $PWD/config
mkdir -p $PWD/data
```

### 使用 Docker-cli 部署 AutoBangumi

复制以下命令运行即可。

```shell
docker run -d \
  --name=AutoBangumi \
  -v AutoBangumi_config:/app/config \
  -v AutoBangumi_data:/app/data \
  -p 7892:7892 \
  --network=bridge \
  --dns=8.8.8.8 \
  --restart unless-stopped \
  estrellaxd/auto_bangumi:latest
```

如果使用 bind mount，可以自行替换绑定路径。

此时 AB 的 WebUI 会自动运行，但是主程序会处于暂停状态，可以进入 `http://abhost:7892` 进行配置。

此时 AB 会自动把环境变量写入 `config.json` 文件中然后自动运行。

推荐使用 _[Portainer](https://www.portainer.io)_ 等带有 UI 的 Docker 管理器进行进阶部署