# ****CentOS7) SSH Keys 설정하는 방법, 예제, 명령어****

## **CentOS SSH 키 생성**

```bash
ls -l ~/.ssh/id_*.pub

ssh-keygen -t rsa -b 4096 -C "onezo11111@gmail.com"
```