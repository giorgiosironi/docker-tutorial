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

`ps -fe` will show the namespaced process tree inside the container. How long do you expect the output to be?

`docker exec -it $container /bin/bash` will open another shell in the same container.

What happens if I run `vim /tmp/new_file.txt`?

What happens if I run `touch /tmp/new_file.txt`, exit the container and start a new one?

## 2. Container images

`docker build -f Dockerfile.hello -t elifesciences/hello .` will build a local image tagged `elifesciences/hello`. Take a look at the [Dockerfile.

`docker run elifesciences/hello` will run the default command of the image, which is a `cat` command.

What do I have to do if I want to run this code on another laptop?

## 3. Users

`docker run -it -u 33 elifesciences/hello /bin/bash` runs a shell for the `www-data` standard user, which is unprivileged.

What happens when I run `echo "IMPACT FACTOR" > message.txt`?

## 4. Volumes

`docker run -it -v $(pwd):/code elifesciences/hello /bin/bash` will run a container where the current folder is mounted as a volume. Incidentally, it also means the volume's content will persist beyond the lifetime of the container.

`mount | grep /code` will show the mount point.

What does `ls -ld /code` shows? What does that 1000 (or other number) mean?

Can I `touch /code/42.txt`?

## 5. Base images

`docker build -f Dockerfile.derived -t elifesciences/hello .` will create an image derived from our own base PHP CLI image.  

`docker run elifesciences/hello` will now use the new image, try it.

But what if we want to use local modifications? Without rebuilding the container every time we save a file?

Make a local change. What happens when I run `docker run -v $(pwd)/hello.php:/srv/hello/hello.php elifesciences/hello`?

## 6. Ports

Run `docker run -p 8003:8003 elifesciences/hello php -S 0.0.0.0:8003 -t .` to start a PHP container listening on port 8003 for HTTP connections, and forward the host's 8003 port to it.

This is a blocking command, so won't exit until Ctrl-C is pressed.

Now `curl -v localhost:8003/hello.php` should answer with `200 OK`.

Do Docker containers have their own port space? What happens if while the first command is running, I start another identical one? What do I have to change to make them coexist?

## 7. Layer cache

Build a sample heavy image (taking more than 60 seconds to build) with `docker build -f Dockerfile.heavy -t elifesciences/heavy .`

Now change `after.txt` by adding a line or similar, and rebuild. What happens?

Change `before.txt` instead, and rebuild. What happens?

Run `docker build -f Dockerfile.heavy -t elifesciences/heavy --no-cache .` to skip the layer cache.

