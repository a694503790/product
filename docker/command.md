## docker pull
```
[root@centos7 docker]# docker pull redis
Using default tag: latest
latest: Pulling from library/redis
Digest: sha256:fe80393a67c7058590ca6b6903f64e35b50fa411b0496f604a85c526fb5bd2d2
Status: Image is up to date for redis:latest
docker.io/library/redis:latest
[root@centos7 docker]# docker image ls
redis               latest              de25a81a5a0b        12 days ago         98.2MB
```

## docker run

```
[root@centos7 ~]# docker run ubuntu:latest /bin/echo 'Hello world'
Hello world
```

### -t -i
进入交互式`shell`

> -t              : Allocate a pseudo-tty

> -i              : Keep STDIN open even if not attached

```
[root@centos7 ~]# docker run -t -i ubuntu:latest /bin/bash
[root@centos7 ~]# docker exec -t -i 9d9687eb691c bash
```

### --name
```
[root@centos7 ~]# docker run -d  --name new_container1 -i -t ubuntu bash
82c270d1489129b950fb2b0875cdc6283f96fb6fa672fcf5d18f4d591fe9176c
[root@centos7 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
82c270d14891        ubuntu              "bash"              5 seconds ago       Up 4 seconds                            new_container1
```

### --env , -e
```
[root@centos7 ~]# docker run -d --env qiumin=ls -i -t ubuntu bash
9d9687eb691c6c130442be681804236bd8ab6545af6a6a6d0cf6037294eeb250
[root@centos7 ~]# docker exec -t -i 9d9687eb691c bash
root@9d9687eb691c:/# echo $qiumin
ls
```

### --detach , -d
后台运行

```
$ docker run -d -p 80:80 my_image nginx -g 'daemon off;'
```

### --publish , -p
发布一个端口映射

```
 qiumin@QiuMin-MacBook-Pro  ~  docker run -d -p 127.0.0.1:8000:8080/tcp qiumin:2.0
eb45981a385052134980eff589e9791bf53cf6288e83ef3db5c497337f713263
 qiumin@QiuMin-MacBook-Pro  ~ 
 qiumin@QiuMin-MacBook-Pro  ~  docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                      NAMES
eb45981a3850        qiumin:2.0          "npm start"         3 seconds ago       Up 3 seconds        127.0.0.1:8000->8080/tcp   happy_mclean
 qiumin@QiuMin-MacBook-Pro  ~ 
```


### --volume , -v
挂载本地磁盘
```
[root@centos7 ~]# docker run -v /root/myproject/:/newqiumin -i -t ubuntu bash
root@c3eb81904c36:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  newqiumin  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@c3eb81904c36:/# ll|grep newqiumin
drwxr-xr-x.   4 root root   71 Oct 25 05:34 newqiumin/
root@c3eb81904c36:/#
```
挂载卷
```
[root@centos7 ~]# docker volume ls
DRIVER              VOLUME NAME
local               qiu
[root@centos7 ~]# docker run -v qiu:/newqiumin2 -i -t ubuntu bash
root@261ce954446e:/#
root@261ce954446e:/#
root@261ce954446e:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  newqiumin2  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

### --hostname , -h
```
[root@centos7 ~]# docker run  --hostname qiumin -i -t ubuntu bash
root@qiumin:/#
root@qiumin:/# hostname
qiumin
```

## docker stop

- 使用`docker stop`

```
[root@centos7 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
c5e676ca4dac        ubuntu:latest       "/bin/bash"         26 minutes ago      Up 26 minutes                           awesome_ganguly
[root@centos7 ~]# docker stop c5e676ca4dac
c5e676ca4dac
[root@centos7 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[root@centos7 ~]#
```

- 使用`docker container stop`

```
[root@centos7 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
aa14c6139f2d        ubuntu:latest       "/bin/bash"         6 seconds ago       Up 4 seconds                            focused_payne
[root@centos7 ~]# docker container stop aa14c6139f2d
aa14c6139f2d
[root@centos7 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[root@centos7 ~]#
```

## docker attach

!> 使用`Ctrl+q`进入容器并且不退出，如果按`Exit`则会停止容器

```
[root@centos7 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
54dfe06163f4        ubuntu:latest       "/bin/bash"         7 seconds ago       Up 6 seconds                            flamboyant_khorana
93e4ebae2155        ubuntu:latest       "/bin/bash"         7 minutes ago       Up 7 minutes                            amazing_brown
[root@centos7 ~]# docker attach 54dfe06163f4
root@54dfe06163f4:/#
root@54dfe06163f4:/#
root@54dfe06163f4:/# read escape sequence
[root@centos7 ~]#
[root@centos7 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
54dfe06163f4        ubuntu:latest       "/bin/bash"         34 seconds ago      Up 33 seconds                           flamboyant_khorana
93e4ebae2155        ubuntu:latest       "/bin/bash"         7 minutes ago       Up 7 minutes                            amazing_brown
[root@centos7 ~]#
```

## docker exec

?> 与`docker attach`相比，`Exit`不会自动停止容器

```
[root@centos7 ~]# docker exec -i -t 54dfe06163f4 bash
root@54dfe06163f4:/#
root@54dfe06163f4:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@54dfe06163f4:/# exit
exit
[root@centos7 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
54dfe06163f4        ubuntu:latest       "/bin/bash"         5 minutes ago       Up 5 minutes                            flamboyant_khorana
93e4ebae2155        ubuntu:latest       "/bin/bash"         12 minutes ago      Up 12 minutes                           amazing_brown
[root@centos7 ~]#
```

## docker rm

```
[root@centos7 ~]# docker container ls -a|grep 9d9687eb691c
9d9687eb691c        ubuntu              "bash"                   11 minutes ago      Exited (0) 13 seconds ago                          sharp_shockley
[root@centos7 ~]# docker rm 9d9687eb691c
9d9687eb691c
[root@centos7 ~]# docker container ls -a|grep 9d9687eb691c

```

## docker build
### --tag , -t
指定[标签:版本号]
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads/node-bulletin-board/bulletin-board-app   v1  docker build -t qiumin:3.0 .
Sending build context to Docker daemon  46.59kB
Step 1/6 : FROM node:6.11.5
 ---> 852391892b9f
Step 2/6 : WORKDIR /usr/src/app
 ---> Using cache
 ---> a0bf0c06bba1
Step 3/6 : COPY package.json .
 ---> Using cache
 ---> 0f36b154317d
Step 4/6 : RUN npm install
 ---> Using cache
 ---> 74b3ed12b25a
Step 5/6 : COPY . .
 ---> Using cache
 ---> 0912fb0ba247
Step 6/6 : CMD [ "npm", "start" ]
 ---> Using cache
 ---> 806390bae189
Successfully built 806390bae189
Successfully tagged qiumin:3.0
 qiumin@QiuMin-MacBook-Pro  ~/Downloads/node-bulletin-board/bulletin-board-app   v1  docker image ls
REPOSITORY                           TAG                 IMAGE ID            CREATED             SIZE
qiumin                               3.0                 806390bae189        23 hours ago        681MB

```
### --file , -f
指定文件路径
```
[root@centos7 bulletin-board-app]# ll Dockerfile
-rw-r--r--. 1 root root 109 Oct 25 17:40 Dockerfile
[root@centos7 bulletin-board-app]#  docker build -f Dockerfile .
Sending build context to Docker daemon  45.57kB
Step 1/6 : FROM node:6.11.5
 ---> 852391892b9f
Step 2/6 : WORKDIR /usr/src/app
 ---> Running in 366f98d3831b
Removing intermediate container 366f98d3831b
 ---> 3a68380215c0
Step 3/6 : COPY package.json .
 ---> 9fa26871c042
Step 4/6 : RUN npm install
 ---> Running in f923e003fa05
vue-event-bulletin@1.0.0 /usr/src/app
+-- body-parser@1.19.0
| +-- bytes@3.1.0
| +-- content-type@1.0.4
| +-- debug@2.6.9
| | `-- ms@2.0.0
| +-- depd@1.1.2
| +-- http-errors@1.7.2
| | +-- inherits@2.0.3
| | `-- toidentifier@1.0.0
| +-- iconv-lite@0.4.24
| | `-- safer-buffer@2.1.2
| +-- on-finished@2.3.0
| | `-- ee-first@1.1.1
| +-- qs@6.7.0
| +-- raw-body@2.4.0
| | `-- unpipe@1.0.0
| `-- type-is@1.6.18
|   +-- media-typer@0.3.0
|   `-- mime-types@2.1.24
|     `-- mime-db@1.40.0
+-- bootstrap@3.4.1
+-- ejs@2.7.1
+-- errorhandler@1.5.1
| +-- accepts@1.3.7
| | `-- negotiator@0.6.2
| `-- escape-html@1.0.3
+-- express@4.17.1
| +-- array-flatten@1.1.1
| +-- content-disposition@0.5.3
| +-- cookie@0.4.0
| +-- cookie-signature@1.0.6
| +-- encodeurl@1.0.2
| +-- etag@1.8.1
| +-- finalhandler@1.1.2
| +-- fresh@0.5.2
| +-- merge-descriptors@1.0.1
| +-- methods@1.1.2
| +-- parseurl@1.3.3
| +-- path-to-regexp@0.1.7
| +-- proxy-addr@2.0.5
| | +-- forwarded@0.1.2
| | `-- ipaddr.js@1.9.0
| +-- range-parser@1.2.1
| +-- safe-buffer@5.1.2
| +-- send@0.17.1
| | +-- destroy@1.0.4
| | +-- mime@1.6.0
| | `-- ms@2.1.1
| +-- serve-static@1.14.1
| +-- setprototypeof@1.1.1
| +-- statuses@1.5.0
| +-- utils-merge@1.0.1
| `-- vary@1.1.2
+-- method-override@2.3.10
+-- morgan@1.9.1
| +-- basic-auth@2.0.1
| `-- on-headers@1.0.2
+-- vue@1.0.28
| `-- envify@3.4.1
|   +-- jstransform@11.0.3
|   | +-- base62@1.2.8
|   | +-- commoner@0.10.8
|   | | +-- commander@2.20.3
|   | | +-- detective@4.7.1
|   | | | +-- acorn@5.7.3
|   | | | `-- defined@1.0.0
|   | | +-- glob@5.0.15
|   | | | +-- inflight@1.0.6
|   | | | | `-- wrappy@1.0.2
|   | | | +-- minimatch@3.0.4
|   | | | | `-- brace-expansion@1.1.11
|   | | | |   +-- balanced-match@1.0.0
|   | | | |   `-- concat-map@0.0.1
|   | | | +-- once@1.4.0
|   | | | `-- path-is-absolute@1.0.1
|   | | +-- graceful-fs@4.2.3
|   | | +-- mkdirp@0.5.1
|   | | | `-- minimist@0.0.8
|   | | +-- private@0.1.8
|   | | +-- q@1.5.1
|   | | `-- recast@0.11.23
|   | |   +-- ast-types@0.9.6
|   | |   +-- esprima@3.1.3
|   | |   `-- source-map@0.5.7
|   | +-- esprima-fb@15001.1.0-dev-harmony-fb
|   | +-- object-assign@2.1.1
|   | `-- source-map@0.4.4
|   |   `-- amdefine@1.0.1
|   `-- through@2.3.8
`-- vue-resource@0.1.17

npm WARN vue-event-bulletin@1.0.0 No repository field.
Removing intermediate container f923e003fa05
 ---> d48a04bd2790
Step 5/6 : COPY . .
 ---> 6dddbe1db65e
Step 6/6 : CMD [ "npm", "start" ]
 ---> Running in e0a26671fa33
Removing intermediate container e0a26671fa33
 ---> c0fa3542e73f
Successfully built c0fa3542e73f
[root@centos7 bulletin-board-app]# docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
<none>              <none>              c0fa3542e73f        7 seconds ago       681MB
```
### - <<EOF
```
[root@centos7 bulletin-board-app]#  docker build - <<EOF
> FROM busybox
> RUN echo "qiumin"
> EOF
Sending build context to Docker daemon  2.048kB
Step 1/2 : FROM busybox
 ---> 19485c79a9bb
Step 2/2 : RUN echo "qiumin"
 ---> Running in 80554921be40
qiumin
Removing intermediate container 80554921be40
 ---> 521e38b77780
Successfully built 521e38b77780
[root@centos7 bulletin-board-app]# docker imagels
docker: 'imagels' is not a docker command.
See 'docker --help'
[root@centos7 bulletin-board-app]# docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
<none>              <none>              521e38b77780        12 seconds ago      1.22MB
```

## docker diff

Symbol|Description
-|-
A|A file or directory was added
D|A file or directory was deleted
C|A file or directory was changed

```
[root@centos7 bulletin-board-app]# docker exec -i -t b6c163c4efe7 bash
root@b6c163c4efe7:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@b6c163c4efe7:/# touch abc
root@b6c163c4efe7:/# exit
exit
[root@centos7 bulletin-board-app]# docker diff b6c163c4efe7
A /abc
C /root
A /root/.bash_history
```