## Ansible 学习升天

常见的模板参数

vars 

creates

present

absent

loop

block

when

notify

handlers

import_playbook

tags: 任务标签， 执行指定任务的标签名 play级别标签名（为所有play级别task 打标签）动态加载，静态加载有区别

- 静态加载的指令有：roles、include、import_tasks、import_role
- 动态加载的指令只有include_xxx，包括include_tasks、include_role


动态加载的指令只有include_xxx，包括include_tasks、include_role



QA:

centos7 上碰见的问题

- selinux 默认开启



### Role 实践升天



## Ansible 任务驱动

1. 执行远程命令

   -   `ansible prod  -m ansible.builtin.shell -a  "ufsmount /export/ufs -o internal"`
   -   `ansible prod -m ansible.buildin.shell -a "ls /uStor/release/ufs/ufs-4.0.10/ |grep mgr |xargs echo `fds$1`"`

   另外的shell参数

2. 排除主机

   -   `ansible 'prod:!*111'  -m ansible.builtin.shell -a  "tar -C /uStor  /export/ufs/Department/release/4.0.10/UFS-Centos7.6-x86_64-4.0.10_2021-07-12_163743.upgrade.tar.gz"`

3. 安装rpm包

   -   task

4. 重启web

   -   `ansible  prod  -m ansible.buildin.service -a "name=ufs-dashboard state=restarted"  `
   -   `ansible  prod  -m ansible.buildin.service -a "name=ufs-agent state=restarted"  `

5. 带入参数 `-e var=var` 使用参数`{{ var }}` 

   ```shell
   - name: Update UFS
     hosts: prod
     remote_user: root
     tasks:
       - name: Install ufs
         ansible.builtin.shell:
         args:
           chdir: /uStor/release/ufs/ufs-{{ version }}/
           #cmd: ls |grep -v mgr | xargs rpm -iUv
           cmd: pwd
   ```

6. Ansible 传输文件/ 文件夹

   ```yaml
     tasks:
       - name: Transer rpm
         ansible.builtin.copy:
           src: /root/4.0.11/release/ufs/ufs-4.0.11
           dest: /uStor/release/ufs
   ```

   



- update 

  ```yml
  - name: Update UFS
    hosts: prod
    remote_user: root
    tasks:
      - name: Transer rpm
        ansible.builtin.copy:
          src: /root/4.0.11/release/ufs/ufs-4.0.11
          dest: /uStor/release/ufs
      - name: Install ufs
        ansible.builtin.shell:
        args:
          chdir: /uStor/release/ufs/ufs-{{ version }}
          cmd: ls |grep mgr | xargs rpm -iUh
      - name: Restart Agent Service
        ansible.builtin.service:
          name: ufs-agent
          state: restarted
      - name: Restart Dashboard Service
        ansible.builtin.service:
          name: ufs-dashboard
          state: restarted
  ```

  

