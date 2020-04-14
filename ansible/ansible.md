## --ask-su-pass
	ask for su password (deprecated, use become)

## --ask-sudo-pass
      ask for sudo password (deprecated, use become)

## --ask-vault-pass
      ask for vault password

## --become-method 'BECOME_METHOD'
      privilege  escalation  method  to  use (default=sudo), valid choices: [
      sudo | su | pbrun | pfexec | doas | dzdo | ksu | runas | pmrun | enable
      | machinectl ]

## --become-user 'BECOME_USER'
      run operations as this user (default=root)

## --list-hosts
      outputs a list of matching hosts; does not execute anything else

## --playbook-dir 'BASEDIR'
      Since  this  tool does not use playbooks, use this as a subsitute play-
      book directory.This sets the relative path for many features  including
      roles/ group_vars/ etc.

## --private-key, --key-file
      use this file to authenticate the connection

## --scp-extra-args 'SCP_EXTRA_ARGS'
      specify extra arguments to pass to scp only (e.g. -l)

## --sftp-extra-args 'SFTP_EXTRA_ARGS'
      specify extra arguments to pass to sftp only (e.g. -f, -l)

## --ssh-common-args 'SSH_COMMON_ARGS'
      specify common arguments to pass to sftp/scp/ssh (e.g. ProxyCommand)

## --ssh-extra-args 'SSH_EXTRA_ARGS'
      specify extra arguments to pass to ssh only (e.g. -R)

## --syntax-check
      perform a syntax check on the playbook, but do not execute it

## --vault-id
      the vault identity to use

## --vault-password-file
      vault password file

## --version
      show program's version number and exit

##   -B 后台运行多少秒，结合-P
     'SECONDS', --background 'SECONDS', run asynchronously, failing after X seconds (default=N/A)

##   -C, --check
      don't  make  any  changes;  instead, try to predict some of the changes
      that may occur

##   -D, --diff
      when changing (small) files and  templates,  show  the  differences  in
      those files; works great with --check

##   -K, --ask-become-pass
      ask for privilege escalation password

##   -M, --module-path
      prepend      colon-separated      path(s)     to     module     library
      (default=[u'/home/jenkins/.ansible/plugins/modules', u'/usr/share/ansi-
      ble/plugins/modules'])

##   -P 每隔多少秒获取信息，结合-B
      'POLL_INTERVAL', --poll 'POLL_INTERVAL'，set the poll interval if using -B (default=15)

##   -R 'SU_USER', --su-user 'SU_USER'

      run  operations  with  su  as this user (default=None) (deprecated, use
      become)

##   -S, --su
      run operations with su (deprecated, use become)

##   -T 'TIMEOUT', --timeout 'TIMEOUT'
      override the connection timeout in seconds (default=10)

##   -U 'SUDO_USER', --sudo-user 'SUDO_USER'
      desired sudo user (default=root) (deprecated, use become)

##   -a 'MODULE_ARGS', --args 'MODULE_ARGS'
      module arguments

##   -b, --become
      run operations with become (does not imply password prompting)

##   -c 'CONNECTION', --connection 'CONNECTION'
      connection type to use (default=smart)
##   -e, --extra-vars 定义变量
      set additional variables as key=value or YAML/JSON, if filename prepend
      with @

##   -f 'FORKS', --forks 'FORKS' 子进程
      specify number of parallel processes to use (default=5)

##   -h, --help
      show this help message and exit

##   -i, --inventory, --inventory-file
      specify  inventory  host  path  or  comma separated host list. --inven-
      tory-file is deprecated

##  -k, --ask-pass 询问密码
      ask for connection password

##  -l 'SUBSET', --limit 'SUBSET'
      further limit selected hosts to an additional pattern

##   -m 'MODULE_NAME', --module-name 'MODULE_NAME'
      module name to execute (default=command)
      condense output

##   -s, --sudo
      run operations with sudo (nopasswd) (deprecated, use become)

##   -t 'TREE', --tree 'TREE'
      log output to this directory

##   -u 'REMOTE_USER', --user 'REMOTE_USER'
      connect as this user (default=None)

##   -v, --verbose
      verbose mode (-vvv for more, -vvvv to enable connection debugging)