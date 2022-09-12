# FaaS

## 자료모으기

---

## What

FaaS(Function-as-a-Service)는 개발자가 자체 인프라를 유지관리할 필요 없이 애플리케이션 패키지를 기능으로 빌드, 실행, 관리할 수 있게 해주는 일종의 클라우드 컴퓨팅 서비스

스테이트리스(stateless) 컨테이너에서 실행되는 이벤트 기반 컴퓨팅 실행 모델

FaaS는 서버리스 컴퓨팅을 구현하는 방식, 개발자가 비즈니스 로직을 작성하면 플랫폼이 관리를 전담하는 Linux 컨테이너에서 이를 실행

FaaS 솔루션 종류

- IBM Cloud Function
- AWS Lambda
- Google Cloud Functions
- Microsoft Azure Functions(오픈소스)
- OpenFaaS(오픈소스)

---

## Why

서버리스는 개발자의 서버 및 리소스 할당 관리 또는 프로비저닝과 같은 인프라 문제를 추상화하고 이를 플랫폼에 적용하므로 개발자는 코드 작성과 비즈니스 가치제공에 집중할 수 있다

FasS의 이점

- 개발자 생산성 향상 및 개발 시간 단축
- 서버 관리의 부담이 없음
- 손쉬운 확장 및 플랫폼에서 관리하는 수평적 스케일링
- 필요한 경우에만 리소스를 사용하거나 리소스 비용 지불
- 거의 모든 프로그래밍 언어로 기능 작성 가능

---

## How

FaaS 인프라는 주로 이벤트 기반 실행 모델을 통해 서비스 제공업체가 온디맨드로 미터링하는 것이 일반적

필요한 경우 사용 가능 하지만 서비스로서의 플랫폼(Platform-as-a-Service) 처럼 서버 프로세스를 백그라운드에서 계속 실행할 필요는 없다

현대적인 PaaS 솔루션은 개발자가 애플리케이션을 배포하는 데 사용할 수 있는 일반적인 워크플로우의 일부로 서버리스 기능을 제공하여, PaaS와 FaaS 간 구분이 모호해졌다

---

## Conclusion

---

## Reference
- [Red Hat - **서비스로서의 기능(FaaS)이란?**](https://www.redhat.com/ko/topics/cloud-native-apps/what-is-faas)