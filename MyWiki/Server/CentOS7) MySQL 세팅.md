# CentOS7) MySQL 세팅

---

### 👉 버전 확인

```java
mysql --version
mysqld -V
```

### 👉 서버 실행

```java
systemctl enable mysqld && systemctl start mysqld && systemctl status mysqld
```

### 👉 서버실행시 패스워드 확인

```java
grep 'temporary password' /var/log/mysqld.log
ezA>ft1AWIur
```

### 👉 서버 접속

```java
mysql -u root -p
```

### 👉 서버 루트 패스워드 변경

```java
ALTER USER 'root'@'localhost' IDENTIFIED BY 'Onezo12#';
```

### 👉 데이터 베이스 생성

```java
CREATE DATABASE db_live_god_life default CHARACTER SET UTF8;
```

### 👉 데이터 베이스 삭제

```java
DROP DATABASE <데이터베이스명>;
```

### 👉 데이터 베이스 리스트 조회

```java
SHOW DATABASES;
```

### 👉 사용자 추가

```java
create user live_god_life@localhost identified by 'Onezo12#';
create user live_god_life@'%' identified by 'Onezo12#';
```

- 뒤에 호스트는 IP접속 허용이랑 연관있음.

### 👉 사용자 리스트 확인

```java
use mysql;
select host,user,password from user;
```

### 👉 사용자 권한 부여

```java
GRANT select,insert,update,delete ON db_live_god_life.* to live_god_life@'%';

GRANT select,insert,update,delete,create,alter,drop ON db_live_god_life.* to live_god_life@'%';
```

### 👉 사용자 IP 접속권한

```java
모든 IP 허용
GRANT ALL ON *.* TO live_god_life@'%';
```

### 👉 변경 사항 적용

```java
FLUSH PRIVILEGES;
```

### 👉 PORT 확인

```java
SHOW GLOBAL VARIABLES LIKE 'PORT';
```

---

세팅 에러

### 👻 Public Key Retrieval is not allowed

- DBeaver 설정변경
- [https://deviscreen.tistory.com/85](https://deviscreen.tistory.com/85)

### 👻 ****You are not allowed to create a user with GRANT****

- [https://dzzienki.tistory.com/22](https://dzzienki.tistory.com/22)

### 👻 Host 'IP' is not allowed to connect to this MySQL server”

- [https://galid1.tistory.com/349](https://galid1.tistory.com/349)