# Apine Docker

## Pull Docker
```
docker pull x3platform/etcd:3.4
```

## Build Docker
```
# login
docker login -u [USERNAME] -p [PASSWORD]

docker buildx build --push --tag x3platform/etcd:3.4 --platform linux/amd64,linux/arm64 --file ./build/Dockerfile ./build/
```
