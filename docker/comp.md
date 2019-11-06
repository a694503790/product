## Tomcat
[官方镜像](https://hub.daocloud.io/repos/47f127d0-8f1d-4f91-9647-739cf3146a04)

!> 使用标签`latest`下载的也是`tomcat:8.5.47`

```
[root@centos7 ~]# docker pull tomcat:8.5.47-jdk8-openjdk
[root@centos7 ~]# docker run -i -t -p 80:8080 tomcat:latest
Using CATALINA_BASE:   /usr/local/tomcat
Using CATALINA_HOME:   /usr/local/tomcat
Using CATALINA_TMPDIR: /usr/local/tomcat/temp
Using JRE_HOME:        /usr/local/openjdk-8
```

?> 需将`jar`包`mount`到`webapp`目录,使用`docker logs -f`看日志
```
[root@centos7 ~]# docker run -v /root/docker/portal/webapps:/usr/local/tomcat/webapps -i -t -p 80:8080 tomcat:latest
Using CATALINA_BASE:   /usr/local/tomcat
Using CATALINA_HOME:   /usr/local/tomcat
Using CATALINA_TMPDIR: /usr/local/tomcat/temp
Using JRE_HOME:        /usr/local/openjdk-8
```

## Mysql
[官方镜像](https://hub.docker.com/_/mysql)

!> 注意官方镜像是`mysql8.0`版本的,需要`pull 5.7.28`版本

```
[root@centos7 conf]# docker pull mysql:5.7.28
```
### without my.cnf
```
[root@centos7 mysql]#  docker run -e MYSQL_ROOT_PASSWORD=Meizu123 -d  -p 3306:3306 mysql:5.7.28
[root@centos7 mysql]#  docker container ls
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                               NAMES
79a5509546e7        mysql:5.7.28               "docker-entrypoint.s…"   5 seconds ago       Up 3 seconds        0.0.0.0:3306->3306/tcp, 33060/tcp   kind_rosalind
```
### with my.cnf
```
[root@centos7 mysql]# docker run -v  /root/docker/mysql/conf/abc.conf:/etc/mysql/conf.d/abc.cnf  -v /root/docker/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=Meizu123 -d -p 3306:3306 mysql:5.7.28
79a5509546e78bf51a89b0143943ba44b36dbfcb171dd0eb77777847217a9c7e
[root@centos7 mysql]#  docker container ls
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                               NAMES
79a5509546e7        mysql:5.7.28               "docker-entrypoint.s…"   5 seconds ago       Up 3 seconds        0.0.0.0:3306->3306/tcp, 33060/tcp   kind_rosalind
```
### my.cnf
```
[root@centos7 conf]# cat abc.conf
[client]
default-character-set=utf8mb4
[mysql]
default-character-set=utf8mb4
[mysqld]
########basic settings########
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
init_connect='SET NAMES utf8mb4'
server-id = 1
#port = 3306
#user = mysql
#bind_address = 0.0.0.0
character_set_server=utf8mb4
skip_name_resolve = 1
max_connections = 1000
max_connect_errors = 1000
transaction_isolation = READ-COMMITTED
explicit_defaults_for_timestamp = 1
join_buffer_size = 134217728
tmp_table_size = 67108864
tmpdir = /tmp
max_allowed_packet = 16777216
sql_mode = "STRICT_TRANS_TABLES,NO_ENGINE_SUBSTITUTION,NO_ZERO_DATE,NO_ZERO_IN_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER"
interactive_timeout = 1800
wait_timeout = 1800
read_buffer_size = 16777216
read_rnd_buffer_size = 33554432
sort_buffer_size = 33554432
lower_case_table_names = 1
######log settings########
log_error = error.log
slow_query_log = 1
slow_query_log_file = slow.log
log_queries_not_using_indexes = 1
log_slow_admin_statements = 1
log_slow_slave_statements = 1
log_throttle_queries_not_using_indexes = 10
expire_logs_days = 90
long_query_time = 2
min_examined_row_limit = 100
########replication settings########
master_info_repository = TABLE
relay_log_info_repository = TABLE
log_bin = bin.log
sync_binlog = 1
gtid_mode = on
enforce_gtid_consistency = 1
log_slave_updates
binlog_format = row
relay_log = relay.log
relay_log_recovery = 1
binlog_gtid_simple_recovery = 1
slave_skip_errors = ddl_exist_errors
########innodb settings########
innodb_page_size = 8192
innodb_buffer_pool_size = 6G
innodb_buffer_pool_instances = 8
innodb_buffer_pool_load_at_startup = 1
innodb_buffer_pool_dump_at_shutdown = 1
innodb_lru_scan_depth = 2000
innodb_lock_wait_timeout = 5
innodb_io_capacity = 4000
innodb_io_capacity_max = 8000
innodb_flush_method = O_DIRECT
innodb_file_format = Barracuda
innodb_file_format_max = Barracuda
innodb_undo_logs = 128
innodb_undo_tablespaces = 3
innodb_flush_neighbors = 1
innodb_log_file_size = 4G
innodb_log_buffer_size = 16777216
innodb_purge_threads = 4
innodb_large_prefix = 1
innodb_thread_concurrency = 64
innodb_print_all_deadlocks = 1
innodb_strict_mode = 1
innodb_sort_buffer_size = 67108864
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
[官方镜像](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/docker.html#docker)

!> `elasticsearch`的`6.2`开始即内置了`x-pack`,如果需要使用`x-pack`，需使用`Dockfiles`定制镜像

```
[root@centos7 ]# docker pull elasticsearch:6.8.4
[root@centos7 ~]# docker run -d -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:6.8.4
a2746bd42a7524fdec1cf1ba56e975bcb46548e03f23624b9d2c3ad71a79b16f
[root@centos7 ~]# docker container ls
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                                            NAMES
a2746bd42a75        elasticsearch:6.8.4   "/usr/local/bin/dock…"   9 seconds ago       Up 7 seconds        0.0.0.0:9200->9200/tcp, 0.0.0.0:9300->9300/tcp   flamboyant_swirles
```

## Kibana
[官方镜像](https://www.elastic.co/guide/en/kibana/6.8/docker.html)

!> 如果`elasticsearch`配置了账号密码,需要修改`/usr/share/kibana/config/kibana.yml`


```
[root@centos7 ~]# docker pull kibana:6.8.4
[root@centos7 ~]# docker run -d --link a2746bd42a75:elasticsearch -p 5601:5601 kibana:6.8.4
[root@centos7 docker]# docker container ls
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                                            NAMES
8d79d06dbfbc        kibana:6.8.4          "/usr/local/bin/kiba…"   16 minutes ago      Up 16 minutes       0.0.0.0:5601->5601/tcp                           upbeat_dubinsky
```

## ActiveMQ
[非官方镜像](https://hub.docker.com/r/rmohr/activemq)

!> 注意是非官方镜像，官方未提供docker安装包

```
[root@centos7 ~]# docker run -d -p 61616:61616 -p 8161:8161 rmohr/activemq
d113209c5bd927f8a50b4faa86743f76263a8ff6f0fe1ceb9ef3288e1e93fbcf
[root@centos7 ~]# docker container ls
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                                                                                   NAMES
d113209c5bd9        rmohr/activemq        "/bin/sh -c 'bin/act…"   8 seconds ago       Up 7 seconds        1883/tcp, 5672/tcp, 0.0.0.0:8161->8161/tcp, 61613-61614/tcp, 0.0.0.0:61616->61616/tcp   nifty_dewdney
```

