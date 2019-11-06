## bridge
!> The default network driver
> User-defined bridge networks are best when you need multiple containers to communicate on the same Docker host.

### User-defined bridge
特点：
- Containers connected to the same user-defined bridge network automatically expose all ports to each other, and no ports to the outside world. if you run the same application stack on the default bridge network, you need to open both the web port and the database port, using the `-p` or `--publish` flag for each

- On a user-defined bridge network, containers can resolve each other by name or alias.

- To remove a container from the default bridge network, you need to stop the container and recreate it with different network options.

- If different groups of applications have different network requirements, you can configure each user-defined bridge separately, as you create it.

创建bridge
```
[root@centos7 ~]# docker network create my-net
8b7c0587c4a9523fae826f27eb2c6e0191d6c5a35f38ada5421f6988470c2003
```
使用bridge创建容器
```
[root@centos7 ~]# docker run -d -t -i --network my-net -p 6380:6379 redis redis-server
f8518e1aee43e2cacbc9239e58028e89eb16290ad234421832ef4933c3f6f28d
```
使用bridge连接已有容器
```
[root@centos7 ~]# docker container ls
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                                                                                   NAMES
b7e6804e6036        redis                 "docker-entrypoint.s…"   16 seconds ago      Up 15 seconds       0.0.0.0:6381->6379/tcp

[root@centos7 ~]# docker network connect my-net b7e6804e6036
```


### Deault bridge
 Containers connected to the default bridge network can communicate, but only by IP address, unless they are linked using the legacy `--link` flag

```
[root@centos7 ~]# docker run -d --link a2746bd42a75:elasticsearch -p 5601:5601 kibana:6.8.4
```
## host
!> Use the host’s networking directly

> Host is only available for swarm services on Docker 17.06 and higher.

?> Host networks are best when the network stack should not be isolated from the Docker host, but you want other aspects of the container to be isolated.

## overlay
> You can also use overlay networks to facilitate communication between a swarm service and a standalone container, or between two standalone containers on different Docker daemons.

?> Overlay networks are best when you need containers running on different Docker hosts to communicate, or when multiple applications work together using swarm services.

## macvlan
> Macvlan networks allow you to assign a MAC address to a container, making it appear as a physical device on your network

?> Macvlan networks are best when you are migrating from a VM setup or need your containers to look like physical hosts on your network, each with a unique MAC address.

## none
> Disable all networking. Usually used in conjunction with a custom network driver.

## Network plugins
> You can install and use third-party network plugins with Docker.
> Third-party network plugins allow you to integrate Docker with specialized network stacks