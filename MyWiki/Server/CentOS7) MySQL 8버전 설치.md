# CentOS7) MySQL 8버전 설치

---

## 1. MySQL 다운로드 링크 확인

[https://www.mysql.com/products/community/](https://www.mysql.com/products/community/)

```java
https://dev.mysql.com/get/mysql80-community-release-el7-7.noarch.rpm
```

## 2. MySQL Repository install

```java
yum install -y https://dev.mysql.com/get/mysql80-community-release-el7-7.noarch.rpm
```

## 3. yum repository list 확인

```java
yum repolist enabled | grep "mysql.*"
```

## 4. MySQL Server install

```java
yum install -y mysql-server
```

## 5. MySQL 설치 버전 확인

```java
mysqld -V
```

## Reference

[https://zero-gravity.tistory.com/338](https://zero-gravity.tistory.com/338)