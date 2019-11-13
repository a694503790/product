## volumes
> 默认目录 `/var/lib/docker/volumes/`

> A given volume can be mounted into multiple containers simultaneously

!> However, in Docker 17.06 and higher, we recommend using the `--mount` flag.

优势：
1. Volumes are easier to back up or migrate than bind mounts.
2. You can manage volumes using Docker CLI commands or the Docker API.
3. Volumes work on both Linux and Windows containers.
4. Volumes can be more safely shared among multiple containers.
5. Volume drivers let you store volumes on remote hosts or cloud providers, to encrypt the contents of volumes, or to add other functionality.
New volumes can have their content pre-populated by a containe

语法：
```
--mount 'type= bind  or volume or tmpfs,source or src = the name of volume or /,destination or dst or target = /root/??? (readonly)' 

```

例子：
```
[root@centos7 ~]# docker run -d -t -i --mount 'src=qiumin,dst=/root/qiumin' -p 6399:6379 redis redis-server
4b5ca5f99a123f4dd918b9413e7f909864d4ab8ca06ae9b8ea7ebf7fd14d056a
```

```
[root@centos7 ~]# docker run -d -t -i --mount 'src=qiumin,dst=/root/qiumin,readonly' -p 6390:6379 redis redis-server
f23b6ef97cb29d9fce47ff492dc03bd075310971ce2958da85d8f6ba99378ba3
```

## bind mounts
> Bind mounts may be stored anywhere on the host system.

!> &nbsp; &nbsp; &nbsp; &nbsp;One side effect of using bind mounts, for better or for worse, is that you can change the host filesystem via processes running in a container, including creating, modifying, or deleting important system files or directories. 

?> If you use `-v` or `--volume` to bind-mount a file or directory that does not yet exist on the Docker host, `-v` creates the endpoint for you. It is always created as a directory.

?> If you use `--mount` to bind-mount a file or directory that does not yet exist on the Docker host, Docker does not automatically create it for you, but generates an error.

!> 为了避免混淆，建议使用`volume`的时候用`--mount`，用`bind mount`的时候用`-v`

例子：
```
[root@centos7 ~]# docker run -d -t -i -v /root/docker:/root/abc -p 6391:6379 redis redis-server
a18d3d5bd06890e0a79cd99c2d89c21bbef208c2bf7c69b51cdb31cdb92bd0eb
[root@centos7 ~]#
```

## tmpfs
> tmpfs mounts are stored in the host system’s memory only, and are never written to the host system’s filesystem.

?> This functionality is only available if you’re running Docker on Linux.

!> 为了避免混淆，建议使用`tmpfs`的时候用`--tmpfs`

例子：
```
[root@centos7 ~]# docker run -d -t -i --tmpfs /root/abc -p 6392:6379 redis redis-server
c1d4b2aa19d3d8aa7e955b90f52598b9a61be678f7385eea8edd549e49f52677
```