# Сабмодули git
## Подготовка к прочтению
Прежде всего, прочтите, [как мы работаем с Git](Git.md).

## Что из себя представляют
Сабмодули git - это репозитории git, логически связанные с вашим. При подключении сабмодуля в ваш репозиторий образуется подобие иерархии репозиториев (в смысле дерева, где "ветка дерева" - ваш репозиторий, а "листья" - сабмодули). При этом изменения в сабмодулях управляются отдельно, так, как будто вы имеете дело с рабочей копией репозитория, подключенного в качестве сабмодуля, существующей независимо. Для того, чтобы обеспечить повторимое одинаковое для всех разработчиков окружение, ваш репозиторий всегда ссылается на конкретный коммит (конкретное состояние) сабмодуля.

## Основные операции
### Инициализация сабмодулей
Если вы не проинициализируете сабмодули специально, когда делаете `git clone`, в каталогах, в которые обычно подключены сабмодули, не будет ничего:
```
➜  Projects git clone git@gitlab.lhhh.spb.ru:ooga/booga-dooga.git ./booga-dooga-test
Cloning into './booga-dooga-test'...
remote: Counting objects: 457, done.
remote: Compressing objects: 100% (45/45), done.
remote: Total 457 (delta 26), reused 47 (delta 20)
Receiving objects: 100% (457/457), 51.35 KiB | 1.32 MiB/s, done.
Resolving deltas: 100% (259/259), done.
➜  Projects ls -alF ./booga-dooga-test/roles/etckeeper
total 0
drwxr-xr-x  2 chistyakov staff   64 Jan  6 03:20 ./
drwxr-xr-x 47 chistyakov staff 1504 Jan  6 03:20 ../
➜  Projects
```
Это, очевидно, не тот результат, который вы планировали.     
Есть два варианта решения данной проблемы:

#### Инициализация сабмодулей в момент создания клона репозитория
Необходимо выполнить команду `git clone` с ключом `--recursive`:
```
➜  Projects git clone --recursive git@gitlab.lhhh.spb.ru:ooga/booga-dooga.git ./booga-dooga-test
Cloning into './booga-dooga-test'...
remote: Counting objects: 457, done.
remote: Compressing objects: 100% (45/45), done.
remote: Total 457 (delta 26), reused 47 (delta 20)
Receiving objects: 100% (457/457), 51.35 KiB | 1.22 MiB/s, done.
Resolving deltas: 100% (259/259), done.
Submodule 'roles/ansible' (https://github.com/kofonfor/ansible-role-ansible.git) registered for path 'roles/ansible'
Submodule 'roles/collectd' (https://github.com/kofonfor/ansible-role-collectd.git) registered for path 'roles/collectd'
Submodule 'roles/common-utils' (https://github.com/kofonfor/ansible-role-common-utils.git) registered for path 'roles/common-utils'
Submodule 'roles/docker-io' (https://github.com/kofonfor/ansible-role-docker-io.git) registered for path 'roles/docker-io'
Submodule 'roles/docker.ubuntu' (https://github.com/angstwad/docker.ubuntu.git) registered for path 'roles/docker.ubuntu'
Submodule 'roles/etckeeper' (https://github.com/kofonfor/ansible-role-etckeeper.git) registered for path 'roles/etckeeper'
.......
Submodule 'roles/zookeeper' (https://github.com/kofonfor/ansible-role-zookeeper.git) registered for path 'roles/zookeeper'
Cloning into '/Users/chistyakov/Projects/booga-dooga-test/roles/ansible'...
remote: Counting objects: 8, done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 8 (delta 1), reused 8 (delta 1), pack-reused 0
Cloning into '/Users/chistyakov/Projects/booga-dooga-test/roles/collectd'...
remote: Counting objects: 98, done.
remote: Compressing objects: 100% (14/14), done.
remote: Total 98 (delta 7), reused 15 (delta 4), pack-reused 77
Cloning into '/Users/chistyakov/Projects/booga-dooga-test/roles/common-utils'...
remote: Counting objects: 243, done.
remote: Total 243 (delta 0), reused 0 (delta 0), pack-reused 243
Receiving objects: 100% (243/243), 32.20 KiB | 286.00 KiB/s, done.
Resolving deltas: 100% (103/103), done.
Cloning into '/Users/chistyakov/Projects/booga-dooga-test/roles/docker-io'...
remote: Counting objects: 114, done.
remote: Total 114 (delta 0), reused 0 (delta 0), pack-reused 114
Receiving objects: 100% (114/114), 19.65 KiB | 98.00 KiB/s, done.
Resolving deltas: 100% (32/32), done.
Cloning into '/Users/chistyakov/Projects/booga-dooga-test/roles/docker.ubuntu'...
remote: Counting objects: 958, done.
remote: Total 958 (delta 0), reused 0 (delta 0), pack-reused 958
Receiving objects: 100% (958/958), 182.28 KiB | 755.00 KiB/s, done.
Resolving deltas: 100% (462/462), done.
Cloning into '/Users/chistyakov/Projects/booga-dooga-test/roles/etckeeper'...
remote: Counting objects: 89, done.
remote: Total 89 (delta 0), reused 0 (delta 0), pack-reused 89
.......
Cloning into '/Users/chistyakov/Projects/booga-dooga-test/roles/zookeeper'...
remote: Counting objects: 112, done.
remote: Total 112 (delta 0), reused 0 (delta 0), pack-reused 112
Receiving objects: 100% (112/112), 18.62 KiB | 162.00 KiB/s, done.
Resolving deltas: 100% (33/33), done.
Submodule path 'roles/ansible': checked out '0e72616664b71032574abdbfbdd414fc7b1902b4'
Submodule path 'roles/collectd': checked out '023303d973ad52e5cc07016584cd2086392e2720'
Submodule path 'roles/common-utils': checked out 'f66909b169c8a3dea6cd87709e34ff64775decfc'
Submodule path 'roles/docker-io': checked out '997b12d7455451163e0c9c9d318ab2232a859f07'
Submodule path 'roles/docker.ubuntu': checked out '82295b90d5d2877741295382cf6016a6cdc50269'
Submodule path 'roles/etckeeper': checked out '297082f051232a0c52b2f3d8aacb5b81352d47e2'
........
Submodule path 'roles/zookeeper': checked out '01d924a0b62e6cfa52a10df74b5e8bcb54c96ed8'
➜  Projects ls -alF ./booga-dooga-test/roles/etckeeper
total 8
drwxr-xr-x  7 chistyakov staff  224 Jan  6 03:25 ./
drwxr-xr-x 47 chistyakov staff 1504 Jan  6 03:24 ../
-rw-r--r--  1 chistyakov staff   43 Jan  6 03:24 .git
-rw-r--r--  1 chistyakov staff 1067 Jan  6 03:25 LICENSE
drwxr-xr-x  3 chistyakov staff   96 Jan  6 03:25 defaults/
drwxr-xr-x  9 chistyakov staff  288 Jan  6 03:25 tasks/
drwxr-xr-x  4 chistyakov staff  128 Jan  6 03:25 templates/
➜  Projects
```
Сам я обычно всегда забываю о ключе `--recursive`, поэтому пользуюсь вторым вариантом.

#### Инициализация сабмодулей после создания клона репозитория
Если вы забыли проинициализировать сабмодули в момент создания клона репозитория, не отчаивайтесь. Вам необходимо выполнить команду `git submodule update --init` в том каталоге, куда вы сделали клон репозитория:
```
➜  Projects git clone git@gitlab.lhhh.spb.ru:ooga/booga-dooga.git ./booga-dooga-test
Cloning into './booga-dooga-test'...
remote: Counting objects: 457, done.
remote: Compressing objects: 100% (45/45), done.
remote: Total 457 (delta 26), reused 47 (delta 20)
Receiving objects: 100% (457/457), 51.35 KiB | 1.22 MiB/s, done.
Resolving deltas: 100% (259/259), done.
➜  Projects ls -alF ./booga-dooga-test/roles/etckeeper
total 0
drwxr-xr-x  2 chistyakov staff   64 Jan  6 03:36 ./
drwxr-xr-x 47 chistyakov staff 1504 Jan  6 03:36 ../
➜  Projects cd booga-dooga-test
➜  booga-dooga-test git:(master) git submodule update --init
Submodule 'roles/ansible' (https://github.com/kofonfor/ansible-role-ansible.git) registered for path 'roles/ansible'
Submodule 'roles/collectd' (https://github.com/kofonfor/ansible-role-collectd.git) registered for path 'roles/collectd'
Submodule 'roles/common-utils' (https://github.com/kofonfor/ansible-role-common-utils.git) registered for path 'roles/common-utils'
Submodule 'roles/docker-io' (https://github.com/kofonfor/ansible-role-docker-io.git) registered for path 'roles/docker-io'
Submodule 'roles/docker.ubuntu' (https://github.com/angstwad/docker.ubuntu.git) registered for path 'roles/docker.ubuntu'
Submodule 'roles/etckeeper' (https://github.com/kofonfor/ansible-role-etckeeper.git) registered for path 'roles/etckeeper'
.......
Submodule 'roles/zookeeper' (https://github.com/kofonfor/ansible-role-zookeeper.git) registered for path 'roles/zookeeper'
Cloning into '/Users/chistyakov/Projects/booga-dooga-test/roles/ansible'...
Cloning into '/Users/chistyakov/Projects/booga-dooga-test/roles/collectd'...
Cloning into '/Users/chistyakov/Projects/booga-dooga-test/roles/common-utils'...
Cloning into '/Users/chistyakov/Projects/booga-dooga-test/roles/docker-io'...
Cloning into '/Users/chistyakov/Projects/booga-dooga-test/roles/docker.ubuntu'...
Cloning into '/Users/chistyakov/Projects/booga-dooga-test/roles/etckeeper'...
.......
Cloning into '/Users/chistyakov/Projects/booga-dooga-test/roles/zookeeper'...
Submodule path 'roles/ansible': checked out '0e72616664b71032574abdbfbdd414fc7b1902b4'
Submodule path 'roles/collectd': checked out '023303d973ad52e5cc07016584cd2086392e2720'
Submodule path 'roles/common-utils': checked out 'f66909b169c8a3dea6cd87709e34ff64775decfc'
Submodule path 'roles/docker-io': checked out '997b12d7455451163e0c9c9d318ab2232a859f07'
Submodule path 'roles/docker.ubuntu': checked out '82295b90d5d2877741295382cf6016a6cdc50269'
Submodule path 'roles/etckeeper': checked out '297082f051232a0c52b2f3d8aacb5b81352d47e2'
.......
Submodule path 'roles/zookeeper': checked out '01d924a0b62e6cfa52a10df74b5e8bcb54c96ed8'
➜  booga-dooga-test git:(master) ls -alF ./roles/etckeeper
total 8
drwxr-xr-x  7 chistyakov staff  224 Jan  6 03:37 ./
drwxr-xr-x 47 chistyakov staff 1504 Jan  6 03:36 ../
-rw-r--r--  1 chistyakov staff   43 Jan  6 03:37 .git
-rw-r--r--  1 chistyakov staff 1067 Jan  6 03:37 LICENSE
drwxr-xr-x  3 chistyakov staff   96 Jan  6 03:37 defaults/
drwxr-xr-x  9 chistyakov staff  288 Jan  6 03:37 tasks/
drwxr-xr-x  4 chistyakov staff  128 Jan  6 03:37 templates/
➜  booga-dooga-test git:(master)
```
### Сохранение изменений в сабмодулях
### Применение изменений, сделанных другим человеком
### Удаление ошибочно добавленного сабмодуля
### Релокация сабмодуля при переезде его основного репозитория
