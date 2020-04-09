# Docker 常用指令

```
查看本地运行image
docker ps
```

```
查看下载的全部images
docker images
docker images -a
```

```
查看image依赖的父image
docker image inspect --format='{{.RepoTags}} {{.Id}} {{.Parent}}' b4e3cb94c6a2
```

```
删除image
docker rmi 927f7021970b
强制删除image
docker rmi -f 927f7021970b
```

```
停止image
docker stop 927f7021970b
```

```
查看image日志
docker logs 927f7021970b
```