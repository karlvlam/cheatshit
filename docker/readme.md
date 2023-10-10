# 1. image
```bash
docker images [-a]
docker pull
docker rmi [image id/name/tag]
```

# 2. container
```bash
docker ps [-a]
```

## interactive shell
```bash
docker run -dit ubuntu:22.04 top -b
docker attach [container id/name] [Ctrl-p, Ctrl-q to detach]
```

## environment variable 
```bash
export MYDATA=hello
docker run -e "ABC=123" -e MYDATA alpine env
```

## port mapping
```bash
docker run -d -p 8080:80 nginx
```

# 3. storage

## Volume
```bash
docker volume create [name]
docker volume inspect [name]

docker run -it -v vol1:/data ubuntu:22.04 /bin/bash
```

## host directory
```bash
docker run -it -v $PWD/tmp:/data ubuntu:22.04 /bin/bash
```

# 4. network

```bash
docker network ls
docker network inspect [container id/name]
```

# 5. build, export/import

## export/import
```bash
docker container export [container id/name] | gzip > image.tar.gz
docker import image.tar.gz import-image:001
```

## build
```bash
docker build -f [docker file] -t [image name/tag] [working directory]
```






