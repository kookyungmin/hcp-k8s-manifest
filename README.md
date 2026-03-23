# Happy Cloud Platform Manifests

<!-- prettier-ignore-start -->
![k8s](https://shields.io/badge/k8s-black?logo=kubernetes&style=for-the-badge%22)
![github actions](https://shields.io/badge/github_actions-black?logo=githubactions&style=for-the-badge%22)
![argocd](https://shields.io/badge/argocd-black?logo=argo&style=for-the-badge%22)

## 해피 클라우드 플랫폼 배포 과정

![img.png](img.png)

GitHub Actions

1. Spring Boot 서비스별 이미지 빌드
2. 레지스트리에 push
3. manifest 저장소의 이미지 태그만 업데이트해서 commit

Argo CD
4. manifest 저장소 변화를 감지
5. Kubernetes에 자동 sync 해서 배포

## 사전 환경 구축

### redis

```
$ kubectl apply -k bases/redis
```

### mysql

```
$ kubectl apply -k bases/mysql
```


* DB SQL 초기화

```
각 서비스 sql/init.sql 실행
```

### kafka

```
$ kubectl apply -k bases/kafka
```

* kafka topic 생성

```
/opt/kafka/bin/kafka-topics.sh \
  --bootstrap-server kafka:9092 \
  --create \
  --if-not-exists \
  --topic hcp.compute.instance.provisioning \
  --partitions 3 \
  --replication-factor 1;
  
/opt/kafka/bin/kafka-topics.sh \
  --bootstrap-server kafka:9092 \
  --create \
  --if-not-exists \
  --topic hcp.compute.instance.provisioning.DLT \
  --partitions 3 \
  --replication-factor 1;
  
/opt/kafka/bin/kafka-topics.sh \
  --bootstrap-server kafka:9092 \
  --create \
  --if-not-exists \
  --topic hcp.compute.instance.status \
  --partitions 3 \
  --replication-factor 1;
  
/opt/kafka/bin/kafka-topics.sh \
  --bootstrap-server kafka:9092 \
  --create \
  --if-not-exists \
  --topic hcp.compute.instance.update.lifecycle \
  --partitions 3 \
  --replication-factor 1;
  
/opt/kafka/bin/kafka-topics.sh \
  --bootstrap-server kafka:9092 \
  --create \
  --if-not-exists \
  --topic hcp.compute.instance.scaling \
  --partitions 3 \
  --replication-factor 1;
  
/opt/kafka/bin/kafka-topics.sh \
  --bootstrap-server kafka:9092 \
  --create \
  --if-not-exists \
  --topic hcp.compute.instance.register.sshkey \
  --partitions 3 \
  --replication-factor 1;
  
/opt/kafka/bin/kafka-topics.sh \
  --bootstrap-server kafka:9092 \
  --create \
  --if-not-exists \
  --topic hcp.compute.instance.update.networkpolicy \
  --partitions 3 \
  --replication-factor 1;
```

### AKHQ (kafka-ui)

```
$ kubectl apply -k bases/kafka-ui
$ minikube service -n hcp hcp-kafka-ui-svc
```






