## include tasks 语句
```shell
# 源文件
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

```yaml
# 拆分后的主文件a.yaml
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  remote_user: root
  su: true
  su_user: qiumin
  tasks:
    - include: b.yaml
    - 
      debug: var=output
```

```yaml
# 拆分后的附加文件b.yaml
- name: "测试sudo_user"
  command: id
  register: output

```

```shell
# 执行结果
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
        "delta": "0:00:00.108341",
        "end": "2020-03-31 11:10:53.289308",
        "failed": false,
        "rc": 0,
        "start": "2020-03-31 11:10:53.180967",
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
## include tasks 附加变量

```yaml
# 源文件a.yaml
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  remote_user: root
  su: true
  su_user: qiumin
  tasks:
    - include: b.yaml yourname=qiumin
    - 
      debug: var=output

```

```yaml
# 源文件b.yaml
- name: "测试sudo_user"
  command: id
  register: output
- name: "打印传递后的变量"
  debug: msg="传递进来的变量结果为：{{ yourname }}"
```

```shell
# 执行结果
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml
[DEPRECATION WARNING]: Instead of su/su_user, use become/become_user and set become_method to 'su'
(default is sudo). This feature will be removed in version 2.6. Deprecation warnings can be disabled
by setting deprecation_warnings=False in ansible.cfg.

PLAY [测试play] ****************************************************************************************

TASK [测试sudo_user] ***********************************************************************************
changed: [lip]

TASK [打印传递后的变量] **************************************************************************************
ok: [lip] => {
    "msg": "传递进来的变量结果为：qiumin"
}

TASK [debug] *****************************************************************************************
ok: [lip] => {
    "output": {
        "changed": true,
        "cmd": [
            "id"
        ],
        "delta": "0:00:00.105138",
        "end": "2020-03-31 11:17:10.813778",
        "failed": false,
        "rc": 0,
        "start": "2020-03-31 11:17:10.708640",
        "stderr": "",
        "stderr_lines": [],
        "stdout": "uid=1001(qiumin) gid=1001(qiumin) groups=1001(qiumin)",
        "stdout_lines": [
            "uid=1001(qiumin) gid=1001(qiumin) groups=1001(qiumin)"
        ]
    }
}

PLAY RECAP *******************************************************************************************
lip                        : ok=3    changed=1    unreachable=0    failed=0
```

## import_playbook 语法

```yaml
# 主playbook[a.yml]
- 
  name: "测试主play"
  hosts: lip
  gather_facts: false
  remote_user: root
  tasks:
    - include: b.yaml
    - 
      debug: var=output
- import_playbook: c.yaml
```
```yaml
# task引用的yaml[b.yaml]
- name: "测试task的import"
  command: id
  register: output
```
```yaml
# 主yaml引入的附yaml[c.yaml]
- 
  name: "测试import_playbook"
  hosts: lip
  gather_facts: false
  remote_user: root
  vars:
    yourname: qiumin
  tasks:
     - name: "打印变量"
       debug: msg="传递进来的变量结果为：{{ yourname }}"

```

```shell
# 执行结果
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [测试主play] ***************************************************************************************

TASK [测试task的import] *********************************************************************************
changed: [lip]

TASK [debug] *****************************************************************************************
ok: [lip] => {
    "output": {
        "changed": true,
        "cmd": [
            "id"
        ],
        "delta": "0:00:00.106674",
        "end": "2020-03-31 12:43:47.510809",
        "failed": false,
        "rc": 0,
        "start": "2020-03-31 12:43:47.404135",
        "stderr": "",
        "stderr_lines": [],
        "stdout": "uid=0(root) gid=0(root) groups=0(root)",
        "stdout_lines": [
            "uid=0(root) gid=0(root) groups=0(root)"
        ]
    }
}

PLAY [测试import_playbook] *****************************************************************************

TASK [打印变量] ******************************************************************************************
ok: [lip] => {
    "msg": "传递进来的变量结果为：qiumin"
}

PLAY RECAP *******************************************************************************************
lip                        : ok=3    changed=1    unreachable=0    failed=0
```
## 结构化playbook
```shell
# 目录
.
├── main.retry
├── main.yml
└── roles
    └── ceshi
        ├── README.md
        ├── defaults
        │   └── main.yml
        ├── files
        ├── handlers
        │   └── main.yml
        ├── meta
        │   └── main.yml
        ├── tasks
        │   ├── b.yml
        │   └── main.yml
        ├── templates
        ├── tests
        │   ├── inventory
        │   └── test.yml
        └── vars
            └── main.yml
```

```yaml
# 主main.yml
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads/test_roles  cat ~/Downloads/test_roles/main.yml
-
 hosts: lip
 gather_facts: false
 remote_user: root
 roles:
    - ceshi
```

```yaml
# ceshi的main.yml
 qiumin@QiuMin-MacBook-Pro  ~/Downloads/test_roles  cat roles/ceshi/tasks/main.yml
    - include: b.yml
    - name: "打印输出"
      debug: var=output
```
```yaml
# ceshi的b.yml
 qiumin@QiuMin-MacBook-Pro  ~/Downloads/test_roles  cat roles/ceshi/tasks/b.yml
- name: "测试task的import"
  command: id
  register: output
```
```yaml
# 执行结果
 qiumin@QiuMin-MacBook-Pro  ~/Downloads/test_roles  ansible-playbook main.yml

PLAY [lip] *******************************************************************************************

TASK [ceshi : 测试task的import] *************************************************************************
changed: [lip]

TASK [ceshi : 打印输出] **********************************************************************************
ok: [lip] => {
    "output": {
        "changed": true,
        "cmd": [
            "id"
        ],
        "delta": "0:00:00.110835",
        "end": "2020-03-31 13:19:43.892485",
        "failed": false,
        "rc": 0,
        "start": "2020-03-31 13:19:43.781650",
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
### defaults
	默认变量目录，优先级最低，理论上不会用，不做解释
### files
	角色可能会用到的文件，比如nginx的证书
```yml
 qiumin@QiuMin-MacBook-Pro  ~/Downloads/test_roles/roles/test  cat tasks/main.yml
    - name: "测试file"
      copy:
         src: file.sh
         dest: /root/file.sh
```
### handlers
	可以被notify触发，按顺序执行
```yml
#task文件 
- name: "测试handlers"
  template: src=/Users/qiumin/Downloads/a.yaml dest=/root/minnnn
  notify:
    - test handlers
```
```yml
# handle文件
- name: test handlers
      debug: msg="成功触发handlers"
```
### meta
	元数据，另外可以添加依赖，添加dependencies后可自动触发其它roles中的task
```yaml
dependencies: 
  - { role: test, test_value: 3 }
```
### tasks
	即任务
```yaml
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads/test_roles  cat roles/ceshi/tasks/main.yml
    - include: b.yml
    - name: "打印输出"
      debug: var=output
```
### templates
	模版相关的文件，里面带变量替换用
```yml
 qiumin@QiuMin-MacBook-Pro  ~/Downloads/test_roles/roles/test  cat tasks/main.yml
    - name: "测试template"
      template:
         src: a.sh
         dest: /root/abbbb.sh
```
### vars
	定义变量的目录
```yml
 qiumin@QiuMin-MacBook-Pro  ~/Downloads/test_roles/roles/test  cat vars/main.yml
---
# vars file for test
yourname: qiumin
```
##  更丰富的结构化
```yml
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads/test_roles  cat main.yml
-
 hosts: lip
 gather_facts: false
 remote_user: root
 pre_tasks:
    - name: "第一个task"
      shell: echo 'hello'
    - name: "第二个task"
      shell: echo 'hello1'
 roles:
    - ceshi
 tasks:
    - name: "第三个task"
      shell: echo 'hello2'
 post_tasks:
    - name: "第四个task"
      shell: echo 'hello3'
```


