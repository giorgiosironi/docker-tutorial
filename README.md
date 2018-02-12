## 0. Setup

- [Docker CE for Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
- [Docker for Mac](https://docs.docker.com/docker-for-mac/install/)

`docker -v` should return `Docker version 17.12.0-ce, ...`

## 1. Containers

`docker run hello-world`

Show daemon running on host.

[Hello world image](https://hub.docker.com/_/hello-world/)

`docker run -it ubuntu /bin/bash` will start a shell running inside an Ubuntu container.

Notice how the image is downloaded automatically, layer by layer.

`ps fe` will show the namespaced process tree inside the container (PID 1)

`docker exec -it $container /bin/bash` will open another shell in the same container.

## 2. Container images

`Dockerfile.hello`

`docker build -f Dockerfile.hello -t elifesciences/hello .` will build a local image tagged `elifesciences/hello`. Take a look at the [Dockerfile.

`docker run elifesciences/hello` will run the default command of the image, which is a `cat` command.

## 3. Users

`docker run -it -u 33 elifesciences/hello /bin/bash` runs a shell for the `www-data` standard user, which is unprivileged.

`echo "IMPACT FACTOR" > message.txt` will return a permission denied error.

## 4. Volumes

`docker run -it -v $(pwd):/code elifesciences/hello /bin/bash` will run a container where the current folder is mounted as a volume.

`mount | grep /code` will show the mount point.

## 5. Base images

## 6. Layer cache
