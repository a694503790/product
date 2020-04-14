!> ansible的默认读取的host文件在/etc/ansible/hosts or ~/.qiumin/ansible/hosts

```
 qiumin@QiuMin-MacBook-Pro  ~  cat ~/.qiumin/ansible/hosts
[session]
192.168.1.105
192.168.1.106
192.168.1.107
```
## 在本机操作
```
 ✘ qiumin@QiuMin-MacBook-Pro  ~  cat ~/.qiumin/ansible/hosts
[session]
local ansible_connection=local
```
## 调整端口
```
 ✘ qiumin@QiuMin-MacBook-Pro  ~  cat ~/.qiumin/ansible/hosts
[session]
192.168.1.105:33
192.168.1.106
192.168.1.107
```

## 设置主机别名
```host文件
 ✘ qiumin@QiuMin-MacBook-Pro  ~  cat ~/.qiumin/ansible/hosts
[session]
lip ansible_ssh_port=5555 ansible_ssh_host=192.168.1.105
192.168.1.106
192.168.1.107
```

```执行结果
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m ping
lip | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```
## 设置用户名
```
 ✘ qiumin@QiuMin-MacBook-Pro  ~  cat ~/.qiumin/ansible/hosts
[session]
lip ansible_ssh_port=22 ansible_ssh_host=192.168.1.105 ansible_ssh_user=qiumin
192.168.1.106
192.168.1.107
```
## ip连续简写
```
 qiumin@QiuMin-MacBook-Pro  ~  cat ~/.qiumin/ansible/hosts
[session]
192.168.1.10[5:7]
```

## 通过host文件设置变量

```host文件
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads  cat ~/.qiumin/ansible/hosts
[session]
lip ansible_ssh_host=192.168.1.127
comp1 ansible_ssh_host=192.168.1.105
comp2 ansible_ssh_host=192.168.1.106
comp3 ansible_ssh_host=192.168.1.107

[session:vars]
redis_log="/data"
```

```yml文件
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  cat a.yaml
- hosts: session
  tasks:
    - debug:
        msg: "My location is {{ redis_log }} "
```

```执行结果
✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [session] ***************************************************************************

TASK [Gathering Facts] *******************************************************************
ok: [comp2]
ok: [comp3]
ok: [lip]
ok: [comp1]

TASK [debug] *****************************************************************************
ok: [lip] => {
    "msg": "My location is /data "
}
ok: [comp1] => {
    "msg": "My location is /data "
}
ok: [comp2] => {
    "msg": "My location is /data "
}
ok: [comp3] => {
    "msg": "My location is /data "
}

PLAY RECAP *******************************************************************************
comp1                      : ok=2    changed=0    unreachable=0    failed=0
comp2                      : ok=2    changed=0    unreachable=0    failed=0
comp3                      : ok=2    changed=0    unreachable=0    failed=0
lip                        : ok=2    changed=0    unreachable=0    failed=0

```

## ansible_ssh_host
      将要连接的远程主机名.与你想要设定的主机的别名不同的话,可通过此变量设置.

## ansible_ssh_port
      ssh端口号.如果不是默认的端口号,通过此变量设置.

## ansible_ssh_user
      默认的 ssh 用户名

## ansible_ssh_pass
      ssh 密码(这种方式并不安全,我们强烈建议使用 --ask-pass 或 SSH 密钥)

## ansible_sudo_pass
      sudo 密码(这种方式并不安全,我们强烈建议使用 --ask-sudo-pass)

## ansible_sudo_exe 
	  (new in version 1.8)
      sudo 命令路径(适用于1.8及以上版本)

## ansible_connection
      与主机的连接类型.比如:local, ssh 或者 paramiko. Ansible 1.2 以前默认使用 paramiko.1.2 以后默认使用 'smart','smart' 方式会根据是否支持 ControlPersist, 来判断'ssh' 方式是否可行.

## ansible_ssh_private_key_file
      ssh 使用的私钥文件.适用于有多个密钥,而你不想使用 SSH 代理的情况.

## ansible_shell_type
      目标系统的shell类型.默认情况下,命令的执行使用 'sh' 语法,可设置为 'csh' 或 'fish'.

## ansible_python_interpreter
      目标主机的 python 路径.适用于的情况: 系统中有多个 Python, 或者命令路径不是"/usr/bin/python",比如  \*BSD, 或者 /usr/bin/python
      不是 2.X 版本的 Python.我们不使用 "/usr/bin/env" 机制,因为这要求远程用户的路径设置正确,且要求 "python" 可执行程序名不可为 python以外的名字(实际有可能名为python26).

      与 ansible_python_interpreter 的工作方式相同,可设定如 ruby 或 perl 的路径....

