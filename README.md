# cheatsheets
cheatsheets

## bash & linux
```bash
ls -atlrhd name*tag
# curl with user:pwd
curl -u username:password http://example.com
# curl upload file
curl -i -X PUT -H "Content-Type: multipart/form-data" -F "apivarname=@locfilepath" -vvv http://localhost:8080/api/v1/method

# ssh usage
sshpass -e scp -J jumphost1,jumphost2 -F config -q -o "UserKnownHostsFile=/dev/null" -o "StrictHostKeyChecking=no" -o "ForwardAgent=yes" deployment.yaml user@host:/directories/

sshpass -f filepath ssh -J jumphost1,jumphost2 -F config -q -o "UserKnownHostsFile=/dev/null" -o "StrictHostKeyChecking=no" -o "ForwardAgent=yes" user@host 'cmd'
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
docker run --rm -it --entrypoint=/bin/sh progrium/busybox
docker run --rm -it --entrypoint=/bin/sh alpine:3.11.6

# docker build
docker build -t image:tag -f Dockerfile .

# docker save
docker save image:tag -o image-tag.tgz

# docker-compose
docker-compose -f docker-compose.yaml up -d 
docker-compose -f docker-compose.yaml down

```

## k8s
https://kubernetes.io/zh/docs/reference/kubectl/cheatsheet/
```bash
# pod
kubectl -n ns get deployment
kubectl -n ns exec -ti pod -- sh
kubectl -n ns cp etc pod:/directories/
kubectl -n ns logs -f pod
kubectl -n ns rollout restart -f deployment.yaml
kubectl -n ns delete pod pdname --grace-period=0 --force
# service
kubectl -n ns get svc
kubectl -n ns delete service 

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

mvn dependency:tree
mvn dependency:list
mvn dependency:analyze
```
## redis
```bash
redis-server redis.conf
redis-cli -c -p port -h host
```

## markdown
https://guides.github.com/features/mastering-markdown/