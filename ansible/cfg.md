!>  ansible的配置文件在/etc/ansible/ansible.cfg or ~/.ansible.cfg

?> ansible配置文件默认采用公钥认证，可以关闭

## defaults段
```
 ✘ qiumin@QiuMin-MacBook-Pro  ~/knowledge/ansible   master ●  cat ~/.ansible.cfg
[defaults]
host_key_checking = False
remote_user = root
inventory = /Users/qiumin/.qiumin/ansible/hosts
```

### ask_pass
ask_pass即playbook 是否会自动默认弹出弹出密码.默认为no，一般无需打开
```
ask_pass=True
```

### ask_sudo_pass
类似ask_pass,用来控制Ansible playbook 在执行sudo之前是否询问sudo密码.默认为no
```
ask_sudo_pass=True
```

### _forks
设置与主机通信时的默认并行进程数
```
_forks=5
```

### gather_facts
控制默认facts收集（远程系统变量）,默认收集
```
gather_facts: False
```

### host_key_checking
检测主机密钥
```
host_key_checking=True
```

### inventory
默认主机文件
```
inventory = /etc/ansible/hosts
```

### log_path
默认日志目录，默认不开启
```
log_path=/var/log/ansible.log
```

### module_name
默认模块，默认为command,建议使用shell作为默认
```
module_name = command
```

### hosts
playbook要通信的默认主机组
```
hosts=*
```
!> /usr/bin/ansible 一直需要一个host pattern,并且不使用这个选项.这个选项只作用于/usr/bin/ansible-playbook

### remote_port
默认的远程SSH端口，默认为22
```
remote_port = 22
```

### remote_tmp
默认的执行路径，默认路径是在用户家目录下属的目录.Ansible 会在这个目录中使用一个随机的文件夹名称
```
remote_tmp = $HOME/.ansible/tmp
```

### remote_user
默认的远程用户，如果不指定，默认使用当前用户
```
remote_user = root
```

### timeout
默认SSH链接尝试超时时间
```
timeout = 10
```

## paramiko段
### record_host_keys
默认设置会记录并验证通过在用户hostfile中新发现的的主机（如果host key checking 被激活的话）. 这个选项在有很多主机的时候将会性能很差.在 这种情况下,建议使用SSH传输代替. 当设置为False时, 性能将会提升.
```
record_host_keys=True
```

## ssh_connection段

```
 ✘ qiumin@QiuMin-MacBook-Pro  ~/knowledge/ansible   master ●  cat ~/.ansible.cfg
[defaults]
host_key_checking = False
remote_user = root
inventory = /Users/qiumin/.qiumin/ansible/hosts
[ssh_connection]

```

### ssh_args
用来调整SSH的通信连接
```
ssh_args = -o ControlMaster=auto -o ControlPersist=60s
```

### pipelining
在不通过实际文件传输的情况下执行ansible模块来使用管道特性,从而减少执行远程模块SSH操作次数.如果开启这个设置,将显著提高性能. 然而当使用”sudo:”操作的时候, 你必须在所有管理的主机的/etc/sudoers中禁用’requiretty’.默认关闭
```
pipelining=False
```














