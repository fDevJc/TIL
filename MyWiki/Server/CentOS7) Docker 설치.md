# CentOS7) Docker 설치

---

## 1. yum-utils 설치

```java
sudo yum install -y yum-utils
```

## 2. repo 설정

```java
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

## 3. docker 설치

```java
sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

## Reference

[https://docs.docker.com/engine/install/centos/](https://docs.docker.com/engine/install/centos/)