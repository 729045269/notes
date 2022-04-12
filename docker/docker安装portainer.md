# docker安装portainer

```


拉取镜像：docker pull portainer/portainer

本地模式：docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock --restart=always --name prtainer portainer/portainer


```

