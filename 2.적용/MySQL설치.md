### 1. yum repository 등록

```
yum install -y https://dev.mysql.com/get/mysql80-community-release-el7-5.noarch.rpm
```

```
Loaded plugins: fastestmirror, langpacks
mysql80-community-release-el7-5.noarch.rpm                                                                                                                    |  11 kB  00:00:00
Examining /var/tmp/yum-root-sVrq1y/mysql80-community-release-el7-5.noarch.rpm: mysql80-community-release-el7-5.noarch
Marking /var/tmp/yum-root-sVrq1y/mysql80-community-release-el7-5.noarch.rpm to be installed
Resolving Dependencies
--> Running transaction check
---> Package mysql80-community-release.noarch 0:el7-5 will be installed
--> Finished Dependency Resolution
base/7/x86_64                                                                                                                                                 | 3.6 kB  00:00:00
docker-ce-stable/7/x86_64                                                                                                                                     | 3.5 kB  00:00:00
extras/7/x86_64                                                                                                                                               | 2.9 kB  00:00:00
updates/7/x86_64                                                                                                                                              | 2.9 kB  00:00:00
updates/7/x86_64/primary_db                                                                                                                                   |  17 MB  00:00:02

Dependencies Resolved

=====================================================================================================================================================================================
 Package                                          Arch                          Version                         Repository                                                      Size
=====================================================================================================================================================================================
Installing:
 mysql80-community-release                        noarch                        el7-5                           /mysql80-community-release-el7-5.noarch                        9.1 k

Transaction Summary
=====================================================================================================================================================================================
Install  1 Package

Total size: 9.1 k
Installed size: 9.1 k
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : mysql80-community-release-el7-5.noarch                                                                                                                            1/1
  Verifying  : mysql80-community-release-el7-5.noarch                                                                                                                            1/1

Installed:
  mysql80-community-release.noarch 0:el7-5

Complete!
```

### 2. yum repository 등록 확인

```
yum repolist enabled | grep "mysql.*"
```

```
mysql-connectors-community/x86_64       MySQL Connectors Community           199
mysql-tools-community/x86_64            MySQL Tools Community                 92
mysql80-community/x86_64                MySQL 8.0 Community Server           346
```

### 3. MySQL 설치

```
yum install -y mysql-server
```

```
Total                                                                                                                                                8.3 MB/s |  75 MB  00:00:09
Retrieving key from file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql-2022
Importing GPG key 0x3A79BD29:
 Userid     : "MySQL Release Engineering <mysql-build@oss.oracle.com>"
 Fingerprint: 859b e8d7 c586 f538 430b 19c2 467b 942d 3a79 bd29
 Package    : mysql80-community-release-el7-5.noarch (@/mysql80-community-release-el7-5.noarch)
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-mysql-2022
Retrieving key from file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
Importing GPG key 0x5072E1F5:
 Userid     : "MySQL Release Engineering <mysql-build@oss.oracle.com>"
 Fingerprint: a4a9 4068 76fc bd3c 4567 70c8 8c71 8d3b 5072 e1f5
 Package    : mysql80-community-release-el7-5.noarch (@/mysql80-community-release-el7-5.noarch)
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : mysql-community-client-plugins-8.0.30-1.el7.x86_64                                                                                                                1/8
  Installing : mysql-community-common-8.0.30-1.el7.x86_64                                                                                                                        2/8
  Installing : mysql-community-libs-8.0.30-1.el7.x86_64                                                                                                                          3/8
  Installing : mysql-community-client-8.0.30-1.el7.x86_64                                                                                                                        4/8
  Installing : mysql-community-icu-data-files-8.0.30-1.el7.x86_64                                                                                                                5/8
  Installing : mysql-community-server-8.0.30-1.el7.x86_64                                                                                                                        6/8
  Installing : mysql-community-libs-compat-8.0.30-1.el7.x86_64                                                                                                                   7/8
  Erasing    : 1:mariadb-libs-5.5.65-1.el7.x86_64                                                                                                                                8/8
  Verifying  : mysql-community-common-8.0.30-1.el7.x86_64                                                                                                                        1/8
  Verifying  : mysql-community-client-plugins-8.0.30-1.el7.x86_64                                                                                                                2/8
  Verifying  : mysql-community-icu-data-files-8.0.30-1.el7.x86_64                                                                                                                3/8
  Verifying  : mysql-community-libs-8.0.30-1.el7.x86_64                                                                                                                          4/8
  Verifying  : mysql-community-client-8.0.30-1.el7.x86_64                                                                                                                        5/8
  Verifying  : mysql-community-libs-compat-8.0.30-1.el7.x86_64                                                                                                                   6/8
  Verifying  : mysql-community-server-8.0.30-1.el7.x86_64                                                                                                                        7/8
  Verifying  : 1:mariadb-libs-5.5.65-1.el7.x86_64                                                                                                                                8/8

Installed:
  mysql-community-libs.x86_64 0:8.0.30-1.el7               mysql-community-libs-compat.x86_64 0:8.0.30-1.el7               mysql-community-server.x86_64 0:8.0.30-1.el7

Dependency Installed:
  mysql-community-client.x86_64 0:8.0.30-1.el7                  mysql-community-client-plugins.x86_64 0:8.0.30-1.el7          mysql-community-common.x86_64 0:8.0.30-1.el7
  mysql-community-icu-data-files.x86_64 0:8.0.30-1.el7

Replaced:
  mariadb-libs.x86_64 1:5.5.65-1.el7

Complete!
```

### 4. MySQL 버전확인
```
mysqld -V
```

```
/usr/sbin/mysqld  Ver 8.0.30 for Linux on x86_64 (MySQL Community Server - GPL)
```

### REFERENCE
https://zero-gravity.tistory.com/338