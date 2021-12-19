# sidewave/docker
Public PHP Docker images for local development.

## Build new version
```shell
docker buildx create --use && \
docker buildx build \
--platform linux/arm64/v8,linux/amd64 \
--build-arg PHP_VERSION=7.1 \
--push \
--tag sidewave/php:7.1-apache . && \
docker buildx stop && \
docker buildx rm

docker buildx create --use && \
docker buildx build \
--platform linux/arm64/v8,linux/amd64 \
--build-arg PHP_VERSION=7.4 \
--push \
--tag sidewave/php:7.4-apache . && \
docker buildx stop && \
docker buildx rm

docker buildx create --use && \
docker buildx build \
--platform linux/arm64/v8,linux/amd64 \
--build-arg PHP_VERSION=8.0 \
--push \
--tag sidewave/php:8.0-apache . && \
docker buildx stop && \
docker buildx rm

docker buildx create --use && \
docker buildx build \
--platform linux/arm64/v8,linux/amd64 \
--build-arg PHP_VERSION=8.1 \
--push \
--tag sidewave/php:8.1-apache . && \
docker buildx stop && \
docker buildx rm
```
