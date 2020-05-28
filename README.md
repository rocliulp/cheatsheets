# cheatsheets
cheatsheets

## bash & linux
```bash
ls -atlrhd name*tag
mkdir -p /dir1/dir2/{dira,dirb}
for i in {1..150}; do echo "$i"; sleep 1; done
while true; do printf .; sleep 5;done

awk '/0.5/ {print $0}' teams.txt
awk '/0.5/ {print $1,$2}' teams.txt
awk '/^[0-9]/ {print $1}' teams.txt
awk '/A1/ {print $NF}' file
uuidgen
cmd1 && cmd2 && cmd3
cmd1 || cmd2 || cmd3

# curl with user:pwd
curl -u username:password http://example.com
# curl upload file
curl -i -X PUT -H "Content-Type: multipart/form-data" -F "apivarname=@locfilepath" -vvv http://localhost:8080/api/v1/method
# http://host:port(8080/8081/...)/swagger-ui.html


# ssh usage
sshpass -e scp -J jumphost1,jumphost2 -F config -q -o "UserKnownHostsFile=/dev/null" -o "StrictHostKeyChecking=no" -o "ForwardAgent=yes" deployment.yaml user@host:/directories/

sshpass -f filepath ssh -J jumphost1,jumphost2 -F config -q -o "UserKnownHostsFile=/dev/null" -o "StrictHostKeyChecking=no" -o "ForwardAgent=yes" user@host 'cmd'
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
    ServerAliveInterval 12
    StrictHostKeyChecking no
    UserKnownHostsFile=/dev/null
```


## docker
```bash
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
docker images | grep image | awk '{ print $3 }' | xargs docker rmi -f
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

## k8s
https://kubernetes.io/zh/docs/reference/kubectl/cheatsheet/
```bash
# pod
kubectl -n ns get deployment
kubectl -n ns exec -ti pod -- sh
kubectl -n ns cp etc pod:/directories/
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
```

## git
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

```
## springboot
```bash
java -jar -Dspring.profiles.active=activeprofilename app.jar
```
## maven
```bash
mvn clean package -e -U -X
mvn validate
mvn clean package -e -U -X -DskipTests
mvn clean package -e -U -DskipTests -ff -q

mvn dependency:tree
mvn dependency:list
mvn dependency:analyze
```
## redis
```bash
redis-server redis.conf
redis-cli -c -p port -h host
```

## sql
```sql
-- mysql date:          str_to_date('2000-01-01 00:00:00', '%Y-%m-%d %T')
-- oracle date:         to_date('2000-01-01 01:00:00', 'YYYY-MM-DD HH24:MI:SS')
-- oracle date default: to_date('23-JUL-19 05.42.32.871000000 AM', 'DD-MON-RRRR HH12.MI.SS.SSSSS AM')

select count(*), column1,column2 from tbname where column1=value1 and column2=value2 and column3 in (v1, v2, v3) and date1 between to_date('dstr', 'fstr') and to_date('dstr', 'fstr') group by column1, column2 having count(*) > 50 order by count(*);
-- mysql
SELECT 1 FROM table WHERE a = 1 AND b = 2 LIMIT 1
-- oracle
SELECT 1 FROM table WHERE a = 1 AND b = 2 and rownum < 2
```

## python
```python
```

## prometheus
```prometheus
# metric console query 
metric_name{label="value"}[15m] offset 5m
sum(metrics_name{label1="value1", label2="value2"})
sum(predict_linear(metric_name{label1="value1",label2="value2"}[20m], 3600*5))
sum(predict_linear(metric_name{label1="value1",label2="value2",label3="value3",label4="value4"}[1h],4*3600))<0
```
## emacs
### Emacs point (cursor) movement
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

Notice also that the commands are somewhat mnemonic:
* “f” stands for “forward”
* “b” stands for “backward”
* “n” stands for “next”
* “p” stands for “previous”
* “a” stands for “beginning” (like the beginning of the alphabet)
* “e” stands for “end”

## markdown
https://guides.github.com/features/mastering-markdown/