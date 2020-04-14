## ping 模块
```
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible all -m ping
192.168.1.105 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
192.168.1.107 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
192.168.1.106 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

## get_url
```yml
  get_url:
    url: "{{ item.url }}"
    dest: "{{ item.dest }}"
    timeout: 100
  with_items:
    - { url: "http://uni.mirrors.163.com/mysql/Downloads/MySQL-5.7/mysql-community-client-5.7.29-1.el7.x86_64.rpm",dest: "/tmp" }
    - { url: "http://uni.mirrors.163.com/mysql/Downloads/MySQL-5.7/mysql-community-common-5.7.29-1.el7.x86_64.rpm",dest: "/tmp" }
    - { url: "http://uni.mirrors.163.com/mysql/Downloads/MySQL-5.7/mysql-community-devel-5.7.29-1.el7.x86_64.rpm",dest: "/tmp" }
    - { url: "http://uni.mirrors.163.com/mysql/Downloads/MySQL-5.7/mysql-community-libs-5.7.29-1.el7.x86_64.rpm",dest: "/tmp" }
    - { url: "http://uni.mirrors.163.com/mysql/Downloads/MySQL-5.7/mysql-community-libs-compat-5.7.29-1.el7.x86_64.rpm",dest: "/tmp" }
    - { url: "http://uni.mirrors.163.com/mysql/Downloads/MySQL-5.7/mysql-community-server-5.7.29-1.el7.x86_64.rpm",dest: "/tmp" }

```

## command模块

```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible all -m command -a 'echo `hostname`'
192.168.1.106 | SUCCESS | rc=0 >>
`hostname`

192.168.1.107 | SUCCESS | rc=0 >>
`hostname`

192.168.1.105 | SUCCESS | rc=0 >>
`hostname`
```

!> command模块和shell模块的差异见上下文，shell模块会传递变量或者管道到服务器运行，command模块不会，而ansible的默认模块为command模块

## shell模块
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible all -m shell -a 'echo `hostname`'
192.168.1.105 | SUCCESS | rc=0 >>
do1cloud00

192.168.1.106 | SUCCESS | rc=0 >>
do1cloud01

192.168.1.107 | SUCCESS | rc=0 >>
do1cloud02

```

## copy模块
```
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m copy -a 'src=/Users/qiumin/Downloads/a.yaml dest=/root'
lip | SUCCESS => {
    "changed": true,
    "checksum": "52c9503bc9e3b8f5333ddd79e6b28930192a5450",
    "dest": "/root/a.yaml",
    "gid": 0,
    "group": "root",
    "md5sum": "67c1f4dedb8821df825877d139ae5b55",
    "mode": "0644",
    "owner": "root",
    "size": 86,
    "src": "/root/.ansible/tmp/ansible-tmp-1585120813.75-68485718758798/source",
    "state": "file",
    "uid": 0
}
```
## fetch模块
```
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m fetch -a 'src=/root/anaconda-ks.cfg dest=/Users/qiumin/Downloads'
lip | SUCCESS => {
    "changed": true,
    "checksum": "625ea4372b3bcf85f3568481c8b01c0d4fc5a35c",
    "dest": "/Users/qiumin/Downloads/lip/root/anaconda-ks.cfg",
    "md5sum": "b5759680e7e8e051c75cee85e0e73e50",
    "remote_checksum": "625ea4372b3bcf85f3568481c8b01c0d4fc5a35c",
    "remote_md5sum": null
}
```
## file模块
### 创建目录
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m file -a 'dest=/root/qiumin mode=755 state=directory'
lip | SUCCESS => {
    "changed": true,
    "gid": 0,
    "group": "root",
    "mode": "0755",
    "owner": "root",
    "path": "/root/qiumin",
    "size": 6,
    "state": "directory",
    "uid": 0
}
```
### 创建文件
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m file -a 'path=/root/qiufile111 mode=755 state=touch'
lip | SUCCESS => {
    "changed": true,
    "dest": "/root/qiufile111",
    "gid": 0,
    "group": "root",
    "mode": "0755",
    "owner": "root",
    "size": 0,
    "state": "file",
    "uid": 0
}
```

### 修改文件权限
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m file -a 'path=/root/qiufile111 mode=600'
lip | SUCCESS => {
    "changed": true,
    "gid": 0,
    "group": "root",
    "mode": "0600",
    "owner": "root",
    "path": "/root/qiufile111",
    "size": 0,
    "state": "file",
    "uid": 0
}
```
### 删除文件或目录
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m file -a 'path=/root/qiufile111 state=absent'
lip | SUCCESS => {
    "changed": true,
    "path": "/root/qiufile111",
    "state": "absent"
}
```
## yum模块
!> 另外还有`disable_gpg_check`和`enablerepo`的常用参数可选
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m yum -a 'name=telnet state=latest'
lip | SUCCESS => {
    "changed": true,
    "msg": "",
    "rc": 0,
    "results": [
        "Loaded plugins: fastestmirror\nLoading mirror speeds from cached hostfile\n * base: mirrors.163.com\n * extras: mirrors.163.com\n * updates: mirrors.163.com\nResolving Dependencies\n--> Running transaction check\n---> Package telnet.x86_64 1:0.17-64.el7 will be installed\n--> Finished Dependency Resolution\n\nDependencies Resolved\n\n================================================================================\n Package          Arch             Version                 Repository      Size\n================================================================================\nInstalling:\n telnet           x86_64           1:0.17-64.el7           base            64 k\n\nTransaction Summary\n================================================================================\nInstall  1 Package\n\nTotal download size: 64 k\nInstalled size: 113 k\nDownloading packages:\nRunning transaction check\nRunning transaction test\nTransaction test succeeded\nRunning transaction\n  Installing : 1:telnet-0.17-64.el7.x86_64                                  1/1 \n  Verifying  : 1:telnet-0.17-64.el7.x86_64                                  1/1 \n\nInstalled:\n  telnet.x86_64 1:0.17-64.el7                                                   \n\nComplete!\n"
    ]
}
```

## user模块
```添加用户
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m user -a 'name=qiu'
lip | SUCCESS => {
    "changed": true,
    "comment": "",
    "create_home": true,
    "group": 1000,
    "home": "/home/qiu",
    "name": "qiu",
    "shell": "/bin/bash",
    "state": "present",
    "system": false,
    "uid": 1000
}
```

```删除用户
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m user -a 'name=qiu state=absent'
lip | SUCCESS => {
    "changed": true,
    "force": false,
    "name": "qiu",
    "remove": false,
    "state": "absent"
}
```
## service模块
```
qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m service -a 'name=rsyslog state=started'
lip | SUCCESS => {
    "changed": true,
    "name": "rsyslog",
    "state": "started",
    "status": {
        "ActiveEnterTimestamp": "Fri 2020-03-13 13:24:17 CST",
        "ActiveEnterTimestampMonotonic": "16282710",
        "ActiveExitTimestamp": "Wed 2020-03-25 15:48:19 CST",
        "ActiveExitTimestampMonotonic": "1045458324290",
        "ActiveState": "inactive",
        "After": "network-online.target basic.target network.target system.slice",
        "AllowIsolate": "no",
        "AmbientCapabilities": "0",
        "AssertResult": "yes",
        "AssertTimestamp": "Fri 2020-03-13 13:24:10 CST",
        "AssertTimestampMonotonic": "9479947",
        "Before": "shutdown.target multi-user.target",
        "BlockIOAccounting": "no",
        "BlockIOWeight": "18446744073709551615",
        "CPUAccounting": "no",
        "CPUQuotaPerSecUSec": "infinity",
        "CPUSchedulingPolicy": "0",
        "CPUSchedulingPriority": "0",
        "CPUSchedulingResetOnFork": "no",
        "CPUShares": "18446744073709551615",
        "CanIsolate": "no",
        "CanReload": "no",
        "CanStart": "yes",
        "CanStop": "yes",
        "CapabilityBoundingSet": "18446744073709551615",
        "ConditionResult": "yes",
        "ConditionTimestamp": "Fri 2020-03-13 13:24:10 CST",
        "ConditionTimestampMonotonic": "9479946",
        "Conflicts": "shutdown.target",
        "ControlPID": "0",
        "DefaultDependencies": "yes",
        "Delegate": "no",
        "Description": "System Logging Service",
        "DevicePolicy": "auto",
        "Documentation": "man:rsyslogd(8) http://www.rsyslog.com/doc/",
        "EnvironmentFile": "/etc/sysconfig/rsyslog (ignore_errors=yes)",
        "ExecMainCode": "1",
        "ExecMainExitTimestamp": "Wed 2020-03-25 15:48:19 CST",
        "ExecMainExitTimestampMonotonic": "1045458331466",
        "ExecMainPID": "858",
        "ExecMainStartTimestamp": "Fri 2020-03-13 13:24:10 CST",
        "ExecMainStartTimestampMonotonic": "9490815",
        "ExecMainStatus": "0",
        "ExecStart": "{ path=/usr/sbin/rsyslogd ; argv[]=/usr/sbin/rsyslogd -n $SYSLOGD_OPTIONS ; ignore_errors=no ; start_time=[Fri 2020-03-13 13:24:10 CST] ; stop_time=[Wed 2020-03-25 15:48:19 CST] ; pid=858 ; code=exited ; status=0 }",
        "FailureAction": "none",
        "FileDescriptorStoreMax": "0",
        "FragmentPath": "/usr/lib/systemd/system/rsyslog.service",
        "GuessMainPID": "yes",
        "IOScheduling": "0",
        "Id": "rsyslog.service",
        "IgnoreOnIsolate": "no",
        "IgnoreOnSnapshot": "no",
        "IgnoreSIGPIPE": "yes",
        "InactiveEnterTimestamp": "Wed 2020-03-25 15:48:19 CST",
        "InactiveEnterTimestampMonotonic": "1045458331893",
        "InactiveExitTimestamp": "Fri 2020-03-13 13:24:10 CST",
        "InactiveExitTimestampMonotonic": "9490900",
        "JobTimeoutAction": "none",
        "JobTimeoutUSec": "0",
        "KillMode": "control-group",
        "KillSignal": "15",
        "LimitAS": "18446744073709551615",
        "LimitCORE": "18446744073709551615",
        "LimitCPU": "18446744073709551615",
        "LimitDATA": "18446744073709551615",
        "LimitFSIZE": "18446744073709551615",
        "LimitLOCKS": "18446744073709551615",
        "LimitMEMLOCK": "65536",
        "LimitMSGQUEUE": "819200",
        "LimitNICE": "0",
        "LimitNOFILE": "4096",
        "LimitNPROC": "63449",
        "LimitRSS": "18446744073709551615",
        "LimitRTPRIO": "0",
        "LimitRTTIME": "18446744073709551615",
        "LimitSIGPENDING": "63449",
        "LimitSTACK": "18446744073709551615",
        "LoadState": "loaded",
        "MainPID": "0",
        "MemoryAccounting": "no",
        "MemoryCurrent": "18446744073709551615",
        "MemoryLimit": "18446744073709551615",
        "MountFlags": "0",
        "Names": "rsyslog.service",
        "NeedDaemonReload": "no",
        "Nice": "0",
        "NoNewPrivileges": "no",
        "NonBlocking": "no",
        "NotifyAccess": "main",
        "OOMScoreAdjust": "0",
        "OnFailureJobMode": "replace",
        "PermissionsStartOnly": "no",
        "PrivateDevices": "no",
        "PrivateNetwork": "no",
        "PrivateTmp": "no",
        "ProtectHome": "no",
        "ProtectSystem": "no",
        "RefuseManualStart": "no",
        "RefuseManualStop": "no",
        "RemainAfterExit": "no",
        "Requires": "basic.target",
        "Restart": "on-failure",
        "RestartUSec": "100ms",
        "Result": "success",
        "RootDirectoryStartOnly": "no",
        "RuntimeDirectoryMode": "0755",
        "SameProcessGroup": "no",
        "SecureBits": "0",
        "SendSIGHUP": "no",
        "SendSIGKILL": "yes",
        "Slice": "system.slice",
        "StandardError": "inherit",
        "StandardInput": "null",
        "StandardOutput": "null",
        "StartLimitAction": "none",
        "StartLimitBurst": "5",
        "StartLimitInterval": "10000000",
        "StartupBlockIOWeight": "18446744073709551615",
        "StartupCPUShares": "18446744073709551615",
        "StatusErrno": "0",
        "StopWhenUnneeded": "no",
        "SubState": "dead",
        "SyslogLevelPrefix": "yes",
        "SyslogPriority": "30",
        "SystemCallErrorNumber": "0",
        "TTYReset": "no",
        "TTYVHangup": "no",
        "TTYVTDisallocate": "no",
        "TasksAccounting": "no",
        "TasksCurrent": "18446744073709551615",
        "TasksMax": "18446744073709551615",
        "TimeoutStartUSec": "1min 30s",
        "TimeoutStopUSec": "1min 30s",
        "TimerSlackNSec": "50000",
        "Transient": "no",
        "Type": "notify",
        "UMask": "0066",
        "UnitFilePreset": "enabled",
        "UnitFileState": "enabled",
        "WantedBy": "multi-user.target",
        "Wants": "network-online.target network.target system.slice",
        "WatchdogTimestampMonotonic": "0",
        "WatchdogUSec": "0"
    }
}
```

## cron模块
```脚本
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m cron -a 'name=test minute=0 hour=1 job=ls'
lip | SUCCESS => {
    "changed": true,
    "envs": [],
    "jobs": [
        "test"
    ]
}
```
```结果
[root@lip1 .ssh]# crontab -l
#Ansible: test
0 1 * * * ls
```
## synchronize模块
```前提是先在目标端安装rsync
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m yum -a 'name=rsync'
lip | SUCCESS => {
    "changed": true,
    "msg": "",
    "rc": 0,
```
```syschronize模块用法
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m synchronize -a 'src=/Users/qiumin/Downloads/  dest=/root/'
lip | SUCCESS => {
    "changed": true,
    "cmd": "/usr/bin/rsync --delay-updates -F --compress --archive --rsh=/usr/local/bin/ssh -S none -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null --out-format=<<CHANGED>>%i %n%L /Users/qiumin/Downloads/ root@192.168.1.127:/root/",
```

## script模块
```sh脚本
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  cat a.sh
echo "dsfsfsfsfsfsdfsf"
```
```script执行结果
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m script -a 'a.sh'
lip | SUCCESS => {
    "changed": true,
    "rc": 0,
    "stderr": "Shared connection to 192.168.1.127 closed.\r\n",
    "stderr_lines": [
        "Shared connection to 192.168.1.127 closed."
    ],
    "stdout": "dsfsfsfsfsfsdfsf\r\n",
    "stdout_lines": [
        "dsfsfsfsfsfsdfsf"
    ]
}
```
