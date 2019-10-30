### 描述
?> 最简单的`dockerfiles`

?> You use the -f flag with docker build to point to a Dockerfile anywhere in your file system

```
$ docker build -f /path/to/a/Dockerfile .
```

?> To tag the image into multiple repositories after the build, add multiple -t parameters when you run the build command

```
$ docker build -t shykes/myapp:1.0.2 -t shykes/myapp:latest .
```

```
FROM busybox
COPY /hello /
RUN cat /hello
```
### FROM
?> Whenever possible, use current official images as the basis for your images.
### LABEL
?> You can add labels to your image to help organize images by project
### RUN
?> Always combine RUN apt-get update with apt-get install in the same RUN statement.
```
FROM alpine
RUN export ADMIN_USER="mark" \
    && echo $ADMIN_USER > ./mark \
    && unset ADMIN_USER
CMD sh
```

!> `pipe`

```
RUN set -o pipefail && wget -O - https://some.site | wc -l > /number
```
### CMD
?> The CMD instruction should be used to run the software contained by your image, along with any arguments.
### EXPOSE
?> The EXPOSE instruction indicates the ports on which a container listens for connections. 
### ENV
```
ENV PG_MAJOR 9.3
ENV PG_VERSION 9.3.4
RUN curl -SL http://example.com/postgres-$PG_VERSION.tar.xz | tar -xJC /usr/src/postgress && …
ENV PATH /usr/local/postgres-$PG_MAJOR/bin:$PATH
```
### ADD or COPY
!> the best use for ADD is local tar file auto-extraction into the image, as in `ADD rootfs.tar.xz /`.
?> COPY the file package.json from your host to the present location (.) in your image (so in this case, to /usr/src/app/package.json)
```
COPY requirements.txt /tmp/
RUN pip install --requirement /tmp/requirements.txt
COPY . /tmp/
```
### ENTRYPOINT
?> The best use for ENTRYPOINT is to set the image’s main command, allowing that image to be run as though it was that command 
### VOLUME
?> The VOLUME instruction should be used to expose any database storage area, configuration storage, or files/folders created by your docker container
### USER
!> If a service can run without privileges, use USER to change to a non-root user.
```
RUN groupadd -r redis && useradd -r -g redis redis
USER redis
RUN [ "redis-server" ]
```
### WORKDIR
?> For clarity and reliability, you should always use absolute paths for your WORKDIR. Also, you should use WORKDIR instead of proliferating instructions like `RUN cd … && do-something`, which are hard to read, troubleshoot, and maintain.
!> Use WORKDIR to specify that all subsequent actions should be taken from the directory /usr/src/app in your image filesystem (never the host’s filesystem).
### ONBUILD
?> An ONBUILD command executes after the current Dockerfile build completes