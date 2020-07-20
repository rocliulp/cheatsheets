# cheatsheets
cheatsheets

# bash & linux
```bash
# http://ezprompt.net/ a tool for PS1 settings.
PS1="\[\e[35m\]\j\s\v \D{%Y-%m-%dT%H:%M:%S%z} \u@\h:\w/\[\e[m\]"
alias ls='ls -atlrh --color=auto'
bash -c "ls -atlrh /tmp"
readlink -f file.txt # show full path of a file
ls -atlrhd name*tag
mkdir -p /dir1/dir2/{dira,dirb}
for i in {1..150}; do echo "$i"; sleep 1; done
while true; do printf .; sleep 5;done
while [[ true ]]; do ls -atlrh; sleep 5; done

# data.txt
# 1	2	3	4
# a	b	c	d
# 5	6	7	8
# q	w	e	e

# text tools 
sed -e '/a/,/e/ s/5/55/g' data.txt
#1	2	3	4
#a	b	c	d
#55	6	7	8
#q	w	e	e
sed -e '/a/,/e/ s/5/55/g' -e '/1/,/a/ s/2/22/g' data.txt
#1	22	3	4
#a	b	c	d
#55	6	7	8
#q	w	e	e
sed -e '/a/,/e/ s/^\(.*\)6\(.*\)$/\166\2/g' -e '/1/,/a/ s/2/22/g' data.txt # Replace specific words/patterns by sed. Backreference sometimes get same result than awk
#1	22	3	4
#a	b	c	d
#5	66	7	8
#q	w	e	e
sed -e '/a/,/e/ s/^\(.*\)6\(.*\)$/\166\2/g' data.txt | awk '/1/,/a/ {$2=$2$2;print}'
#1 22 3 4
#a bb c d
sed -e '/a/,/e/ s/5/55/g' data.txt  | awk '/a/,/e/ {print $1}'
#a
#55
#q
sed -e '/a/,/e/ s/5/55/g' data.txt  | awk '/a/,/e/ {$2=$2$2;print}' # replace by awk, replace specific columns
#a bb c d
#55 66 7 8
#q ww e e
awk '/0.5/ {print $0}' teams.txt
awk '/0.5/ {print $1,$2}' teams.txt
awk '/^[0-9]/ {print $1}' teams.txt
awk '/A1/ {print $NF}' file

uuidgen
cmd1 && cmd2 && cmd3
cmd1 || cmd2 || cmd3

zip –r filename.zip directory_name
unzip -x -d dir file.zip

echo "aaabbbccc" | base64
echo "YWFhYmJiY2NjCg==" | base64 --decode

nohup myprogram </dev/null >myprogram.log 2>&1 &

# curl with user:pwd
curl -u username:password http://example.com
# curl upload file
curl -i -X PUT -H "Content-Type: multipart/form-data" -F "apivarname=@locfilepath" -vvv http://localhost:8080/api/v1/method
# http://host:port(8080/8081/...)/swagger-ui.html
# curl output and pipe redirect
curl -X PUT http://localhost:8080/api/v1/method -o std.out > out.std 2>&1 &

# Install package from source without root/sudo
wget http://sourceforge.net/projects/sshpass/files/latest/download -O sshpass.tar.gz
tar -xvf sshpass.tar.gz
cd sshpass-1.06/
./configure --help
./configure --prefix='/home/dir/user/sshpass/bin'
make
make install

# ssh&scp usage
alias ssh='ssh -q -o "UserKnownHostsFile=/dev/null" -o "StrictHostKeyChecking=no" -o "ForwardAgent=yes" -o "TCPKeepAlive=yes" -o "ServerAliveInterval=30"'
scp -o "ForwardAgent=yes" -i id_rsa -F config -oProxyJump=jmp user@host:/tmp/file locfile
sshpass -e scp -J jumphost1,jumphost2 -F config -q -o "UserKnownHostsFile=/dev/null" -o "StrictHostKeyChecking=no" -o "ForwardAgent=yes" deployment.yaml user@host:/directories/

sshpass -f filepath ssh -J jumphost1,jumphost2 -/F config -q -o "UserKnownHostsFile=/dev/null" -o "StrictHostKeyChecking=no" -o "ForwardAgent=yes" user@host 'cmd'
```
## ssh config
```config
Host jmp
    Hostname host
    User logonuser
    IdentityFile ~/.ssh/id_rsa
    HostKeyAlgorithms=+ssh-dss
    ForwardAgent yes
    LogLevel error
    StrictHostKeyChecking no
    UserKnownHostsFile=/dev/null
    TCPKeepAlive yes
    ServerAliveInterval 30
```
## DNS
```bash
# https://www.hostinger.com/tutorials/how-to-use-the-dig-command-in-linux/
dig +answer +short hostname.corp.com
dig +answer +short -x ip.ip.ip.ip
dig hostinger.com +trace
https://www.cyberciti.biz/faq/linux-unix-dig-command-examples-usage-syntax/
dig Hostname
dig DomaiNameHere
dig @DNS-server-name Hostname
dig @DNS-server-name IPAddress
dig @DNS-server-name Hostname|IPAddress type
```
## cursor movement

Move by |   Forward |   Backward
--------|-----------|-----------
word    |   M-f     |   M-b
line    |   C-e     |   C-a
switch line/current |   C-x C-x |   C-x C-x
## delete characters
del by  |   End/Forward |   Begin/Backward
--------|---------------|------------
word    |               |   M-Backspace
char    |   C-d         |   Backspace
space   |               |   C-w
line    |   C-u         |   C-k

# docker
```bash
pkill -9 containerd-shim

# docker ps
# https://docs.docker.com/engine/reference/commandline/ps/
docker ps --filter "label=color=blue"
docker ps --filter "name=nostalgic_stallman"
docker ps -a --filter 'exited=137'
docker ps --filter status=paused
# Output format
docker ps --format "{{.ID}}: {{.Command}}"
docker ps --filter volume=remote-volume --format "table {{.ID}}\t{{.Mounts}}"
docker ps --format "table {{.ID}}\t{{.Labels}}"

# docker images
# https://docs.docker.com/engine/reference/commandline/images/
# Untaged images
docker images --filter "dangling=true"
docker rmi $(docker images -f "dangling=true" -q)
docker images | grep image | awk '{ print $3 }' | xargs --no-run-if-empty docker rmi -f
docker images --filter "label=com.example.version"
docker images --filter=reference='busy*:uclibc' --filter=reference='busy*:glibc'
# Output format
docker images --format "{{.ID}}: {{.Repository}}"
docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"

# docker exec
docker exec -e COLUMNS="`tput cols`" -e LINES="`tput lines`" -ti name bash

# docker run
docker run --rm --name myname --hostname $(hostname -s) -e ENVNAME=MYENV myimage:mytag
docker run --detach --name myname --hostname $(hostname -s) -e ENVNAME=MYENV myimage:mytag
docker run --rm -it --entrypoint=/bin/sh busybox:1.31.1
# progrium/busybox
# alpine:3.11.6
# python:3.8.3-alpine3.11
# joffotron/docker-net-tools
# nicolaka/netshoot
# raesene/alpine-nettools
# praqma/network-multitool
# amouat/network-utils
# pstauffer/curl
# byrnedo/alpine-curl 
docker inspect container

# docker build
docker build -t image:tag -f Dockerfile .
docker build -t docker-register-FQDN.com/repo/image-name:tag .
docker push docker-register-FQDN.com/repo/image-name:tag
docker tag docker-register-FQDN.com/repo/image-name:oldtag docker-register-FQDN.com/repo/image-name:newtag
# untag. image id does not work here.
docker rmi docker-register-FQDN.com/repo/image-name:oldtag
# docker save
docker save image:tag -o image-tag.tgz

# docker-compose
docker-compose -f docker-compose.yaml build
docker-compose -f docker-compose.yaml up -d 
docker-compose -f docker-compose.yaml down

# registery
curl -u usr:pwd -X GET https://docker-register.fqdn.com/v2/repo/_catalog
curl -u usr:pwd -X GET https://docker-register.fqdn.com/v2/repo/img/tags/list
```

# k8s
https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward

https://kubernetes.io/zh/docs/reference/kubectl/cheatsheet/

```bash
# https://medium.com/faun/kubectl-commands-cheatsheet-43ce8f13adfb
# explain to check the yaml filelds usage 
kubectl explain svc
# pod
kubectl -n ns get deployment
kubectl -n ns exec -ti pod -- sh
kubectl -n ns cp etc pod:directories/etc # root directory does not need '/' to avoid error: tar: removing leading '/' from member names
kubectl -n ns cp pod:directories/etc etc # root directory does not need '/' to avoid error: tar: removing leading '/' from member names
kubectl cp ns/pod:root-dir/dir/file local-dir # Did not verify that yet.
# There might be articles showing cp files/dirs from a pod to another but so far I did not see any success when running that.

kubectl -n ns logs --tail 100 -f pod
kubectl -n ns rollout restart -f deployment.yaml
kubectl rollout restart deployment mydeploy
# You can set some environment variable which will force your deployment pods to restart:
kubectl set env deployment mydeploy DEPLOY_DATE="$(date)"
## {
kubectl scale deployment mydeploy --replicas=0
kubectl scale deployment mydeploy --replicas=1
## }
kubectl -n ns delete pod pdname --grace-period=0 --force
# service
kubectl -n ns get svc
kubectl -n ns delete service1 servic2 service3
# deployment
kubectl delete deployment deployment1 deployment2 deployment3
# configmap
kubectl get configmap -n ns
# secret
kubectl create secret generic my-secret -n ns --from-file=PASSPHRASE=PASSPHRASE.txt --from-file=MY_PRIVATE_KEY=MY_PRIVATE_KEY.txt
kubectl create secret generic my-secret -n ns --from-literal=ADMIN_USER_NAME=admin --from-file=ADMIN_USER_TOKEN=ADMIN_USER_ACCESS_TOKEN.txt
kubectl get secret -n ns
kubectl describe secret my-secret -n ns
# filter/selector/sort
# https://kubernetes.io/zh/docs/concepts/overview/working-with-objects/field-selectors/
# https://kubernetes.io/zh/docs/concepts/overview/working-with-objects/labels/
# https://kubernetes.io/zh/docs/tasks/access-application-cluster/list-all-running-container-images/
kubectl get services --sort-by=.metadata.name # 列出当前命名空间下所有 services，按照名称排序
kubectl get pods --sort-by='.status.containerStatuses[0].restartCount'
kubectl get pods -n test --sort-by=.spec.capacity.storage
# https://kubernetes.io/zh/docs/concepts/overview/working-with-objects/field-selectors/
# metadata.name=my-service
# metadata.namespace!=default
# status.phase=Pending
kubectl get pods --field-selector status.phase=Running
kubectl get services  --all-namespaces --field-selector metadata.namespace!=default
kubectl get pods --field-selector=status.phase!=Running,spec.restartPolicy=Always
kubectl get statefulsets,services --all-namespaces --field-selector metadata.namespace!=default
# Get the version label of all pods with label app=cassandra
kubectl get pods --selector=app=cassandra -o jsonpath='{.items[*].metadata.labels.version}'
kubectl get pod,svc --selector=app=applabelname
kubectl get all --selector=app=applabelname
# https://blog.mayadata.io/openebs/kubernetes-label-selector-and-field-selector
kubectl get pod --field-selector metadata.name=redis-aaabbbccc-w895j -n ns # need full name. Cannot regex
kubectl get pod --field-selector spec.name=label-example
# pod&svc&run&mariadb&mysql&network diagnostics
# https://kubernetes.io/zh/docs/tasks/run-application/run-single-instance-stateful-application/
# https://github.com/GoogleCloudPlatform/click-to-deploy/tree/master/k8s/mariadb
kubectl run -it --rm --image=owner/mariadb:tag --restart=Never mysql-client -- mysql -h svcname.namespace.svc.cluster.local -u root -ppasswd
kubectl -n ns run --rm -it --image=radial/busyboxplus:curl --restart=Never curl
kubectl -n ns run --rm --image=radial/busyboxplus:curl --restart=Never curl -- curl url
kubectl -n ns run --rm -it --image=busybox --restart=Never mybusybox -- sh
kubectl -n ns exec  podcontainer -- mysqldump --all-databases --add-drop-database --add-drop-table --single-transaction -uroot -ppasswd > dump-file.sql
kubectl -n ns exec  podcontainer -- mysqldump --databases db1 db2 db3 db4 --add-drop-database --add-drop-table --single-transaction -uroot -ppasswd > dump-file.sql
nohup kubectl -n ns exec podcontainer -- mysql -uroot -ppasswd -e "source /tmp/dump-file.sql;" &
# secret
# https://opensource.com/article/19/6/introduction-kubernetes-secrets-and-configmaps
kubectl get secret mariadb-root-password -o jsonpath='{.data.password}'
kubectl get secret mariadb-root-password -o jsonpath='{.data.password}' | base64 --decode -
kubectl -n ns get secret secretname --template={{.data.keyname}} | base64 --decode -
kubectl -n dev get secret sre-grafana-secret -o
# node
kubectl get nodes -l "nodetype=mylabel"
# taint node https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/
kubectl taint node mynode key=value:NoSchedule
# stats & monitoring
kubectl top pod -n nsname
kubectl top node -n nsname

# port-forward - https://kubernetes.io/zh/docs/tasks/access-application-cluster/port-forward-access-application-cluster/ - 与本地 7000 端口建立的连接将转发到运行 Redis 服务器的 pod 的 6379 端口。通过此连接，您可以使用本地工作站来调试在 pod 中运行的数据库。
kubectl port-forward redis-master-765d459796-258hz 7000:6379
kubectl port-forward pods/redis-master-765d459796-258hz 7000:6379
kubectl port-forward deployment/redis-master 7000:6379
kubectl port-forward rs/redis-master 7000:6379
kubectl port-forward svc/redis-master 7000:6379
redis-cli -p 7000
```

# git
```bash
git config --global user.name "name"

cd localgitrepopath
git config user.name "name"

git clone https://github.com/acct/url.git
# edit code/file
git add ./yourfiles
git commit -m "msg"
git push

# get latest code regardless local changes
# rm -rf .
git reset --hard HEAD
git clean -xffd
git pull

# branch
git branch -a # list all branches
git branch bname # create a new branch
git checkout bname # switch to another branch
git checkout -b bname # create & switch to a new branch
git push origin --delete bname # remove a remote branch

git status

# merge specific branch to master workflow
git checkout master
git branch new-branch
git checkout new-branch
# ...develop some code...
git add –A
git commit –m "Some commit message"
git checkout master
git merge new-branch # merger parameter branch(new-branch) to the current working branch(master)
```
# springboot
```bash
java -jar -Dspring.profiles.active=activeprofilename app.jar
```
# maven
```bash
mvn clean package -e -U -X
mvn validate
mvn clean package -e -U -X -DskipTests
mvn clean package -e -U -DskipTests -ff -q

mvn dependency:tree
mvn dependency:list
mvn dependency:analyze
```
# gradle
[Gradle scripts](https://github.com/davenkin/gradle-learning)
```

```
# redis
```bash
redis-server redis.conf
redis-cli -c -p port -h host

# scan
scan 0
scan 17
sscan myset 0 match f*
scan 176 MATCH *11* COUNT 1000
SCAN 0 TYPE zset
hscan hash 0
```

# sql
```sql
-- mysql date:          str_to_date('2000-01-01 00:00:00', '%Y-%m-%d %T')
-- oracle date:         to_date('2000-01-01 01:00:00', 'YYYY-MM-DD HH24:MI:SS')
-- oracle date default: to_date('23-JUL-19 05.42.32.871000000 AM', 'DD-MON-RRRR HH12.MI.SS.SSSSS AM')

select count(*), column1,column2 from tbname where column1=value1 and column2=value2 and column3 in (v1, v2, v3) and date1 between to_date('dstr', 'fstr') and to_date('dstr', 'fstr') group by column1, column2 having count(*) > 50 order by count(*);
-- mysql
SELECT 1 FROM table WHERE a = 1 AND b = 2 LIMIT 1;
-- oracle
SELECT 1 FROM table WHERE a = 1 AND b = 2 and rownum < 2;

SELECT * from mytable WHERE mycolumn = myvalue AND ROWNUM < 100;
-- oracle topk
select * from (select * from my_view where alert_level=3 order by alert_time desc) where rownum<=10;-- performance issues if many records
SELECT column1, column2 FROM (SELECT column1, column2, ROW_NUMBER() OVER (ORDER BY column1) RNK FROM mytable WHERE column1 >= 10000 AND column2 = myvalue) WHERE RNK < 100000 ORDER BY column1;
select  column1, column2 from mytable order by column1 asc fetch first 10000 rows only; -- need 12c and later

--mysql, query size/record counts of each database/schema
-- https://www.a2hosting.com/kb/developer-corner/mysql/determining-the-size-of-mysql-databases-and-tables
SELECT table_schema AS "Database", ROUND(SUM(data_length + index_length) / 1024 / 1024, 2) AS "Size (MB)" FROM information_schema.TABLES GROUP BY table_schema;
SELECT table_name AS "Table", ROUND(((data_length + index_length) / 1024 / 1024), 2) AS "Size (MB)" FROM information_schema.TABLES WHERE table_schema = "database_name" ORDER BY (data_length + index_length) DESC;
-- https://stackoverflow.com/questions/286039/get-record-counts-for-all-tables-in-mysql-database
SELECT TABLE_NAME,SUM(TABLE_ROWS) FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_SCHEMA = 'your_db' GROUP BY TABLE_NAME;
SELECT table_schema 'Database', SUM(data_length + index_length) AS 'DBSize', SUM(TABLE_ROWS) AS DBRows, SUM(AUTO_INCREMENT) AS DBAutoIncCount FROM information_schema.tables GROUP BY table_schema;

--mysql
DROP DATABASE IF EXISTS db;
CREATE DATABASE db DEFAULT CHARACTER SET utf8;
CREATE USER IF NOT EXISTS 'user'@'%' IDENTIFIED BY 'pass';
CREATE USER IF NOT EXISTS 'user'@'localhost' IDENTIFIED BY 'pass';
GRANT ALL PRIVILEGES ON *.* TO 'user'@'%';
GRANT ALL PRIVILEGES ON *.* TO 'user'@'localhost';
FLUSH PRIVILEGES;

REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'user'@'%', 'user'@'localhost';
FLUSH PRIVILEGES;

DROP USER IF EXISTS 'user'@'%';
DROP USER IF EXISTS 'user'@'localhost';
```

# python
```bash
docker run -it --rm -it --entrypoint=/bin/sh --name py3alpine python:3.8.3-alpine3.11
docker run -it --rm -it --entrypoint=/bin/sh --name py2 python:2
```
```python

```

# prometheus
```prometheus
# metric console query 
metric_name{label="value"}[15m] offset 5m
sum(metrics_name{label1="value1", label2="value2"})
sum(predict_linear(metric_name{label1="value1",label2="value2"}[20m], 3600*5))
sum(predict_linear(metric_name{label1="value1",label2="value2",label3="value3",label4="value4"}[1h],4*3600))<0
```
# emacs
## cursor movement
Move by     |   Forward |   Backward
------------|-----------|-----------
character   |   C-f     |   C-b
word        |   M-f     |   M-b
line        |   C-n     |   C-p
screen      |   C-v     |   M-v

Move to     |   Beginning of    |   End of
------------|-------------------|-----------
line        |   C-a             |   C-e
sentence    |   M-a             |   M-e
paragraph   |   M-{             |   M-}
buffer      |   M-<             |   M->

Notice that the commands are somewhat mnemonic:
* "f" stands for "forward"
* "b" stands for "backward"
* "n" stands for "next"
* "p" stands for "previous"
* "a" stands for "beginning" (like the beginning of the alphabet)
* "e" stands for "end"

# sqlplus
```bash
echo '127.0.0.1 ${HOSTNAME}' >> /etc/hosts # For sql on Mac sometimes
rlwrap sqlplus usr/pwd@//host:port/dbins
rlwrap sqlplus usr/pwd@//localhost:port/dbins
```
# vim
```vim
" vim on Mac Terminal, change encoding for Chinese:
:e ++enc=gb2312 filename

" Remove invisible newline at the end of line. 'echo -n "string"'?
:set noendofline binary
:wq
```

# mysql
https://gist.github.com/hofmannsven/9164408
```bash
# mycli - https://hub.docker.com/r/diyan/mycli
docker run --rm diyan/mycli --help
docker run -d --name=mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=secret mysql:5.7
docker run --rm -ti --name=mycli --link=mysql:mysql diyan/mycli --host=mysql --database=mysql --user=root --password=secret
# https://github.com/chaifeng/docker-mysql-mycli -- looks better...
docker run --rm -it --name mycli -e MYSQL_DATABASE=dbname -e MYSQL_HOST=dbhost -e MYSQL_USER=root -e MYSQL_PASSWORD="secret" --network db_default chaifeng/mycli

docker run --name dbname -e MYSQL_ROOT_PASSWORD=root_pass -e MYSQL_ALLOW_EMPTY_PASSWORD=true -e MYSQL_USER=dbuser -e MYSQL_PASSWORD=dbpass -e MYSQL_DATABASE=dbname -v /data/dbname:/var/lib/mysql -p 3306:3306 --restart=on-failure -d mariadb:10.3
docker run --rm -ti --name=mycli --link=dbname:dbname diyan/mycli --host=dbname --user=root --password=root_pass
docker exec -it dbcontainer mysql -N -B --raw -u dbuser -pdbpass db -e "SELECT * FROM mytable;"
```

# tmux
[tmuxcheatsheet.com](https://tmuxcheatsheet.com/)

Shortcuts                                   | decription
--------------------------------------------|---------------------------
Ctrl+b - "                                  | split a pane horizontally
Ctrl+b - %                                  | split a pane vertically 
Ctrl+b - z                                  | zoom/unzoom current pane 
Ctrl+b - Arrow keys (Left, Right, Up, Down) | witch between panes 
Ctrl+b - [ | use your normal navigation keys(Up, PgDn) to scroll around then 'q' to quit to normal
Ctrl+b - PgUp | go directly into copy mode and scroll one page up
set -g mouse on        | #For tmux version 2.1 and up
set -g mode-mouse on  |  #For tmux versions < 2.1
tmux -u | UTF-8
[gpakosz tmux config](https://github.com/gpakosz/.tmux)
http://louiszhai.github.io/2017/09/30/tmux/
https://readthedocs.org/projects/tao-of-tmux-chinese/downloads/pdf/latest/

# Jenkins
[Pipeline scripts](https://www.w3cschool.cn/jenkins/jenkins-jg9528pb.html)

[Global Variable Reference: docker, pipeline, env, params, currentBuild, scm objects usage](https://opensource.triology.de/jenkins/pipeline-syntax/globals)

Jenkinsfile Pipeline有两种基本格式：
1. Declarative Pipeline （新）
2. Scripted Pipeline （旧）

其中的第二种Scripted Pipeline比较古老。它功能强大。允许通过脚本写出较高级的Pipeline。但使用不一定便捷。有些使用甚至繁琐费力。

第一种Declarative Pipeline更常见些并且也更便捷。可能需要Config中的Plugins设置配合。
总的来说Declarative Pipeline中也能实现Scripted Pipeline的功能。需要将脚本写在Pipeline -> Stages -> Stage -> steps - script的block中。基本能实现相同的效果。

Declarative Pipeline的最外层为Pipeline {}。

Scripted Pipeline的最外层为node {}。

# markdown

[Github Mastering-markdown](https://guides.github.com/features/mastering-markdown/)

[runoob.com Grammar](https://www.runoob.com/markdown/md-advance.html)

[appinn Grammar](https://www.appinn.com/markdown/)

