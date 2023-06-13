# CentOS MySQL외부접속허용 방화벽설정

- MySQL 포트 등록
    
    ```bash
    firewall-cmd --zone=public --add-port=3306/tcp --permanent
    ```
    
- 방화벽 재실행
    
    ```bash
    firewall-cmd --reload
    ```
    
- 방화벽 목록확인
    
    ```bash
    firewall-cmd --list-ports
    ```
    
- default ZONE 확인
    
    ```bash
    /etc/firewalld/firewalld.conf
    
    DefaultZone=public
    ```
    
- ZONE 설정 파일 확인