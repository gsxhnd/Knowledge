# 5. 仓库

一个容易混淆的概念是注册服务器（Registry）。 实际上注册服务器是管理仓库的具体服务器，每个服务器上可以有多个仓库，而每个仓库下面有多个镜像。从这方面来说， 仓库可以被认为是一个具体的项目或目录。例如对于仓库地址`registry.hub.docker.com/ubuntu` 来说，`registry.hub.docker.com`是注册服务器地址，`ubuntu`是仓库名。大部分时候，并不需要严格区分这两者的概念。

## 5.1. Docker Hub

目前 Docker 官方维护了一个公共仓库 Docker Hub：[https://hub.docker.com](https://hub.docker.com/)

我们可以在 Docker Hub 上完成注册。这样就可以使用 Docker Hub 来托管我们的镜像了。

通过`docker search`命令来查找官方仓库中的镜像，并利用`docker pull` 命令来将它下载到本地。

```text
$ docker search alpine
NAME                                   DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
alpine                                 A minimal Docker image based on Alpine Linux…   7869      [OK]
mhart/alpine-node                      Minimal Node.js built on Alpine Linux           483
anapsix/alpine-java                    Oracle Java 8 (and 7) with GLIBC 2.28 over A…   473                  [OK]
frolvlad/alpine-glibc                  Alpine Docker image with glibc (~12MB)          268                  [OK]
alpine/git                             A  simple git container running in alpine li…   191                  [OK]
```

## 5.2. 使用国内镜像

参考地址：[https://www.docker-cn.com/registry-mirror](https://www.docker-cn.com/registry-mirror)

临时性的使用：

```text
$ docker pull registry.docker-cn.com/library/alpine:3.13.6
3.13.6: Pulling from library/alpine
4e9f2cdf4387: Pull complete
Digest: sha256:2582893dec6f12fd499d3a709477f2c0c0c1dfcd28024c93f1f0626b9e3540c8
Status: Downloaded newer image for alpine:3.13.6
docker.io/library/alpine:3.13.6
```

永久性的使用：

修改 `/etc/docker/daemon.json` 文件（没有的话可以手动创建，需要通过`root`用户操作）并添加上 registry-mirrors 键值。

```javascript
{
  "registry-mirrors": ["https://registry.docker-cn.com"]
}
```

修改保存后重启 Docker 服务以使配置生效。

```text
$ systemctl restart docker
1
```

## 5.3. Docker 私有仓库搭建

### 5.3.4

```text
$ curl -XGET http://registry:5000/v2/_catalog
$ curl -XGET http://registry:5000/v2/image_name/tags/list
```
