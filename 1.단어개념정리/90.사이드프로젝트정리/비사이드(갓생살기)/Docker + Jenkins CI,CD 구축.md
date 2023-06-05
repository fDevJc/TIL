# Docker + Jenkins CI,CD 구축

---

## 아키텍처

- Naver Cloud
- Git
- Docker
- Jenkins Container
- Service Container

---

## 1. Naver Cloud서버에 Docker 설치

👉 서버 OS 환경에 맞게 공식 문서를 참조하여 설치

[Install Docker Engine](https://docs.docker.com/engine/install/)

## 2. Naver Cloud서버에 Jenkins Container 설치 및 세팅

### 👉 2.1 Jenkins Image 받기

```
docker pull jenkins/jenkins:lts-jdk11
```

- jdk11 버전이 설치된 jenkins container 이미지 받기

### 👉 2.2 Jenkins Container 실행

```
docker run -d -p 8080:8080 -p 50000:50000 \
-v /home/jenkins:/var/jenkins \
-v /root/.ssh:/root/.ssh \
-v /var/run/docker.sock:/var/run/docker.sock \
-e TZ=Asia/Seoul \
--name jenkins-docker -u root jenkins/jenkins:lts-jdk11
```

- 호스트 포트(8080)과 컨테이너 포트(8080) 바운딩
    - 네이버클라우드 ACG 8080포트 허용
- jdk11 버전이 설치된 젠킨스 이미지를 사용하여 Jenkins Container 구동
- jenkins, .ssh, docker.sock 호스트와 볼륨 마운팅
- -e : 환경변수 시간 Asia/Seoul로 변경

### 👉 2.3 웹으로 Jenkins 접속

```
네이버 공인IP:8080 으로 Jenkins 접속
```

- 도커 로그에서 패스워드 확인
    
    ```
    docker logs jenkins-docker
    ```
    

### 👉 2.4 Jenkins Plugin 설치

- SSH Plugin
    
    ```
    jenkins 관리 -> 플러그인 관리
    SSH 검색 후
    - SSH agent 설치
    - Publish Over SSH 설치
    ```
    
    - 🤔 SSH agent 와 Public Over SSH 차이점이나 장단점에 대해 찾아봐야겠음..
    현재는 SSH agent 사용
- GIT Plugin
    
    ```
    - Github Integration 플러그인 설치
    ```
    
- Docker Plugin
    
    ```
    - Docker 플러그인 설치
    - Docker Pipeline 플러그인 설치
    ```
    

### 👉 2.5 Jenkins Container 안에 Docker CLI 설치

```
$ docker exec -it jenkins-docker /bin/bash
$ sudo apt-get install docker-ce-cli
```

- [https://docs.docker.com/engine/install/debian/](https://docs.docker.com/engine/install/debian/) 공식문서 참고하여 CLI만 설치
- Docker in docker 등 자료 참고

## 3. Jenkins Container, Github 연동

### 👉 3.1 Jenkins Container SSH PublicKey, PrivateKey 생성

```
ssh-keygen -t rsa

id_rsa (private key, 비밀키)
id_rsa.pub (pulbic key, 공개키)
생성됨
```

- 앞서 jenkins container를 구동할때 -v /root/.ssh:/root/.ssh 옵션을 이용하여 호스트와 볼륨을 공유하였기 때문에 호스트 OS에서 위의 명령어를 이용하여 public key 와 private key 생성

### 👉 3.2 Github SSH keys 등록

```
프로필 -> settings -> SSH and GPG keys -> new SSH key
-> Key 란에 생성한 공개키 입력
```

### 👉 3.3 Jenkins Git Credential 생성

```
Jenkins 관리 -> Manage Credentials -> Global -> Add Credentials
-> Kind: SSH Username with private key 선택 -> ID, Uername, PrivateKey 입력
```

- ID : Credetial 구분값
- Username : Git username
- PrivateKey : 생성한 PrivateKey 입력

## 4. Jenkins Container, Dockerhub 연동

### 👉 4.1 Docker Access Token 생성

```
Docker hub Account Settings -> Security -> New Access Token
```

### 👉 4.2 Jenkins Docker Credential 생성

```
Jenkins 관리 -> Manage Credentials -> Global -> Add Credentials
-> Kind: Username with password 선택 -> ID, Uername, Password 입력
```

- Password : 생성한 Docker Access Token 입력

## 5. Jenkins Container에서 호스트OS SSH 접근 설정

```
ssh-copy-id -i ~/.ssh/id_rsa.pub -p [ssh접속포트] root@[네이버클라우드서버IP]
```

- 현재 아키텍처가 하나의 서버에 여러 컨테이너를 두는 방식이라 Jenkins pipeline에서 호스트로 SSH 접속하여 docker 명령어를 통해 호스트에 서비스 이미지를 받아 컨테이너 실행 계획

## 6. 프로젝트 Dockerfile 작성

```
### build stage ###
FROM openjdk:11 AS builder

# set arg
ARG WORKSPACE=/home/spring-docker
ARG BUILD_TARGET=${WORKSPACE}/build/libs
WORKDIR ${WORKSPACE}

# copy code & build
COPY . .
RUN ./gradlew clean bootJar

# unpack jar
WORKDIR ${BUILD_TARGET}
RUN jar -xf *.jar

### create image stage ###
FROM openjdk:11

ARG WORKSPACE=/home/spring-docker
ARG BUILD_TARGET=${WORKSPACE}/build/libs
ARG DEPLOY_PATH=${WORKSPACE}/deploy

# copy from build stage
COPY --from=builder ${BUILD_TARGET}/org ${DEPLOY_PATH}/org
COPY --from=builder ${BUILD_TARGET}/BOOT-INF/lib ${DEPLOY_PATH}/BOOT-INF/lib
COPY --from=builder ${BUILD_TARGET}/META-INF ${DEPLOY_PATH}/META-INF
COPY --from=builder ${BUILD_TARGET}/BOOT-INF/classes ${DEPLOY_PATH}/BOOT-INF/classes

WORKDIR ${DEPLOY_PATH}

EXPOSE 8090/tcp
ENTRYPOINT ["java","org.springframework.boot.loader.JarLauncher"]
```

- 문법 공부 필요 일단 샘플로 구해서 도커 빌드 성공
- EXPOSE 8090/tcp
    - EXPOSE로 포트를 명시해도 결국 docker run에서 포트포워딩이 필요하다
    - [https://soft.plusblog.co.kr/139](https://soft.plusblog.co.kr/139)

## 7. Jenkins Pipeline 구성

```
pipeline {
    agent any
    environment {
        imagename = ""
        registryCredential = ''
        dockerImage = ''
    }
    stages {
        stage('git clone') {
            steps {
                echo 'Clonning Repository'
                git url: '',
                branch: 'master',
                credentialsId: ''
            }
            post {
                success {
                    echo 'Successfully Cloned Repository'
                }
                failure {
                    echo 'Git Clone fail'
                }
            }
        }
        
        stage('build gradle') {
            steps {
                echo 'Build Gradle'
                dir('.') {
                    sh './gradlew clean build'
                }
            }
            post {
                success {
                    echo 'Successfully Gradle Build'
                }
                failure {
                    echo 'Build fail'
                }
            }
        }
        
        stage('Build docker') {
            steps {
                echo 'Build Docker'
                script {
                    dockerImage = docker.build imagename
                }
            }
            post {
                success {
                    echo 'Successfully Docker Build'
                }
                failure {
                    echo 'Build fail'
                }
            }
        }
        
        stage('Push Docker') {
            steps {
                echo 'Push Docker'
                script {
                    docker.withRegistry( '', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
            post {
                success {
                    echo 'Successfully Docker Build'
                }
                failure {
                    echo 'Build fail'
                }
            }
        }
        
        stage('Docker Run') {
            steps {
                echo 'Pull Docker Image & Docker Image Run'
                sh "ssh -o StrictHostKeyChecking=no root@[서버아이피] -p [포트] 'docker run -d --name docker-sample -p 8090:8090 onezo/god-life-apigateway-service'"
            }
            post {
                success {
                    echo 'Successfully Docker Build'
                }
                failure {
                    echo 'Build fail'
                }
            }
        }
    }
}
```

- 문법 공부필요 여러 서비스 파이프라인구성하면서 공부하자