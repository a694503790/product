?> `playbook`由一个或者多个`play`组成，每个`play`必须有`hosts`和`tasks`,`name`可以省略，`var`可以不定义

## name
	即play名称
## hosts
	即需要执行的host组，可使用单独某台机器的别名
## remote_user
	即定义用于远程的用户，也可以仅在某个task中指定
```
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  remote_user: qiumin
  tasks:
    - name: "测试remote_user"
      command: id
      register: output
    - 
      debug: var=output
```
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [测试play] ****************************************************************************************

TASK [测试remote_user] *********************************************************************************
changed: [lip]

TASK [debug] *****************************************************************************************
ok: [lip] => {
    "output": {
        "changed": true,
        "cmd": [
            "id"
        ],
        "delta": "0:00:00.093959",
        "end": "2020-03-30 09:14:14.572240",
        "failed": false,
        "rc": 0,
        "start": "2020-03-30 09:14:14.478281",
        "stderr": "",
        "stderr_lines": [],
        "stdout": "uid=1001(qiumin) gid=1001(qiumin) groups=1001(qiumin)",
        "stdout_lines": [
            "uid=1001(qiumin) gid=1001(qiumin) groups=1001(qiumin)"
        ]
    }
}

PLAY RECAP *******************************************************************************************
lip                        : ok=2    changed=1    unreachable=0    failed=0
```

## sudo
	即确认是否需要先sudo后再执行，也可以仅在某个task中指定
```代码
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  remote_user: qiumin
  sudo: yes
  tasks:
    - name: "测试sudo"
      command: id
      register: output
    - 
      debug: var=output
```
```结果
PLAY [测试play] ****************************************************************************************

TASK [测试sudo] ****************************************************************************************
changed: [lip]

TASK [debug] *****************************************************************************************
ok: [lip] => {
    "output": {
        "changed": true,
        "cmd": [
            "id"
        ],
        "delta": "0:00:00.112487",
        "end": "2020-03-30 09:21:02.615402",
        "failed": false,
        "rc": 0,
        "start": "2020-03-30 09:21:02.502915",
        "stderr": "",
        "stderr_lines": [],
        "stdout": "uid=0(root) gid=0(root) groups=0(root)",
        "stdout_lines": [
            "uid=0(root) gid=0(root) groups=0(root)"
        ]
    }
}

PLAY RECAP *******************************************************************************************
lip                        : ok=2    changed=1    unreachable=0    failed=0
```
## su_user
	如确认需要sudo，确认sudo后的用户,比如登陆到root后切换成elk
```
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  remote_user: root
  su: true
  su_user: qiumin
  tasks:
    - name: "测试sudo_user"
      command: id
      register: output
    - 
      debug: var=output
```
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml
[DEPRECATION WARNING]: Instead of su/su_user, use become/become_user and set become_method to 'su'
(default is sudo). This feature will be removed in version 2.6. Deprecation warnings can be disabled
by setting deprecation_warnings=False in ansible.cfg.

PLAY [测试play] ****************************************************************************************

TASK [测试sudo_user] ***********************************************************************************
changed: [lip]

TASK [debug] *****************************************************************************************
ok: [lip] => {
    "output": {
        "changed": true,
        "cmd": [
            "id"
        ],
        "delta": "0:00:00.108443",
        "end": "2020-03-30 16:00:47.404187",
        "failed": false,
        "rc": 0,
        "start": "2020-03-30 16:00:47.295744",
        "stderr": "",
        "stderr_lines": [],
        "stdout": "uid=1001(qiumin) gid=1001(qiumin) groups=1001(qiumin)",
        "stdout_lines": [
            "uid=1001(qiumin) gid=1001(qiumin) groups=1001(qiumin)"
        ]
    }
}

PLAY RECAP *******************************************************************************************
lip                        : ok=2    changed=1    unreachable=0    failed=0
```
## vars
	即定义变量
```
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  vars:
      redis_log: "qiumin"
  tasks:
    - name: "测试sudo_user"
      debug: msg="输出redis_log:{{ redis_log }}" 
```
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [测试play] ****************************************************************************************

TASK [测试sudo_user] ***********************************************************************************
ok: [lip] => {
    "msg": "输出redis_log:qiumin"
}

PLAY RECAP *******************************************************************************************
lip                        : ok=1    changed=0    unreachable=0    failed=0
```
## handlers
	触发器，假如上一步执行后是changed=1状态，则触发handlers，第二次触发则没有效果
!> 注意触发器不是按照在task中触发的顺序执行，而是在task执行完成后依次按照handlers中的触发
```
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  vars:
      redis_log: "qiumin"
  tasks:
    - name: "测试handlers"
      template: src=/Users/qiumin/Downloads/a.yaml dest=/root/minnnn
      notify:
        - test handlers
  handlers:
    - name: test handlers
      debug: msg="成功触发handlers"
```
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [测试play] ****************************************************************************************

TASK [测试handlers] ************************************************************************************
changed: [lip]

RUNNING HANDLER [test handlers] **********************************************************************
ok: [lip] => {
    "msg": "成功触发handlers"
}

PLAY RECAP *******************************************************************************************
lip                        : ok=2    changed=1    unreachable=0    failed=0

 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [测试play] ****************************************************************************************

TASK [测试handlers] ************************************************************************************
ok: [lip]

PLAY RECAP *******************************************************************************************
lip                        : ok=1    changed=0    unreachable=0    failed=0
```
## ignore_errors
	即是否忽略错误，执行下一步，True或者False,也可在task中指定
```
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  ignore_errors: true
  tasks:
    - name: "关闭selinux"
      command: /sbin/setenforce 0
```
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [测试play] ****************************************************************************************

TASK [关闭selinux] *************************************************************************************
fatal: [lip]: FAILED! => {"changed": true, "cmd": ["/sbin/setenforce", "0"], "delta": "0:00:00.109399", "end": "2020-03-30 12:13:36.316263", "msg": "non-zero return code", "rc": 1, "start": "2020-03-30 12:13:36.206864", "stderr": "/sbin/setenforce: SELinux is disabled", "stderr_lines": ["/sbin/setenforce: SELinux is disabled"], "stdout": "", "stdout_lines": []}
...ignoring

PLAY RECAP *******************************************************************************************
lip                        : ok=1    changed=1    unreachable=0    failed=0

```
## tasks
	即任务，每一个play下都有无数的tasks
### name
	即任务名称
### command
	即command模块
```yml代码
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  tasks:
    - name: "关闭selinux"
      command: /sbin/setenforce 0
```
```执行结果
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [测试play] ****************************************************************************************

TASK [关闭selinux] *************************************************************************************
fatal: [lip]: FAILED! => {"changed": true, "cmd": ["/sbin/setenforce", "0"], "delta": "0:00:00.083810", "end": "2020-03-30 08:50:23.092366", "msg": "non-zero return code", "rc": 1, "start": "2020-03-30 08:50:23.008556", "stderr": "/sbin/setenforce: SELinux is disabled", "stderr_lines": ["/sbin/setenforce: SELinux is disabled"], "stdout": "", "stdout_lines": []}
	to retry, use: --limit @/Users/qiumin/Downloads/a.retry

PLAY RECAP *******************************************************************************************
lip                        : ok=0    changed=0    unreachable=0    failed=1
```

### shell
	即shell模块
```
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  vars:
      redis_log: "qiumin"
  tasks:
    - name: "测试tasks"
      shell: "echo $LANG"
      register: output
    - name: "打印输出"
      debug: msg={{ output }}

```
```
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [测试play] ****************************************************************************************

TASK [测试tasks] ***************************************************************************************
changed: [lip]

TASK [打印输出] ******************************************************************************************
ok: [lip] => {
    "msg": {
        "changed": true,
        "cmd": "echo $LANG",
        "delta": "0:00:00.107680",
        "end": "2020-03-30 11:53:28.842424",
        "failed": false,
        "rc": 0,
        "start": "2020-03-30 11:53:28.734744",
        "stderr": "",
        "stderr_lines": [],
        "stdout": "en_US.UTF-8",
        "stdout_lines": [
            "en_US.UTF-8"
        ]
    }
}

PLAY RECAP *******************************************************************************************
lip                        : ok=2    changed=1    unreachable=0    failed=0
```	
### service
	即service模块	
```
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  tasks:
    - name: "测试tasks"
      service: 
         name: firewalld
         state: started
```
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [测试play] ****************************************************************************************

TASK [测试tasks] ***************************************************************************************
ok: [lip]

PLAY RECAP *******************************************************************************************
lip                        : ok=1    changed=0    unreachable=0    failed=0
```
### copy
```
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  tasks:
    - name: "测试tasks"
      copy: 
         src: /Users/qiumin/Downloads/a.yaml
         dest: /root/b.yaml
```
### template
	template和copy的不同之处在于，如果copy一个yaml，template可包含变量
```
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  tasks:
    - name: "测试tasks"
      template: 
         src: /Users/qiumin/Downloads/a.yaml
         dest: /root/c.yaml
```
```
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [测试play] ****************************************************************************************

TASK [测试tasks] ***************************************************************************************
changed: [lip]

PLAY RECAP *******************************************************************************************
lip                        : ok=1    changed=1    unreachable=0    failed=0
```
### meta
  主要是在roles中使用，这里不阐述
### notify
	结合Handlers使用，最佳的应用场景是用来重启服务,查看hadlers具体使用方法
### create
	这个是tag，但未发现文档说明和解析
### register
	用于装command或者shell返回的结果，查看【示例：打印命令返回值】看具体使用方法
### delegate_to 关键字
  任务委派，即假设有一个play中有3个task，中间的task需要在指定机器执行
```yml
- hosts: test70,test71
  gather_facts: no
  tasks:
  - file:
      path: "/tmp/ttt"
      state: touch
 
  - file:
      path: "/tmp/delegate"
      state: touch
    delegate_to: test61
 
  - file:
      path: "/tmp/ttt1"
      state: touch
```

### connection关键字
    当很多任务时，有部分任务需要在本机执行的使用方法
```yaml
- hosts: test70,test61
  gather_facts: no
  tasks:
  - file:
      path: "/tmp/inAnsible"
      state: touch
    connection: local
 
  - file:
      path: "/tmp/test"
      state: touch
```
### run_once 关键字
    用于部分task只需要运行一次的场景
```yaml
- hosts: A,B,C,D,E
  gather_facts: no
  tasks:
  - get_url:
      url: "http://nexus.zsythink.net/repository/testraw/testfile/test.tar"
      dest: /tmp/
    connection: local
    run_once: true
 
  - copy:
      src: "/tmp/test.tar"
      dest: "/tmp"

```
### wait_for
	等待一个端口监听或者等待文件创建才开始下一个动作
```yaml
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  ignore_errors: true
  tasks:
    - name: "关闭selinux"
      command: /sbin/setenforce 0
    - name: "检查端口是否监听"
      wait_for:
       host: 0.0.0.0
       delay: 2
       port: 80
       state: started
    - name: "执行"
      command: /sbin/setenforce 0
```
```
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [测试play] ****************************************************************************************

TASK [关闭selinux] *************************************************************************************
fatal: [lip]: FAILED! => {"changed": true, "cmd": ["/sbin/setenforce", "0"], "delta": "0:00:00.099897", "end": "2020-03-30 14:49:27.122632", "msg": "non-zero return code", "rc": 1, "start": "2020-03-30 14:49:27.022735", "stderr": "/sbin/setenforce: SELinux is disabled", "stderr_lines": ["/sbin/setenforce: SELinux is disabled"], "stdout": "", "stdout_lines": []}
...ignoring

TASK [检查端口是否监听] **************************************************************************************
ok: [lip]

TASK [执行] ********************************************************************************************
fatal: [lip]: FAILED! => {"changed": true, "cmd": ["/sbin/setenforce", "0"], "delta": "0:00:00.103046", "end": "2020-03-30 14:49:50.963744", "msg": "non-zero return code", "rc": 1, "start": "2020-03-30 14:49:50.860698", "stderr": "/sbin/setenforce: SELinux is disabled", "stderr_lines": ["/sbin/setenforce: SELinux is disabled"], "stdout": "", "stdout_lines": []}
...ignoring

PLAY RECAP *******************************************************************************************
lip                        : ok=3    changed=2    unreachable=0    failed=0
```

### vars_prompt
```yml
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  ignore_errors: true
  vars_prompt:
    - name: "First"
      prompt: "please input your username"
      private: no
    - name: "Second"
      prompt: "please input your passwd"
      default: 'good'  # 默认显示一个值
      private: yes  #置为yes的话，那么就是看不见自己输入的什么了
  tasks:
    - name: "打印用户名"
      debug: msg="您输入的用户名为：{{ First }}"
    - name: "打印密码"
      debug: msg="您输入的密码为：{{ Second }}"
```
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml
please input your username: qiu
please input your passwd [good]:

PLAY [测试play] ****************************************************************************************

TASK [打印用户名] *****************************************************************************************
ok: [lip] => {
    "msg": "您输入的用户名为：qiu"
}

TASK [打印密码] ******************************************************************************************
ok: [lip] => {
    "msg": "您输入的密码为：min"
}

PLAY RECAP *******************************************************************************************
lip                        : ok=2    changed=0    unreachable=0    failed=0
```

### authorized_key
```host文件
[root@lip1 ~]# cat  /etc/ansible/hosts
[session]
lip ansible_ssh_host=192.168.1.127 ansible_ssh_pass=qiumin520
local ansible_connection=local
```
```yaml文件
[root@lip1 ~]# cat tian.yaml
-
  name: "测试play"
  hosts: local
  gather_facts: false
  ignore_errors: true
  tasks:
    - name: "设置root用户的sshkeys"
      user:
        name: root
        generate_ssh_key: yes
        ssh_key_bits: 2048
        ssh_key_file: .ssh/id_rsa
-
  name: "测试play2"
  hosts: lip
  gather_facts: false
  tasks:
    - name: "将root用户的公钥传递到本机"
      authorized_key:
        user: root
        state: present
        key: "{{ lookup('file', '/root/.ssh/id_rsa.pub') }}"
```
```执行结果
[root@lip1 ~]# ansible-playbook tian.yaml

PLAY [测试play] ****************************************************************************************

TASK [设置root用户的sshkeys] ******************************************************************************
ok: [local]

PLAY [测试play2] ***************************************************************************************

TASK [将root用户的公钥传递到本机] *******************************************************************************
changed: [lip]

PLAY RECAP *******************************************************************************************
lip                        : ok=1    changed=1    unreachable=0    failed=0
local                      : ok=1    changed=0    unreachable=0    failed=0
```
```查看是否已经添加
[root@lip1 ~]# cat ~/.ssh/authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDLtoGq+GoK2maLUkxp9ns6Sk7GqUDpN8Tb2jrFx2RyqPwBNs9/lgGX52SzM6IUZ4TmWjMxTWOcGVxtWTeOU7pkY3eHXhPvn8KFvc4POFL1J6XepIH5S6deovs4G+w+1tUfWuIzcs+lHjqO2BKT/tB3HH7PZiBJ1A7+JXqkm3SFa+HLHaisuePauI5UVluJZP01JsQ6zYf7fvgBWyJ2pBMZZGGjlXIxAR+ySoRiu7hUK2Te1yVVRSlHHSE5AvmEH2ESW9DJO43CzDCKe59xacqM/tUgj72h5rbIP7f67M2uDEB56M5oX+NAkqBO8+36rJBt/LINfi/HB7kus41rd/Ph ansible-generated on lip1
```


## 示例：打印变量

```
- 
  name: “第一个play”
  hosts: lip
  gather_facts: false
  vars: 
     redis_log: "qiumin"
  tasks:
     - 
        debug: msg="输出redis_log变量的值：{{ redis_log }}"
     - 
        debug: var=redis_log
```

```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [“第一个play”] *************************************************************************************

TASK [debug] *****************************************************************************************
ok: [lip] => {
    "msg": "输出redis_log变量的值：qiumin"
}

TASK [debug] *****************************************************************************************
ok: [lip] => {
    "redis_log": "qiumin"
}

PLAY RECAP *******************************************************************************************
lip                        : ok=2    changed=0    unreachable=0    failed=0

```

## 示例：打印命令返回值
```yaml
- 
  name: "打印命令返回"
  hosts: lip
  gather_facts: false
  tasks:
    - 
      command: "w"
      register: ouput
    - 
      debug: var=ouput
```

```rezult
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [打印命令返回] ****************************************************************************************

TASK [command] ***************************************************************************************
changed: [lip]

TASK [debug] *****************************************************************************************
ok: [lip] => {
    "ouput": {
        "changed": true,
        "cmd": [
            "w"
        ],
        "delta": "0:00:00.122231",
        "end": "2020-03-26 15:03:48.545926",
        "failed": false,
        "rc": 0,
        "start": "2020-03-26 15:03:48.423695",
        "stderr": "",
        "stderr_lines": [],
        "stdout": " 15:03:48 up 13 days,  1:39,  2 users,  load average: 0.00, 0.01, 0.05\nUSER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT\nroot     pts/0    192.168.23.54    07:37   28.00s  0.31s  0.31s -bash\nroot     pts/1    192.168.23.54    15:03    0.00s  0.37s  0.11s w",
        "stdout_lines": [
            " 15:03:48 up 13 days,  1:39,  2 users,  load average: 0.00, 0.01, 0.05",
            "USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT",
            "root     pts/0    192.168.23.54    07:37   28.00s  0.31s  0.31s -bash",
            "root     pts/1    192.168.23.54    15:03    0.00s  0.37s  0.11s w"
        ]
    }
}

PLAY RECAP *******************************************************************************************
lip                        : ok=2    changed=1    unreachable=0    failed=0
```