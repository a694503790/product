## Tomcat
[官方镜像](https://hub.daocloud.io/repos/47f127d0-8f1d-4f91-9647-739cf3146a04)
```
```

## Mysql
[官方镜像](https://hub.daocloud.io/repos/fa51c1d6-9dc2-49d9-91ac-4bbfc24a1bda)
```
```

## Redis
[官方镜像](https://hub.daocloud.io/repos/beb958f9-ffb6-4f68-817b-c17e1ff476c3)

```
[root@centos7 docker]# docker pull redis
[root@centos7 docker]# docker run -d -t -i -p 6379:6379 redis redis-server
b835539d19741f0e0d7a0bf2354ccf2f265f174cb2e4439c461b8b7dd790d00b
[root@centos7 src]# ./redis-cli
127.0.0.1:6379> info
# Server
redis_version:5.0.6
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:24cefa6406f92a1f
redis_mode:standalone
os:Linux 3.10.0-862.el7.x86_64 x86_64
```

## Memcached
[官方镜像](https://hub.docker.com/_/memcached)
```
[root@centos7 docker]# docker pull memcached
Using default tag: latest
latest: Pulling from library/memcached
Digest: sha256:fcfed7e429eea2e533dfcbc1ce7a3cddfbe91480677aa59d8f2efeddcd216fa4
Status: Image is up to date for memcached:latest
docker.io/library/memcached:latest
[root@centos7 docker]# docker run  -d -p 11211:11211 memcached
b465b1c963a12f20d33482e5acc7639887773b9ee09facfeec9ba5791935b91b
[root@centos7 docker]# telnet 127.0.0.1 11211
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
stats
STAT pid 1
STAT uptime 155
STAT time 1572341562
STAT version 1.5.19
```

## Elasticsearch
[官方镜像](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html)
```
```

## Kibana
[官方镜像](https://www.elastic.co/guide/en/kibana/7.4/docker.html)
```
```

## ActiveMQ
[非官方镜像]()
```
```

