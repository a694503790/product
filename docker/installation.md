### 先决条件
!> Docker cannot run correctly if your kernel is older than version 3.10 or if it is missing some modules

&nbsp; &nbsp; &nbsp; &nbsp;To install Docker Engine - Community, you need a maintained version of CentOS 7. Archived versions aren’t supported or tested.

&nbsp; &nbsp; &nbsp; &nbsp;The centos-extras repository must be enabled. This repository is enabled by default, but if you have disabled it, you need to re-enable it.

&nbsp; &nbsp; &nbsp; &nbsp;The overlay2 storage driver is recommended.

### yum 安装
!> yum-utils provides the yum-config-manager utility

```
$ sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
$ sudo yum-config-manager \
  --add-repo \
  https://download.docker.com/linux/centos/docker-ce.repo
$ sudo yum install docker-ce docker-ce-cli containerd.io
$ sudo systemctl start docker
$ sudo docker run hello-world
```

### rpm 安装
?> Go to https://download.docker.com/linux/centos/7/x86_64/stable/Packages/ and download the .rpm file
```
$ curl -O https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.6-3.3.el7.x86_64.rpm   
$ curl -O https://download.docker.com/linux/centos/7/x86_64/stable/Packages/docker-ce-19.03.4-3.el7.x86_64.rpm  
$ curl -O https://download.docker.com/linux/centos/7/x86_64/stable/Packages/docker-ce-cli-19.03.4-3.el7.x86_64.rpm 
$ sudo yum install /path/to/package.rpm
$ sudo systemctl start docker
$ sudo docker run hello-world
```
### scripts 安装
!> Using these scripts is not recommended for production environments
```
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh
```
### 镜像加速
```
mkdir -p /etc/docker
tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://zzftire8.mirror.aliyuncs.com"]
}
EOF
systemctl daemon-reload
systemctl restart docker
```
## 卸载
!> Older versions of Docker were called docker or docker-engine
```
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

!> The Docker Engine - Community package is now called docker-ce

```
$ sudo yum remove docker-ce
Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:

$ sudo rm -rf /var/lib/docker
You must delete any edited configuration files manually.
```