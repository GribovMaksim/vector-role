# Домашнее задание к занятию 5 «Тестирование roles»

## Подготовка к выполнению

1. Установите molecule: `pip3 install "molecule==3.5.2"`.
2. Выполните `docker pull aragast/netology:latest` —  это образ с podman, tox и несколькими пайтонами (3.7 и 3.9) внутри.

## Основная часть

Ваша цель — настроить тестирование ваших ролей. 

Задача — сделать сценарии тестирования для vector. 

Ожидаемый результат — все сценарии успешно проходят тестирование ролей.

### Molecule

1. Запустите  `molecule test -s centos_7` внутри корневой директории clickhouse-role, посмотрите на вывод команды. Данная команда может отработать с ошибками, это нормально. Наша цель - посмотреть как другие в реальном мире используют молекулу.
![image](https://github.com/GribovMaksim/vector-role/assets/112322500/c36a5e98-aff5-4570-9ff1-0f3d059a1129)
2. Перейдите в каталог с ролью vector-role и создайте сценарий тестирования по умолчанию при помощи `molecule init scenario --driver-name docker`.
3. Добавьте несколько разных дистрибутивов (centos:8, ubuntu:latest) для инстансов и протестируйте роль, исправьте найденные ошибки, если они есть.
```bash
vagrant@vagrant:~/molecule/vector-role$ molecule test
WARNING  No schema found in docker driver.
INFO     default scenario test matrix: dependency, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
INFO     Performing prerun with role_name_check=0...
INFO     Set ANSIBLE_LIBRARY=/home/vagrant/.cache/ansible-compat/f5bcd7/modules:/home/vagrant/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
INFO     Set ANSIBLE_COLLECTIONS_PATH=/home/vagrant/.cache/ansible-compat/f5bcd7/collections:/home/vagrant/.ansible/collections:/usr/share/ansible/collections
INFO     Set ANSIBLE_ROLES_PATH=/home/vagrant/.cache/ansible-compat/f5bcd7/roles:/home/vagrant/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles
INFO     Using /home/vagrant/.cache/ansible-compat/f5bcd7/roles/zzz.vector_role symlink to current repository in order to enable Ansible to find the role using its expected full name.
INFO     Running default > dependency
WARNING  Skipping, missing the requirements file.
WARNING  Skipping, missing the requirements file.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy
INFO     Sanity checks: 'docker'

PLAY [Destroy] *****************************************************************

TASK [Set async_dir for HOME env] **********************************************
ok: [localhost]

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=centos8)
changed: [localhost] => (item=ubuntu)

TASK [Wait for instance(s) deletion to complete] *******************************
ok: [localhost] => (item=centos8)
ok: [localhost] => (item=ubuntu)

TASK [Delete docker networks(s)] ***********************************************
skipping: [localhost]

PLAY RECAP *********************************************************************
localhost                  : ok=3    changed=1    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Running default > syntax

playbook: /home/vagrant/molecule/vector-role/molecule/default/converge.yml
INFO     Running default > create

PLAY [Create] ******************************************************************

TASK [Set async_dir for HOME env] **********************************************
ok: [localhost]

TASK [Log into a Docker registry] **********************************************
skipping: [localhost] => (item=None)
skipping: [localhost] => (item=None)
skipping: [localhost]

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item={'image': 'pycontribs/centos:8', 'name': 'centos8', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item={'image': 'pycontribs/centos:8', 'name': 'centos8', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})
skipping: [localhost]

TASK [Synchronization the context] *********************************************
skipping: [localhost] => (item={'image': 'pycontribs/centos:8', 'name': 'centos8', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})
skipping: [localhost]

TASK [Discover local Docker images] ********************************************
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'image': 'pycontribs/centos:8', 'name': 'centos8', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 1, 'ansible_index_var': 'i'})

TASK [Build an Ansible compatible image (new)] *********************************
skipping: [localhost] => (item=molecule_local/pycontribs/centos:8)
skipping: [localhost] => (item=molecule_local/pycontribs/ubuntu:latest)
skipping: [localhost]

TASK [Create docker network(s)] ************************************************
skipping: [localhost]

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item={'image': 'pycontribs/centos:8', 'name': 'centos8', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=centos8)
changed: [localhost] => (item=ubuntu)

TASK [Wait for instance(s) creation to complete] *******************************
FAILED - RETRYING: [localhost]: Wait for instance(s) creation to complete (300 retries left).
changed: [localhost] => (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': '317667358678.170929', 'results_file': '/home/vagrant/.ansible_async/317667358678.170929', 'changed': True, 'item': {'image': 'pycontribs/centos:8', 'name': 'centos8', 'pre_build_image': True}, 'ansible_loop_var': 'item'})
changed: [localhost] => (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': '463884739739.170955', 'results_file': '/home/vagrant/.ansible_async/463884739739.170955', 'changed': True, 'item': {'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=6    changed=2    unreachable=0    failed=0    skipped=5    rescued=0    ignored=0

INFO     Running default > prepare
WARNING  Skipping, prepare playbook not configured.
INFO     Running default > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [centos8]
ok: [ubuntu]

TASK [Include vector-role] *****************************************************

TASK [vector-role : include_tasks] *********************************************
included: /home/vagrant/molecule/vector-role/tasks/download_install_vector.yml for centos8, ubuntu

TASK [vector-role : fixing repo for CentOS 8] **********************************
skipping: [ubuntu]
changed: [centos8]

TASK [vector-role : Get vector distrib (deb)] **********************************
skipping: [centos8]
changed: [ubuntu]

TASK [vector-role : Get vector distrib (rpm)] **********************************
skipping: [ubuntu]
changed: [centos8]

TASK [vector-role : Install vector packages (deb)] *****************************
skipping: [centos8]
changed: [ubuntu]

TASK [vector-role : Install vector packages (rpm)] *****************************
skipping: [ubuntu]
changed: [centos8]

RUNNING HANDLER [vector-role : Start Vector service] ***************************
skipping: [centos8]
skipping: [ubuntu]

PLAY RECAP *********************************************************************
centos8                    : ok=5    changed=3    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0
ubuntu                     : ok=4    changed=2    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0

INFO     Running default > idempotence

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [centos8]
ok: [ubuntu]

TASK [Include vector-role] *****************************************************

TASK [vector-role : include_tasks] *********************************************
included: /home/vagrant/molecule/vector-role/tasks/download_install_vector.yml for centos8, ubuntu

TASK [vector-role : fixing repo for CentOS 8] **********************************
skipping: [ubuntu]
ok: [centos8]

TASK [vector-role : Get vector distrib (deb)] **********************************
skipping: [centos8]
ok: [ubuntu]

TASK [vector-role : Get vector distrib (rpm)] **********************************
skipping: [ubuntu]
ok: [centos8]

TASK [vector-role : Install vector packages (deb)] *****************************
skipping: [centos8]
ok: [ubuntu]

TASK [vector-role : Install vector packages (rpm)] *****************************
skipping: [ubuntu]
ok: [centos8]

PLAY RECAP *********************************************************************
centos8                    : ok=5    changed=0    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0
ubuntu                     : ok=4    changed=0    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0

INFO     Idempotence completed successfully.
INFO     Running default > side_effect
WARNING  Skipping, side effect playbook not configured.
INFO     Running default > verify
INFO     Running Ansible Verifier

PLAY [Verify] ******************************************************************

TASK [Example assertion] *******************************************************
ok: [centos8] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [ubuntu] => {
    "changed": false,
    "msg": "All assertions passed"
}

PLAY RECAP *********************************************************************
centos8                    : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Verifier completed successfully.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy

PLAY [Destroy] *****************************************************************

TASK [Set async_dir for HOME env] **********************************************
ok: [localhost]

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=centos8)
changed: [localhost] => (item=ubuntu)

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: [localhost]: Wait for instance(s) deletion to complete (300 retries left).
changed: [localhost] => (item=centos8)
changed: [localhost] => (item=ubuntu)

TASK [Delete docker networks(s)] ***********************************************
skipping: [localhost]

PLAY RECAP *********************************************************************
localhost                  : ok=3    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Pruning extra files from scenario ephemeral directory
```
4. Добавьте несколько assert в verify.yml-файл для  проверки работоспособности vector-role (проверка, что конфиг валидный, проверка успешности запуска и др.). 
5. Запустите тестирование роли повторно и проверьте, что оно прошло успешно.
```bash
WARNING  No schema found in docker driver.
INFO     default scenario test matrix: dependency, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
INFO     Performing prerun with role_name_check=0...
INFO     Set ANSIBLE_LIBRARY=/home/vagrant/.cache/ansible-compat/f5bcd7/modules:/home/vagrant/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
INFO     Set ANSIBLE_COLLECTIONS_PATH=/home/vagrant/.cache/ansible-compat/f5bcd7/collections:/home/vagrant/.ansible/collections:/usr/share/ansible/collections
INFO     Set ANSIBLE_ROLES_PATH=/home/vagrant/.cache/ansible-compat/f5bcd7/roles:/home/vagrant/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles
INFO     Using /home/vagrant/.cache/ansible-compat/f5bcd7/roles/zzz.vector_role symlink to current repository in order to enable Ansible to find the role using its expected full name.
INFO     Running default > dependency
WARNING  Skipping, missing the requirements file.
WARNING  Skipping, missing the requirements file.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy
INFO     Sanity checks: 'docker'

PLAY [Destroy] *****************************************************************

TASK [Set async_dir for HOME env] **********************************************
ok: [localhost]

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=centos8)
changed: [localhost] => (item=ubuntu)

TASK [Wait for instance(s) deletion to complete] *******************************
ok: [localhost] => (item=centos8)
ok: [localhost] => (item=ubuntu)

TASK [Delete docker networks(s)] ***********************************************
skipping: [localhost]

PLAY RECAP *********************************************************************
localhost                  : ok=3    changed=1    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Running default > syntax

playbook: /home/vagrant/molecule/vector-role/molecule/default/converge.yml
INFO     Running default > create

PLAY [Create] ******************************************************************

TASK [Set async_dir for HOME env] **********************************************
ok: [localhost]

TASK [Log into a Docker registry] **********************************************
skipping: [localhost] => (item=None)
skipping: [localhost] => (item=None)
skipping: [localhost]

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item={'image': 'pycontribs/centos:8', 'name': 'centos8', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item={'image': 'pycontribs/centos:8', 'name': 'centos8', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})
skipping: [localhost]

TASK [Synchronization the context] *********************************************
skipping: [localhost] => (item={'image': 'pycontribs/centos:8', 'name': 'centos8', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})
skipping: [localhost]

TASK [Discover local Docker images] ********************************************
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'image': 'pycontribs/centos:8', 'name': 'centos8', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 1, 'ansible_index_var': 'i'})

TASK [Build an Ansible compatible image (new)] *********************************
skipping: [localhost] => (item=molecule_local/pycontribs/centos:8)
skipping: [localhost] => (item=molecule_local/pycontribs/ubuntu:latest)
skipping: [localhost]

TASK [Create docker network(s)] ************************************************
skipping: [localhost]

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item={'image': 'pycontribs/centos:8', 'name': 'centos8', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=centos8)
changed: [localhost] => (item=ubuntu)

TASK [Wait for instance(s) creation to complete] *******************************
FAILED - RETRYING: [localhost]: Wait for instance(s) creation to complete (300 retries left).
changed: [localhost] => (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': '492426393182.185445', 'results_file': '/home/vagrant/.ansible_async/492426393182.185445', 'changed': True, 'item': {'image': 'pycontribs/centos:8', 'name': 'centos8', 'pre_build_image': True}, 'ansible_loop_var': 'item'})
changed: [localhost] => (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': '757808016701.185471', 'results_file': '/home/vagrant/.ansible_async/757808016701.185471', 'changed': True, 'item': {'image': 'pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=6    changed=2    unreachable=0    failed=0    skipped=5    rescued=0    ignored=0

INFO     Running default > prepare
WARNING  Skipping, prepare playbook not configured.
INFO     Running default > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
			 
ok: [ubuntu]
ok: [centos8]

TASK [Include vector-role] *****************************************************

TASK [vector-role : include_tasks] *********************************************
included: /home/vagrant/molecule/vector-role/tasks/download_install_vector.yml for centos8, ubuntu

TASK [vector-role : fixing repo for CentOS 8] **********************************
skipping: [ubuntu]
changed: [centos8]

TASK [vector-role : Get vector distrib (deb)] **********************************
skipping: [centos8]
changed: [ubuntu]

TASK [vector-role : Get vector distrib (rpm)] **********************************
skipping: [ubuntu]
changed: [centos8]

TASK [vector-role : Install vector packages (deb)] *****************************
skipping: [centos8]
changed: [ubuntu]

TASK [vector-role : Install vector packages (rpm)] *****************************
skipping: [ubuntu]
changed: [centos8]

RUNNING HANDLER [vector-role : Start Vector service] ***************************
skipping: [centos8]
skipping: [ubuntu]

PLAY RECAP *********************************************************************
centos8                    : ok=5    changed=3    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0
ubuntu                     : ok=4    changed=2    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0

INFO     Running default > idempotence

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
			 
ok: [ubuntu]
ok: [centos8]

TASK [Include vector-role] *****************************************************

TASK [vector-role : include_tasks] *********************************************
included: /home/vagrant/molecule/vector-role/tasks/download_install_vector.yml for centos8, ubuntu

TASK [vector-role : fixing repo for CentOS 8] **********************************
skipping: [ubuntu]
ok: [centos8]

TASK [vector-role : Get vector distrib (deb)] **********************************
skipping: [centos8]
ok: [ubuntu]

TASK [vector-role : Get vector distrib (rpm)] **********************************
skipping: [ubuntu]
ok: [centos8]

TASK [vector-role : Install vector packages (deb)] *****************************
skipping: [centos8]
ok: [ubuntu]

TASK [vector-role : Install vector packages (rpm)] *****************************
skipping: [ubuntu]
ok: [centos8]

PLAY RECAP *********************************************************************
centos8                    : ok=5    changed=0    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0
ubuntu                     : ok=4    changed=0    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0

INFO     Idempotence completed successfully.
INFO     Running default > side_effect
WARNING  Skipping, side effect playbook not configured.
INFO     Running default > verify
INFO     Running Ansible Verifier

PLAY [Verify] ******************************************************************

TASK [Gathering Facts] *********************************************************
ok: [ubuntu]
ok: [centos8]

TASK [verify vector config] ****************************************************
ok: [centos8]
ok: [ubuntu]

TASK [verify vector is started] ************************************************
skipping: [centos8]
skipping: [ubuntu]

PLAY RECAP *********************************************************************
centos8                    : ok=2    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
ubuntu                     : ok=2    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Verifier completed successfully.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy

PLAY [Destroy] *****************************************************************

TASK [Set async_dir for HOME env] **********************************************
ok: [localhost]

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=centos8)
changed: [localhost] => (item=ubuntu)

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: [localhost]: Wait for instance(s) deletion to complete (300 retries left).
changed: [localhost] => (item=centos8)
changed: [localhost] => (item=ubuntu)

TASK [Delete docker networks(s)] ***********************************************
skipping: [localhost]

PLAY RECAP *********************************************************************
localhost                  : ok=3    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Pruning extra files from scenario ephemeral directory
```
6. Добавьте новый тег на коммит с рабочим сценарием в соответствии с семантическим версионированием.

### Tox

1. Добавьте в директорию с vector-role файлы из [директории](./example).
2. Запустите `docker run --privileged=True -v <path_to_repo>:/opt/vector-role -w /opt/vector-role -it aragast/netology:latest /bin/bash`, где path_to_repo — путь до корня репозитория с vector-role на вашей файловой системе.
3. Внутри контейнера выполните команду `tox`, посмотрите на вывод.
```bash
[root@34f09bd4568b vector-role]# tox
py37-ansible210 create: /opt/vector-role/.tox/py37-ansible210
py37-ansible210 installdeps: -rtox-requirements.txt, ansible<3.0
ERROR: invocation failed (exit code 1), logfile: /opt/vector-role/.tox/py37-ansible210/log/py37-ansible210-1.log
====================================================== log start =======================================================
Collecting ansible<3.0
  Downloading ansible-2.10.7.tar.gz (29.9 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 29.9/29.9 MB 3.5 MB/s eta 0:00:00
  Preparing metadata (setup.py): started
  Preparing metadata (setup.py): still running...
  Preparing metadata (setup.py): finished with status 'done'
Collecting selinux
  Using cached selinux-0.2.1-py2.py3-none-any.whl (4.3 kB)
ERROR: Could not find a version that satisfies the requirement ansible-lint==6.14.2 (from versions: 1.0.0, 1.0.1, 1.0.2, 1.0.3, 1.0.4, 2.0.1, 2.0.3, 2.1.0, 2.1.3, 2.2.0, 2.3.0, 2.3.1, 2.3.2, 2.3.3, 2.3.5, 2.3.6, 2.3.8, 2.3.9, 2.4.0, 2.4.1, 2.4.2, 2.5.0, 2.6.1, 2.6.2, 2.7.0rc1, 2.7.0, 2.7.1, 3.0.0rc1, 3.0.0rc2, 3.0.0rc3, 3.0.0rc4, 3.0.0rc5, 3.0.0rc6, 3.0.0rc7, 3.0.0rc8, 3.0.0rc9, 3.0.0rc10, 3.0.0rc11, 3.0.0rc12, 3.0.0, 3.0.1, 3.0.2rc1, 3.1.0rc1, 3.1.0, 3.1.1, 3.1.2, 3.1.3, 3.2.0, 3.2.1rc1, 3.2.1rc2, 3.2.1, 3.2.2rc1, 3.2.2, 3.2.3, 3.2.4, 3.2.5, 3.3.0rc1, 3.3.0rc2, 3.3.0rc3, 3.3.0rc4, 3.3.0, 3.3.2, 3.3.3, 3.4.0rc1, 3.4.0rc2, 3.4.0, 3.4.1, 3.4.3, 3.4.4, 3.4.5, 3.4.6, 3.4.7, 3.4.8, 3.4.9, 3.4.10, 3.4.11, 3.4.12, 3.4.13, 3.4.15, 3.4.16, 3.4.17, 3.4.18, 3.4.19, 3.4.20, 3.4.21, 3.4.22, 3.4.23, 3.5.0rc1, 3.5.0, 3.5.1, 4.0.0a1, 4.0.0a2, 4.0.0, 4.0.1, 4.1.0a0, 4.1.0, 4.1.1a0, 4.1.1a1, 4.1.1a2, 4.1.1a3, 4.1.1a4, 4.1.1a5, 4.1.1a6, 4.2.0a1, 4.2.0rc1, 4.2.0rc2, 4.2.0, 4.3.0a0, 4.3.0a1, 4.3.0a2, 4.3.0a3, 4.3.0a4, 4.3.0a5, 4.3.0a6, 4.3.0, 4.3.1, 4.3.2, 4.3.3, 4.3.4, 4.3.5, 4.3.6, 4.3.7, 5.0.0a0, 5.0.0a1, 5.0.0a2.dev12, 5.0.0a3.dev23, 5.0.0a3.dev27, 5.0.0a3, 5.0.0, 5.0.1, 5.0.2, 5.0.3a0, 5.0.3a1, 5.0.3rc1, 5.0.3, 5.0.4, 5.0.5a0, 5.0.5a1, 5.0.5, 5.0.6, 5.0.7, 5.0.8, 5.0.9, 5.0.10, 5.0.11, 5.0.12, 5.1.0a0, 5.1.0a1, 5.1.1, 5.1.2, 5.1.3, 5.2.0, 5.2.1, 5.3.0, 5.3.1, 5.3.2, 5.4.0)
ERROR: No matching distribution found for ansible-lint==6.14.2
WARNING: You are using pip version 22.0.4; however, version 23.1.2 is available.
You should consider upgrading via the '/opt/vector-role/.tox/py37-ansible210/bin/python -m pip install --upgrade pip' command.

======================================================= log end ========================================================
ERROR: could not install deps [-rtox-requirements.txt, ansible<3.0]; v = InvocationError("/opt/vector-role/.tox/py37-ansible210/bin/python -m pip install -rtox-requirements.txt 'ansible<3.0'", 1)
py37-ansible30 create: /opt/vector-role/.tox/py37-ansible30
py37-ansible30 installdeps: -rtox-requirements.txt, ansible<3.1
ERROR: invocation failed (exit code 1), logfile: /opt/vector-role/.tox/py37-ansible30/log/py37-ansible30-1.log
====================================================== log start =======================================================
Collecting ansible<3.1
  Downloading ansible-3.0.0.tar.gz (30.8 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 30.8/30.8 MB 3.0 MB/s eta 0:00:00
  Preparing metadata (setup.py): started
  Preparing metadata (setup.py): still running...
  Preparing metadata (setup.py): finished with status 'done'
Collecting selinux
  Using cached selinux-0.2.1-py2.py3-none-any.whl (4.3 kB)
ERROR: Could not find a version that satisfies the requirement ansible-lint==6.14.2 (from versions: 1.0.0, 1.0.1, 1.0.2, 1.0.3, 1.0.4, 2.0.1, 2.0.3, 2.1.0, 2.1.3, 2.2.0, 2.3.0, 2.3.1, 2.3.2, 2.3.3, 2.3.5, 2.3.6, 2.3.8, 2.3.9, 2.4.0, 2.4.1, 2.4.2, 2.5.0, 2.6.1, 2.6.2, 2.7.0rc1, 2.7.0, 2.7.1, 3.0.0rc1, 3.0.0rc2, 3.0.0rc3, 3.0.0rc4, 3.0.0rc5, 3.0.0rc6, 3.0.0rc7, 3.0.0rc8, 3.0.0rc9, 3.0.0rc10, 3.0.0rc11, 3.0.0rc12, 3.0.0, 3.0.1, 3.0.2rc1, 3.1.0rc1, 3.1.0, 3.1.1, 3.1.2, 3.1.3, 3.2.0, 3.2.1rc1, 3.2.1rc2, 3.2.1, 3.2.2rc1, 3.2.2, 3.2.3, 3.2.4, 3.2.5, 3.3.0rc1, 3.3.0rc2, 3.3.0rc3, 3.3.0rc4, 3.3.0, 3.3.2, 3.3.3, 3.4.0rc1, 3.4.0rc2, 3.4.0, 3.4.1, 3.4.3, 3.4.4, 3.4.5, 3.4.6, 3.4.7, 3.4.8, 3.4.9, 3.4.10, 3.4.11, 3.4.12, 3.4.13, 3.4.15, 3.4.16, 3.4.17, 3.4.18, 3.4.19, 3.4.20, 3.4.21, 3.4.22, 3.4.23, 3.5.0rc1, 3.5.0, 3.5.1, 4.0.0a1, 4.0.0a2, 4.0.0, 4.0.1, 4.1.0a0, 4.1.0, 4.1.1a0, 4.1.1a1, 4.1.1a2, 4.1.1a3, 4.1.1a4, 4.1.1a5, 4.1.1a6, 4.2.0a1, 4.2.0rc1, 4.2.0rc2, 4.2.0, 4.3.0a0, 4.3.0a1, 4.3.0a2, 4.3.0a3, 4.3.0a4, 4.3.0a5, 4.3.0a6, 4.3.0, 4.3.1, 4.3.2, 4.3.3, 4.3.4, 4.3.5, 4.3.6, 4.3.7, 5.0.0a0, 5.0.0a1, 5.0.0a2.dev12, 5.0.0a3.dev23, 5.0.0a3.dev27, 5.0.0a3, 5.0.0, 5.0.1, 5.0.2, 5.0.3a0, 5.0.3a1, 5.0.3rc1, 5.0.3, 5.0.4, 5.0.5a0, 5.0.5a1, 5.0.5, 5.0.6, 5.0.7, 5.0.8, 5.0.9, 5.0.10, 5.0.11, 5.0.12, 5.1.0a0, 5.1.0a1, 5.1.1, 5.1.2, 5.1.3, 5.2.0, 5.2.1, 5.3.0, 5.3.1, 5.3.2, 5.4.0)
ERROR: No matching distribution found for ansible-lint==6.14.2
WARNING: You are using pip version 22.0.4; however, version 23.1.2 is available.
You should consider upgrading via the '/opt/vector-role/.tox/py37-ansible30/bin/python -m pip install --upgrade pip' command.

======================================================= log end ========================================================
ERROR: could not install deps [-rtox-requirements.txt, ansible<3.1]; v = InvocationError("/opt/vector-role/.tox/py37-ansible30/bin/python -m pip install -rtox-requirements.txt 'ansible<3.1'", 1)
py39-ansible210 create: /opt/vector-role/.tox/py39-ansible210
py39-ansible210 installdeps: -rtox-requirements.txt, ansible<3.0
py39-ansible210 installed: ansible==2.10.7,ansible-base==2.10.17,ansible-compat==4.0.2,ansible-core==2.14.5,ansible-lint==6.14.2,arrow==1.2.3,attrs==23.1.0,binaryornot==0.4.4,black==23.3.0,bracex==2.3.post1,certifi==2023.5.7,cffi==1.15.1,chardet==5.1.0,charset-normalizer==3.1.0,click==8.1.3,click-help-colors==0.9.1,cookiecutter==2.1.1,cryptography==40.0.2,distro==1.8.0,enrich==1.2.7,filelock==3.12.0,idna==3.4,Jinja2==3.1.2,jinja2-time==0.2.0,jmespath==1.0.1,jsonschema==4.17.3,lxml==4.9.2,markdown-it-py==2.2.0,MarkupSafe==2.1.2,mdurl==0.1.2,molecule==5.0.0,molecule-podman==2.0.3,mypy-extensions==1.0.0,packaging==23.1,pathspec==0.11.1,platformdirs==3.5.1,pluggy==1.0.0,pycparser==2.21,Pygments==2.15.1,pyrsistent==0.19.3,python-dateutil==2.8.2,python-slugify==8.0.1,PyYAML==6.0,requests==2.30.0,resolvelib==0.8.1,rich==13.3.5,ruamel.yaml==0.17.26,ruamel.yaml.clib==0.2.7,selinux==0.3.0,six==1.16.0,subprocess-tee==0.4.1,text-unidecode==1.3,tomli==2.0.1,typing_extensions==4.5.0,urllib3==2.0.2,wcmatch==8.4.1,yamllint==1.29.0
py39-ansible210 run-test-pre: PYTHONHASHSEED='3920108830'
py39-ansible210 run-test: commands[0] | molecule test -s compatibility --destroy always
CRITICAL 'molecule/compatibility/molecule.yml' glob failed.  Exiting.
ERROR: InvocationError for command /opt/vector-role/.tox/py39-ansible210/bin/molecule test -s compatibility --destroy always (exited with code 1)
py39-ansible30 create: /opt/vector-role/.tox/py39-ansible30
py39-ansible30 installdeps: -rtox-requirements.txt, ansible<3.1
py39-ansible30 installed: ansible==3.0.0,ansible-base==2.10.17,ansible-compat==4.0.2,ansible-core==2.14.5,ansible-lint==6.14.2,arrow==1.2.3,attrs==23.1.0,binaryornot==0.4.4,black==23.3.0,bracex==2.3.post1,certifi==2023.5.7,cffi==1.15.1,chardet==5.1.0,charset-normalizer==3.1.0,click==8.1.3,click-help-colors==0.9.1,cookiecutter==2.1.1,cryptography==40.0.2,distro==1.8.0,enrich==1.2.7,filelock==3.12.0,idna==3.4,Jinja2==3.1.2,jinja2-time==0.2.0,jmespath==1.0.1,jsonschema==4.17.3,lxml==4.9.2,markdown-it-py==2.2.0,MarkupSafe==2.1.2,mdurl==0.1.2,molecule==5.0.0,molecule-podman==2.0.3,mypy-extensions==1.0.0,packaging==23.1,pathspec==0.11.1,platformdirs==3.5.1,pluggy==1.0.0,pycparser==2.21,Pygments==2.15.1,pyrsistent==0.19.3,python-dateutil==2.8.2,python-slugify==8.0.1,PyYAML==6.0,requests==2.30.0,resolvelib==0.8.1,rich==13.3.5,ruamel.yaml==0.17.26,ruamel.yaml.clib==0.2.7,selinux==0.3.0,six==1.16.0,subprocess-tee==0.4.1,text-unidecode==1.3,tomli==2.0.1,typing_extensions==4.5.0,urllib3==2.0.2,wcmatch==8.4.1,yamllint==1.29.0
py39-ansible30 run-test-pre: PYTHONHASHSEED='3920108830'
py39-ansible30 run-test: commands[0] | molecule test -s compatibility --destroy always
CRITICAL 'molecule/compatibility/molecule.yml' glob failed.  Exiting.
ERROR: InvocationError for command /opt/vector-role/.tox/py39-ansible30/bin/molecule test -s compatibility --destroy always (exited with code 1)
_______________________________________________________ summary ________________________________________________________
ERROR:   py37-ansible210: could not install deps [-rtox-requirements.txt, ansible<3.0]; v = InvocationError("/opt/vector-role/.tox/py37-ansible210/bin/python -m pip install -rtox-requirements.txt 'ansible<3.0'", 1)
ERROR:   py37-ansible30: could not install deps [-rtox-requirements.txt, ansible<3.1]; v = InvocationError("/opt/vector-role/.tox/py37-ansible30/bin/python -m pip install -rtox-requirements.txt 'ansible<3.1'", 1)
ERROR:   py39-ansible210: commands failed
ERROR:   py39-ansible30: commands failed
```
4. Создайте облегчённый сценарий для `molecule` с драйвером `molecule_podman`. Проверьте его на исполнимость.
5. Пропишите правильную команду в `tox.ini`, чтобы запускался облегчённый сценарий.
6. Запустите команду `tox`. Убедитесь, что всё отработало успешно.
 ```bash
 [root@5b678abeae16 vector-role]# tox
py37-ansible210 installed: ansible==2.10.7,ansible-base==2.10.17,ansible-compat==1.0.0,ansible-lint==5.1.3,arrow==1.2.3,bcrypt==4.0.1,binaryornot==0.4.4,bracex==2.3.post1,cached-property==1.5.2,Cerberus==1.3.2,certifi==2023.5.7,cffi==1.15.1,chardet==5.1.0,charset-normalizer==3.1.0,click==8.1.3,click-help-colors==0.9.1,cookiecutter==2.1.1,cryptography==40.0.2,distro==1.8.0,enrich==1.2.7,idna==3.4,importlib-metadata==6.6.0,Jinja2==3.1.2,jinja2-time==0.2.0,jmespath==1.0.1,lxml==4.9.2,markdown-it-py==2.2.0,MarkupSafe==2.1.2,mdurl==0.1.2,molecule==3.5.2,molecule-podman==1.1.0,packaging==23.1,paramiko==2.12.0,pathspec==0.11.1,pluggy==1.0.0,pycparser==2.21,Pygments==2.15.1,PyNaCl==1.5.0,python-dateutil==2.8.2,python-slugify==8.0.1,PyYAML==5.4.1,requests==2.30.0,rich==13.3.5,ruamel.yaml==0.17.26,ruamel.yaml.clib==0.2.7,selinux==0.2.1,six==1.16.0,subprocess-tee==0.3.5,tenacity==8.2.2,text-unidecode==1.3,typing_extensions==4.5.0,urllib3==2.0.2,wcmatch==8.4.1,yamllint==1.26.3,zipp==3.15.0
py37-ansible210 run-test-pre: PYTHONHASHSEED='3069389728'
py37-ansible210 run-test: commands[0] | molecule test -s compatibility --destroy always
INFO     compatibility scenario test matrix: destroy, create, converge, destroy
INFO     Performing prerun...
INFO     Set ANSIBLE_LIBRARY=/root/.cache/ansible-compat/b984a4/modules:/root/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
INFO     Set ANSIBLE_COLLECTIONS_PATH=/root/.cache/ansible-compat/b984a4/collections:/root/.ansible/collections:/usr/share/ansible/collections
INFO     Set ANSIBLE_ROLES_PATH=/root/.cache/ansible-compat/b984a4/roles:/root/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles
INFO     Running compatibility > destroy
INFO     Sanity checks: 'podman'

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item={'image': 'pycontribs/centos:8', 'name': 'instances', 'pre_build_image': True})

TASK [Wait for instance(s) deletion to complete] *******************************
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '34908571322.7200', 'results_file': '/root/.ansible_async/34908571322.7200', 'changed': True, 'failed': False, 'item': {'image': 'pycontribs/centos:8', 'name': 'instances', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Running compatibility > create

PLAY [Create] ******************************************************************

TASK [get podman executable path] **********************************************
ok: [localhost]

TASK [save path to executable as fact] *****************************************
ok: [localhost]

TASK [Log into a container registry] *******************************************
skipping: [localhost] => (item="instances registry username: None specified")

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item=Dockerfile: None specified)

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item="Dockerfile: None specified; Image: pycontribs/centos:8")

TASK [Discover local Podman images] ********************************************
ok: [localhost] => (item=instances)

TASK [Build an Ansible compatible image] ***************************************
skipping: [localhost] => (item=pycontribs/centos:8)

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item="instances command: None specified")

TASK [Remove possible pre-existing containers] *********************************
changed: [localhost]

TASK [Discover local podman networks] ******************************************
skipping: [localhost] => (item=instances: None specified)

TASK [Create podman network dedicated to this scenario] ************************
skipping: [localhost]

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=instances)

TASK [Wait for instance(s) creation to complete] *******************************
changed: [localhost] => (item=instances)

PLAY RECAP *********************************************************************
localhost                  : ok=8    changed=3    unreachable=0    failed=0    skipped=5    rescued=0    ignored=0

INFO     Running compatibility > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [instances]

TASK [Copy something to test use of synchronize module] ************************
changed: [instances]

TASK [Include vector-role] *****************************************************

TASK [vector-role : include_tasks] *********************************************
included: /opt/vector-role/tasks/download_install_vector.yml for instances

TASK [vector-role : fixing repo for CentOS 8] **********************************
changed: [instances]

TASK [vector-role : Get vector distrib (deb)] **********************************
skipping: [instances]

TASK [vector-role : Get vector distrib (rpm)] **********************************
changed: [instances]

TASK [vector-role : Install vector packages (deb)] *****************************
skipping: [instances]

TASK [vector-role : Install vector packages (rpm)] *****************************
changed: [instances]

RUNNING HANDLER [vector-role : Start Vector service] ***************************
skipping: [instances]

PLAY RECAP *********************************************************************
instances                  : ok=6    changed=4    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0

INFO     Running compatibility > destroy

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item={'image': 'pycontribs/centos:8', 'name': 'instances', 'pre_build_image': True})

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
FAILED - RETRYING: Wait for instance(s) deletion to complete (299 retries left).
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '777155165708.9810', 'results_file': '/root/.ansible_async/777155165708.9810', 'changed': True, 'failed': False, 'item': {'image': 'pycontribs/centos:8', 'name': 'instances', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Pruning extra files from scenario ephemeral directory
py37-ansible30 installed: ansible==3.0.0,ansible-base==2.10.17,ansible-compat==1.0.0,ansible-lint==5.1.3,arrow==1.2.3,bcrypt==4.0.1,binaryornot==0.4.4,bracex==2.3.post1,cached-property==1.5.2,Cerberus==1.3.2,certifi==2023.5.7,cffi==1.15.1,chardet==5.1.0,charset-normalizer==3.1.0,click==8.1.3,click-help-colors==0.9.1,cookiecutter==2.1.1,cryptography==40.0.2,distro==1.8.0,enrich==1.2.7,idna==3.4,importlib-metadata==6.6.0,Jinja2==3.1.2,jinja2-time==0.2.0,jmespath==1.0.1,lxml==4.9.2,markdown-it-py==2.2.0,MarkupSafe==2.1.2,mdurl==0.1.2,molecule==3.5.2,molecule-podman==1.1.0,packaging==23.1,paramiko==2.12.0,pathspec==0.11.1,pluggy==1.0.0,pycparser==2.21,Pygments==2.15.1,PyNaCl==1.5.0,python-dateutil==2.8.2,python-slugify==8.0.1,PyYAML==5.4.1,requests==2.30.0,rich==13.3.5,ruamel.yaml==0.17.26,ruamel.yaml.clib==0.2.7,selinux==0.2.1,six==1.16.0,subprocess-tee==0.3.5,tenacity==8.2.2,text-unidecode==1.3,typing_extensions==4.5.0,urllib3==2.0.2,wcmatch==8.4.1,yamllint==1.26.3,zipp==3.15.0
py37-ansible30 run-test-pre: PYTHONHASHSEED='3069389728'
py37-ansible30 run-test: commands[0] | molecule test -s compatibility --destroy always
INFO     compatibility scenario test matrix: destroy, create, converge, destroy
INFO     Performing prerun...
INFO     Set ANSIBLE_LIBRARY=/root/.cache/ansible-compat/b984a4/modules:/root/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
INFO     Set ANSIBLE_COLLECTIONS_PATH=/root/.cache/ansible-compat/b984a4/collections:/root/.ansible/collections:/usr/share/ansible/collections
INFO     Set ANSIBLE_ROLES_PATH=/root/.cache/ansible-compat/b984a4/roles:/root/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles
INFO     Running compatibility > destroy
INFO     Sanity checks: 'podman'

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item={'image': 'pycontribs/centos:8', 'name': 'instances', 'pre_build_image': True})

TASK [Wait for instance(s) deletion to complete] *******************************
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '764640107806.9946', 'results_file': '/root/.ansible_async/764640107806.9946', 'changed': True, 'failed': False, 'item': {'image': 'pycontribs/centos:8', 'name': 'instances', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Running compatibility > create

PLAY [Create] ******************************************************************

TASK [get podman executable path] **********************************************
ok: [localhost]

TASK [save path to executable as fact] *****************************************
ok: [localhost]

TASK [Log into a container registry] *******************************************
skipping: [localhost] => (item="instances registry username: None specified")

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item=Dockerfile: None specified)

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item="Dockerfile: None specified; Image: pycontribs/centos:8")

TASK [Discover local Podman images] ********************************************
ok: [localhost] => (item=instances)

TASK [Build an Ansible compatible image] ***************************************
skipping: [localhost] => (item=pycontribs/centos:8)

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item="instances command: None specified")

TASK [Remove possible pre-existing containers] *********************************
changed: [localhost]

TASK [Discover local podman networks] ******************************************
skipping: [localhost] => (item=instances: None specified)

TASK [Create podman network dedicated to this scenario] ************************
skipping: [localhost]

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=instances)

TASK [Wait for instance(s) creation to complete] *******************************
changed: [localhost] => (item=instances)

PLAY RECAP *********************************************************************
localhost                  : ok=8    changed=3    unreachable=0    failed=0    skipped=5    rescued=0    ignored=0

INFO     Running compatibility > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [instances]

TASK [Copy something to test use of synchronize module] ************************
changed: [instances]

TASK [Include vector-role] *****************************************************

TASK [vector-role : include_tasks] *********************************************
included: /opt/vector-role/tasks/download_install_vector.yml for instances

TASK [vector-role : fixing repo for CentOS 8] **********************************
changed: [instances]

TASK [vector-role : Get vector distrib (deb)] **********************************
skipping: [instances]

TASK [vector-role : Get vector distrib (rpm)] **********************************
changed: [instances]

TASK [vector-role : Install vector packages (deb)] *****************************
skipping: [instances]

TASK [vector-role : Install vector packages (rpm)] *****************************
changed: [instances]

RUNNING HANDLER [vector-role : Start Vector service] ***************************
skipping: [instances]

PLAY RECAP *********************************************************************
instances                  : ok=6    changed=4    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0

INFO     Running compatibility > destroy

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item={'image': 'pycontribs/centos:8', 'name': 'instances', 'pre_build_image': True})

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
FAILED - RETRYING: Wait for instance(s) deletion to complete (299 retries left).
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '413971758090.12560', 'results_file': '/root/.ansible_async/413971758090.12560', 'changed': True, 'failed': False, 'item': {'image': 'pycontribs/centos:8', 'name': 'instances', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Pruning extra files from scenario ephemeral directory
py39-ansible210 installed: ansible==2.10.7,ansible-base==2.10.17,ansible-compat==4.0.2,ansible-core==2.14.5,ansible-lint==6.14.2,arrow==1.2.3,attrs==23.1.0,binaryornot==0.4.4,black==23.3.0,bracex==2.3.post1,certifi==2023.5.7,cffi==1.15.1,chardet==5.1.0,charset-normalizer==3.1.0,click==8.1.3,click-help-colors==0.9.1,cookiecutter==2.1.1,cryptography==40.0.2,distro==1.8.0,enrich==1.2.7,filelock==3.12.0,idna==3.4,Jinja2==3.1.2,jinja2-time==0.2.0,jmespath==1.0.1,jsonschema==4.17.3,lxml==4.9.2,markdown-it-py==2.2.0,MarkupSafe==2.1.2,mdurl==0.1.2,molecule==5.0.0,molecule-podman==2.0.3,mypy-extensions==1.0.0,packaging==23.1,pathspec==0.11.1,platformdirs==3.5.1,pluggy==1.0.0,pycparser==2.21,Pygments==2.15.1,pyrsistent==0.19.3,python-dateutil==2.8.2,python-slugify==8.0.1,PyYAML==6.0,requests==2.30.0,resolvelib==0.8.1,rich==13.3.5,ruamel.yaml==0.17.26,ruamel.yaml.clib==0.2.7,selinux==0.3.0,six==1.16.0,subprocess-tee==0.4.1,text-unidecode==1.3,tomli==2.0.1,typing_extensions==4.5.0,urllib3==2.0.2,wcmatch==8.4.1,yamllint==1.29.0
py39-ansible210 run-test-pre: PYTHONHASHSEED='3069389728'
py39-ansible210 run-test: commands[0] | molecule test -s compatibility --destroy always
WARNING  No schema found in podman driver.
INFO     compatibility scenario test matrix: destroy, create, converge, destroy
INFO     Performing prerun with role_name_check=0...
INFO     Set ANSIBLE_LIBRARY=/root/.cache/ansible-compat/f5bcd7/modules:/root/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
INFO     Set ANSIBLE_COLLECTIONS_PATH=/root/.cache/ansible-compat/f5bcd7/collections:/root/.ansible/collections:/usr/share/ansible/collections
INFO     Set ANSIBLE_ROLES_PATH=/root/.cache/ansible-compat/f5bcd7/roles:/root/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles
INFO     Using /root/.cache/ansible-compat/f5bcd7/roles/zzz.vector_role symlink to current repository in order to enable Ansible to find the role using its expected full name.
INFO     Running compatibility > destroy
Traceback (most recent call last):
  File "/opt/vector-role/.tox/py39-ansible210/bin/molecule", line 8, in <module>
    sys.exit(main())
  File "/opt/vector-role/.tox/py39-ansible210/lib/python3.9/site-packages/click/core.py", line 1130, in __call__
    return self.main(*args, **kwargs)
  File "/opt/vector-role/.tox/py39-ansible210/lib/python3.9/site-packages/click/core.py", line 1055, in main
    rv = self.invoke(ctx)
  File "/opt/vector-role/.tox/py39-ansible210/lib/python3.9/site-packages/click/core.py", line 1657, in invoke
    return _process_result(sub_ctx.command.invoke(sub_ctx))
  File "/opt/vector-role/.tox/py39-ansible210/lib/python3.9/site-packages/click/core.py", line 1404, in invoke
    return ctx.invoke(self.callback, **ctx.params)
  File "/opt/vector-role/.tox/py39-ansible210/lib/python3.9/site-packages/click/core.py", line 760, in invoke
    return __callback(*args, **kwargs)
  File "/opt/vector-role/.tox/py39-ansible210/lib/python3.9/site-packages/click/decorators.py", line 26, in new_func
    return f(get_current_context(), *args, **kwargs)
  File "/opt/vector-role/.tox/py39-ansible210/lib/python3.9/site-packages/molecule/command/test.py", line 113, in test
    base.execute_cmdline_scenarios(scenario_name, args, command_args, ansible_args)
  File "/opt/vector-role/.tox/py39-ansible210/lib/python3.9/site-packages/molecule/command/base.py", line 119, in execute_cmdline_scenarios
    execute_scenario(scenario)
  File "/opt/vector-role/.tox/py39-ansible210/lib/python3.9/site-packages/molecule/command/base.py", line 162, in execute_scenario
    execute_subcommand(scenario.config, action)
  File "/opt/vector-role/.tox/py39-ansible210/lib/python3.9/site-packages/molecule/command/base.py", line 152, in execute_subcommand
    return command(config).execute(args)
  File "/opt/vector-role/.tox/py39-ansible210/lib/python3.9/site-packages/molecule/logger.py", line 189, in wrapper
    rt = func(*args, **kwargs)
  File "/opt/vector-role/.tox/py39-ansible210/lib/python3.9/site-packages/molecule/command/destroy.py", line 55, in execute
    self._config.provisioner.destroy()
  File "/opt/vector-role/.tox/py39-ansible210/lib/python3.9/site-packages/molecule/provisioner/ansible.py", line 766, in destroy
    pb = self._get_ansible_playbook(self.playbooks.destroy)
  File "/opt/vector-role/.tox/py39-ansible210/lib/python3.9/site-packages/molecule/provisioner/ansible.py", line 937, in _get_ansible_playbook
    return ansible_playbook.AnsiblePlaybook(
  File "/opt/vector-role/.tox/py39-ansible210/lib/python3.9/site-packages/molecule/provisioner/ansible_playbook.py", line 54, in __init__
    self._env = self._config.provisioner.env
  File "/opt/vector-role/.tox/py39-ansible210/lib/python3.9/site-packages/molecule/provisioner/ansible.py", line 589, in env
    default_env = self.default_env
  File "/opt/vector-role/.tox/py39-ansible210/lib/python3.9/site-packages/molecule/provisioner/ansible.py", line 523, in default_env
    self._config.ansible_collections_path: ":".join(collections_path_list),
  File "/opt/vector-role/.tox/py39-ansible210/lib/python3.9/site-packages/molecule/config.py", line 127, in ansible_collections_path
    if self.runtime.version >= Version("2.10.0.dev0"):
  File "/opt/vector-role/.tox/py39-ansible210/lib/python3.9/site-packages/ansible_compat/runtime.py", line 230, in version
    self._version = parse_ansible_version(proc.stdout)
  File "/opt/vector-role/.tox/py39-ansible210/lib/python3.9/site-packages/ansible_compat/config.py", line 42, in parse_ansible_version
    raise InvalidPrerequisiteError(msg)
ansible_compat.errors.InvalidPrerequisiteError: Unable to parse ansible cli version: ansible 2.10.17
  config file = None
  configured module search path = ['/root/.cache/ansible-compat/f5bcd7/modules', '/root/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /opt/vector-role/.tox/py39-ansible210/lib/python3.9/site-packages/ansible
  executable location = /opt/vector-role/.tox/py39-ansible210/bin/ansible
  python version = 3.9.2 (default, Jun 13 2022, 19:42:33) [GCC 8.5.0 20210514 (Red Hat 8.5.0-10)]

Keep in mind that only 2.12 or newer are supported.
ERROR: InvocationError for command /opt/vector-role/.tox/py39-ansible210/bin/molecule test -s compatibility --destroy always (exited with code 1)
py39-ansible30 installed: ansible==3.0.0,ansible-base==2.10.17,ansible-compat==4.0.2,ansible-core==2.14.5,ansible-lint==6.14.2,arrow==1.2.3,attrs==23.1.0,binaryornot==0.4.4,black==23.3.0,bracex==2.3.post1,certifi==2023.5.7,cffi==1.15.1,chardet==5.1.0,charset-normalizer==3.1.0,click==8.1.3,click-help-colors==0.9.1,cookiecutter==2.1.1,cryptography==40.0.2,distro==1.8.0,enrich==1.2.7,filelock==3.12.0,idna==3.4,Jinja2==3.1.2,jinja2-time==0.2.0,jmespath==1.0.1,jsonschema==4.17.3,lxml==4.9.2,markdown-it-py==2.2.0,MarkupSafe==2.1.2,mdurl==0.1.2,molecule==5.0.0,molecule-podman==2.0.3,mypy-extensions==1.0.0,packaging==23.1,pathspec==0.11.1,platformdirs==3.5.1,pluggy==1.0.0,pycparser==2.21,Pygments==2.15.1,pyrsistent==0.19.3,python-dateutil==2.8.2,python-slugify==8.0.1,PyYAML==6.0,requests==2.30.0,resolvelib==0.8.1,rich==13.3.5,ruamel.yaml==0.17.26,ruamel.yaml.clib==0.2.7,selinux==0.3.0,six==1.16.0,subprocess-tee==0.4.1,text-unidecode==1.3,tomli==2.0.1,typing_extensions==4.5.0,urllib3==2.0.2,wcmatch==8.4.1,yamllint==1.29.0
py39-ansible30 run-test-pre: PYTHONHASHSEED='3069389728'
py39-ansible30 run-test: commands[0] | molecule test -s compatibility --destroy always
WARNING  No schema found in podman driver.
INFO     compatibility scenario test matrix: destroy, create, converge, destroy
INFO     Performing prerun with role_name_check=0...
INFO     Set ANSIBLE_LIBRARY=/root/.cache/ansible-compat/f5bcd7/modules:/root/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
INFO     Set ANSIBLE_COLLECTIONS_PATH=/root/.cache/ansible-compat/f5bcd7/collections:/root/.ansible/collections:/usr/share/ansible/collections
INFO     Set ANSIBLE_ROLES_PATH=/root/.cache/ansible-compat/f5bcd7/roles:/root/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles
INFO     Using /root/.cache/ansible-compat/f5bcd7/roles/zzz.vector_role symlink to current repository in order to enable Ansible to find the role using its expected full name.
INFO     Running compatibility > destroy
Traceback (most recent call last):
  File "/opt/vector-role/.tox/py39-ansible30/bin/molecule", line 8, in <module>
    sys.exit(main())
  File "/opt/vector-role/.tox/py39-ansible30/lib/python3.9/site-packages/click/core.py", line 1130, in __call__
    return self.main(*args, **kwargs)
  File "/opt/vector-role/.tox/py39-ansible30/lib/python3.9/site-packages/click/core.py", line 1055, in main
    rv = self.invoke(ctx)
  File "/opt/vector-role/.tox/py39-ansible30/lib/python3.9/site-packages/click/core.py", line 1657, in invoke
    return _process_result(sub_ctx.command.invoke(sub_ctx))
  File "/opt/vector-role/.tox/py39-ansible30/lib/python3.9/site-packages/click/core.py", line 1404, in invoke
    return ctx.invoke(self.callback, **ctx.params)
  File "/opt/vector-role/.tox/py39-ansible30/lib/python3.9/site-packages/click/core.py", line 760, in invoke
    return __callback(*args, **kwargs)
  File "/opt/vector-role/.tox/py39-ansible30/lib/python3.9/site-packages/click/decorators.py", line 26, in new_func
    return f(get_current_context(), *args, **kwargs)
  File "/opt/vector-role/.tox/py39-ansible30/lib/python3.9/site-packages/molecule/command/test.py", line 113, in test
    base.execute_cmdline_scenarios(scenario_name, args, command_args, ansible_args)
  File "/opt/vector-role/.tox/py39-ansible30/lib/python3.9/site-packages/molecule/command/base.py", line 119, in execute_cmdline_scenarios
    execute_scenario(scenario)
  File "/opt/vector-role/.tox/py39-ansible30/lib/python3.9/site-packages/molecule/command/base.py", line 162, in execute_scenario
    execute_subcommand(scenario.config, action)
  File "/opt/vector-role/.tox/py39-ansible30/lib/python3.9/site-packages/molecule/command/base.py", line 152, in execute_subcommand
    return command(config).execute(args)
  File "/opt/vector-role/.tox/py39-ansible30/lib/python3.9/site-packages/molecule/logger.py", line 189, in wrapper
    rt = func(*args, **kwargs)
  File "/opt/vector-role/.tox/py39-ansible30/lib/python3.9/site-packages/molecule/command/destroy.py", line 55, in execute
    self._config.provisioner.destroy()
  File "/opt/vector-role/.tox/py39-ansible30/lib/python3.9/site-packages/molecule/provisioner/ansible.py", line 766, in destroy
    pb = self._get_ansible_playbook(self.playbooks.destroy)
  File "/opt/vector-role/.tox/py39-ansible30/lib/python3.9/site-packages/molecule/provisioner/ansible.py", line 937, in _get_ansible_playbook
    return ansible_playbook.AnsiblePlaybook(
  File "/opt/vector-role/.tox/py39-ansible30/lib/python3.9/site-packages/molecule/provisioner/ansible_playbook.py", line 54, in __init__
    self._env = self._config.provisioner.env
  File "/opt/vector-role/.tox/py39-ansible30/lib/python3.9/site-packages/molecule/provisioner/ansible.py", line 589, in env
    default_env = self.default_env
  File "/opt/vector-role/.tox/py39-ansible30/lib/python3.9/site-packages/molecule/provisioner/ansible.py", line 523, in default_env
    self._config.ansible_collections_path: ":".join(collections_path_list),
  File "/opt/vector-role/.tox/py39-ansible30/lib/python3.9/site-packages/molecule/config.py", line 127, in ansible_collections_path
    if self.runtime.version >= Version("2.10.0.dev0"):
  File "/opt/vector-role/.tox/py39-ansible30/lib/python3.9/site-packages/ansible_compat/runtime.py", line 230, in version
    self._version = parse_ansible_version(proc.stdout)
  File "/opt/vector-role/.tox/py39-ansible30/lib/python3.9/site-packages/ansible_compat/config.py", line 42, in parse_ansible_version
    raise InvalidPrerequisiteError(msg)
ansible_compat.errors.InvalidPrerequisiteError: Unable to parse ansible cli version: ansible 2.10.17
  config file = None
  configured module search path = ['/root/.cache/ansible-compat/f5bcd7/modules', '/root/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /opt/vector-role/.tox/py39-ansible30/lib/python3.9/site-packages/ansible
  executable location = /opt/vector-role/.tox/py39-ansible30/bin/ansible
  python version = 3.9.2 (default, Jun 13 2022, 19:42:33) [GCC 8.5.0 20210514 (Red Hat 8.5.0-10)]

Keep in mind that only 2.12 or newer are supported.
ERROR: InvocationError for command /opt/vector-role/.tox/py39-ansible30/bin/molecule test -s compatibility --destroy always (exited with code 1)
_______________________________________________________ summary ________________________________________________________
  py37-ansible210: commands succeeded
  py37-ansible30: commands succeeded
ERROR:   py39-ansible210: commands failed
ERROR:   py39-ansible30: commands failed
 ```
7. Добавьте новый тег на коммит с рабочим сценарием в соответствии с семантическим версионированием.

После выполнения у вас должно получится два сценария molecule и один tox.ini файл в репозитории. Не забудьте указать в ответе теги решений Tox и Molecule заданий. В качестве решения пришлите ссылку на  ваш репозиторий и скриншоты этапов выполнения задания. 
