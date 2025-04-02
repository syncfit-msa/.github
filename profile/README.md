#  Legacy Application의 MSA 전환 프로젝트


## 🚩 1. 프로젝트 개요

본 프로젝트는 기존 레거시 시스템을 마이크로서비스 아키텍처(MSA)로 전환 및 클라우드화 하여, 운영 효율성을 높이고 서비스 확장성을 극대화하는 것입니다. 단일 애플리케이션을 여러 개의 독립적인 서비스로 분리함으로써 각 팀은 독립적으로 개발하고 배포할 수 있으며, 부분적인 장애가 전체 시스템에 영향을 미치는 위험을 줄여 서비스의 안정성을 향상시킬 수 있습니다.

- 전환 프로젝트 : 음악 추천 서비스 Syncfit [소스코드](https://)

- 프로젝트 개발 기간: 2025.03.27 - 2025.04.02 (7일)

- Notion: [프로젝트 노션 링크](https://www.notion.so/InspireCamp-10-1c35145985de80e39edef7eff44dda3d)

## 🚩 2. 기존 서비스 소개 및 전환 목표
- 서비스 소개
Syncfit은 사용자가 감정을 입력하면 노래를 추천 해주는 서비스입니다. 입력한 장르에서 gemini api를 이용하여 추천 장르를 추출하고 이를 다시 spotify api에 입력하여 장르에 해당하는 곡을 반환해줍니다. 위시리스트(플레이리스트) 기능을 제공하여, 사용자가 추천 받은 곡을 위시리스트에 담을 수 있습니다. 위시리스트의 대표 이미지 변경 기능도 함께 제공합니다.

- 기존 서비스의 특징 및 전환 목표
   ![image](https://github.com/user-attachments/assets/2523770a-bd97-4c67-8093-7a85359f7f9d)

기존의 Syncfit 서비스는 도메인을 기준으로 분리된 디렉토리 구조를 가지고 있으며 각 도메인은 MVC 패턴으로 구성되어 있습니다. 기존 10개의 도메인 중 비지니스 로직에 맞춰 통합 가능한 서비스끼리 묶어 도메인을 재구성하였습니다. 또한 모놀리식 구조로 설계되어 있어 장애 발생시 모든 서비스 에러 발생할 수 있다는 한계가 있습니다.

레거시 서비스를 MSA로 전환함에 따라 설정한 목적은 다음과 같습니다.
- 확장성: 각 서비스를 필요에 따라 개별적으로 확장할 수 있으므로 자원 활용이 최적화되고 고부하 상황에서도 유연하게 대응할 수 있도록 합니다.
- 빠른 요구사항 대응: MSA 전환을 통해 더 빈번한 기능 배포와 빠른 대응이 가능해져 비즈니스 요구사항 변화에 민첩하게 대응할 수 있도록 합니다.
- 배포 자동화: 작은 단위의 변경 사항을 서비스별로 배포할 수 있으므로 지속적인 배포(Continuous Delivery) 문화가 정착되고, CI/CD 파이프라인을 통한 자동화로 개발부터 배포까지의 사이클 타임이 단축합니다.

결과적으로 본 프로젝트는 확장성(Scalability), 민첩성(Agility), 그리고 운영의 효율성(Efficiency)을 향상시켜 향후 서비스 증가와 트래픽 상승에도 대응할 수 있는 탄탄한 기반을 마련하는 것을 목적으로 합니다.


## 🚩 3. 서비스 구조
![image](https://github.com/user-attachments/assets/45ac807e-2853-4757-8cc7-221251c2a333)



## 🚩 4. 서비스 목록
본 프로젝트에서는 마이크로서비스 아키텍처로 전환하는 과정에서 다음과 같은 서비스들이 구축되었습니다. 
서비스는 서비스가 담당하는 특정 기능이나 역할을 기준으로 분리하였습니다.

- [User Service](https://github.com/syncfit-msa/user-service/blob/develop/README.md): 사용자 관련 서비스

- [Auth Service](https://github.com/syncfit-msa/auth-service/blob/develop/README.md): 사용자 인증 및 권한 관리 서비스
 
- [Wishlist Service](https://github.com/syncfit-msa/wishlist-service/blob/wish/sep/README.md): 사용자 맞춤형 위시리스트 관리 서비스

- [Track Service](https://github.com/syncfit-msa/track-service/blob/develop/README.md): 트랙 정보 관리 서비스

- [YouTube Client](https://github.com/syncfit-msa/youtube-service/blob/develop/README.md): 영상 검색 및 연동 서비스

- [Recommend Service](https://github.com/syncfit-msa/recommend-service/blob/develop/README.md): gemini api 와 spoitfy api를 활용한 음악 추천

## 🚩 5. 기술적 요구 사항 및 통신 방식
- Eureka(Service-Discovery)
  
서비스 디스커버리(Service Discovery) 메커니즘을 구축하여 동적으로 서비스들의 위치를 탐색하고 연결합니다. Eureka와 같은 서비스 레지스트리를 도입하여 모든 마이크로서비스 인스턴스가 레지스트리에 자신의 주소를 등록하고, 다른 서비스들은 해당 레지스트리를 통해 서비스 주소를 조회할 수 있습니다. 이를 통해 IP나 포트가 변동되는 동적 환경에서도 안정적으로 서비스 통신이 가능해지고, 수평 확장된 여러 인스턴스에 대한 부하 분산도 용이합니다.

  
- API Gateway

  
클라이언트의 모든 요청을 단일 진입점(single entry point)에서 받아 처리합니다. Spring Cloud Gateway를 사용하여 외부 요청을 인증/인가하고 적절한 내부 마이크로서비스로 라우팅합니다. API 게이트웨이는 게이트웨이 레벨에서 인증, 인가, 로깅, 로드 밸런싱 등을 수행하여 각 마이크로서비스의 부담을 줄이고 Cross-cutting Concern를 중앙에서 처리
보안과 모니터링을 강화하고, 추후 BFF(Backends for Frontends) 패턴 등도 용이하게 적용 가능합니

- 구성 관리 및 Spring Cloud Bus 활용
  
Spring Cloud Config 서버를 도입하여 모든 서비스의 설정을 중앙 저장소에서 관리하고, Spring Cloud Bus를 통해 구성 변경 사항을 실시간으로 각 서비스에 Broadcast합니다. Spring Cloud Bus는 경량 메시지 브로커(RabbitMQ )를 통해 분산된 각 노드들을 연결하고, 구성 변경 등의 상태 변화를 브로드캐스트하는 역할을 합니다. RabbitMQ를 Spring Cloud Bus의 underlying broker로 활용하여 /actuator/busrefresh 수행 시 모든 서비스의 설정을 즉시 갱신하였습니다.

- Feign

마이크로서비스들 간의 HTTP 호출을 간편하게 처리하기 위해 Feign을 사용했습니다. 각 서비스는 Feign을 통해 다른 서비스와의 HTTP 호출을 최소화하고, 이를 통해 코드의 간결함과 유지보수성을 높였습니다.

- CI/CD 파이프라인 구축
  
Jenkins를 활용하여 지속적 통합 및 배포(CI/CD) 파이프라인을 구축하였습니다. 코드가 변경되면 Jenkins가 자동으로 빌드, 테스트, Docker 이미지화, 그리고 스테이징/프로덕션 환경으로의 배포까지 수행하도록 파이프라인 설계하였습니다. GitHub 등 소스 저장소와 연동하여 코드 푸시 🡪 빌드 🡪 테스트 🡪 Docker 이미지 푸시(ECR 등) 🡪 배포 단계 자동화하였습니다.

- AWS 클라우드 인프라 활용
  
배포 환경으로 AWS 클라우드를 활용하여 탄력적이고 안정적인 인프라를 구성하였습니다. 마이크로서비스는 AWS EC2 인스턴스 (또는 컨테이너 오케스트레이션을 위해 AWS ECS) 위에 Docker 컨테이너로 배포하였습니다. Jenkins 서버를 구성하고 보안 그룹으로 접근 제한하였습니다. 컨테이너 이미지는 AWS ECR에 저장하고, 배포 시 해당 이미지를 가져와 배포하는 전략 사용하였습니다.

## 🚩 6. 서비스별 설정 방법

각 서비스에 대한 구체적인 설정 및 실행 방법은 아래에 서술되어 있습니다.

### 5.1. 공통 설정
- **환경 변수**: 각 서비스는 `.env` 파일을 통해 환경 변수를 설정하였습니다. 데이터베이스 URL, RabbitMQ 포트, 인증 관련 키 등을 환경변수로 설정하여 안전하게 관리하였습니다. 

### 5.2. 설정 정보
각 서비스는 독립적으로 실행될 수 있도록 설정 파일을 가지고 있습니다. **User Service**를 예시로 설명하겠습니다.

#### 5.2.2 application.yml
`application.yml` 파일은 주로 애플리케이션의 설정을 정의하는 곳으로, 여러 설정을 포함할 수 있습니다. `spring.profiles.active`에 따라 활성화될 프로파일을 지정하고, 여러 모듈이나 설정을 포함할 수 있도록 설계됩니다.

```yaml
spring:
  config:
    import: optional:configserver:http://${CONFIG_HOST:127.0.0.1}:${CONFIG_PORT:8888}/
  cloud:
    config:
      name: user-service  # Config Server에서 가져올 설정 이름
  application:
    name: user-service  # 애플리케이션 이름 (서비스 이름)
  profiles:
    active: local  # 활성화될 프로파일 (local, dev, prod 등)
    include:
      - redis  # Redis 설정 포함
      - datasource  # 데이터 소스 설정 포함
      - security  # 보안 관련 설정 포함
      - openfeign  # OpenFeign 관련 설정 포함
      - eureka  # Eureka 관련 설정 포함
      - actuator  # Actuator 관련 설정 포함
      - internal  # 내부 설정 포함
```

- spring.config.import: Config Server에서 설정 정보를 가져오는 부분입니다. 이 설정은 CONFIG_HOST와 CONFIG_PORT 환경 변수로 Config Server의 URL을 동적으로 설정합니다. Config Server를 통해 공통 설정을 가져오며, optional을 사용하여 서버가 없을 경우에도 실패하지 않도록 처리합니다.

- spring.cloud.config.name: 설정 서버에서 가져올 설정의 이름을 지정합니다. 여기서 user-service는 해당 애플리케이션의 설정을 의미합니다.

- spring.profiles.active: 활성화할 프로파일을 지정합니다. 로컬 환경에서는 local, 운영 환경에서는 prod 등을 지정할 수 있습니다. 이 값에 따라 설정 파일이 달라집니다.

- include: 여러 설정 파일을 포함시킬 수 있습니다. redis, datasource, security 등을 포함시켜 필요에 따라 설정을 분리하여 관리할 수 있습니다.

#### 5.2.2 bootstrap.yml
bootstrap.yml 파일은 Spring Cloud 환경에서 중요한 역할을 하며, 주로 애플리케이션이 시작될 때 가장 먼저 로드되는 설정 파일입니다. 이 파일에서 Config Server와의 연결 설정을 포함시켜 초기 설정을 로드합니다.

```
spring:
  application:
    name: user-service  # 애플리케이션 이름 (서비스 이름)
  cloud:
    config:
      uri: http://${CONFIG_HOST:127.0.0.1}:${CONFIG_PORT:8888}  # Config Server의 URI 설정
  rabbitmq:
    host: ${RABBITMQ_HOST:localhost}  # RabbitMQ 서버 호스트 (기본값: localhost)
    port: ${RABBITMQ_PORT:5672}  # RabbitMQ 서버 포트 (기본값: 5672)
    username: ${RABBITMQ_USERNAME:guest}  # RabbitMQ 사용자 이름 (기본값: guest)
    password: ${RABBITMQ_PASSWORD:guest}  # RabbitMQ 사용자 비밀번호 (기본값: guest)
```
- spring.application.name: 애플리케이션의 이름을 지정합니다. 이 이름은 Config Server에서 설정을 가져오는 데 사용됩니다.

- spring.cloud.config.uri: Config Server의 URI를 설정합니다. 이 값은 Config Server에서 설정 정보를 가져오기 위한 URL입니다. CONFIG_HOST와 CONFIG_PORT 환경 변수로 동적으로 구성되며, 기본적으로 http://127.0.0.1:8888을 사용합니다.

- rabbitmq: RabbitMQ와의 연결을 위한 설정입니다. 메시지 큐를 사용하기 위해 RabbitMQ 서버의 호스트, 포트, 사용자 이름, 비밀번호를 설정합니다. 기본값을 지정하여, 환경 변수가 없을 경우 기본값을 사용하도록 설정할 수 있습니다.

#### 5.2.3 Config Server에서 설정 정보 가져오기
- Spring Cloud의 Config Server는 중앙 집중식 설정 서버로, 여러 마이크로서비스에서 공유할 설정을 외부 파일로 관리하고 이를 애플리케이션에서 쉽게 사용할 수 있게 합니다. application.yml 및 bootstrap.yml에서 설정한대로, Config Server에서 애플리케이션의 설정 정보를 가져옵니다.

- Config Server URI: bootstrap.yml의 spring.cloud.config.uri 항목에서 설정한 URI를 통해, 애플리케이션은 Config Server에서 설정 파일을 가져옵니다.

- name에 따른 설정 정보 적용용: application.yml의 spring.cloud.config.name 항목에서 정의된 값에 해당하는 설정을 Config Server에서 로드합니다. 예를 들어, user-service라는 이름을 사용하면, Config Server는 user-service.yml 설정 파일을 로드하여 애플리케이션에 적용합니다.


### 5.3. 실행 방법
- -서비스를 로컬에서 실행하려면, Config-Service, Eureka, Api-gateway, 기타 서비스 순으로 실행합니다. 

## 🚩 7. 인증 방식
![image](https://github.com/user-attachments/assets/8dc35797-96db-40f0-bce3-ea5273557c81)

- 인증
API Gateway + OAuth + Redis를 활용하여 인증 구조를 구현하였습니다. API Gateway에서 JWT 검증을 수행하고, OAuth를 통해 사용자 인증을 처리하며, Redis를 이용해 Refresh Token을 관리합니다.

- 아키텍처 개요
```
[Client] → [API Gateway (JWT 검증)] → [각 서비스]
                                     ↘
                          [User-Service (OAuth 인증, Redis)]
```
- 인증 흐름

1️⃣ 로그인 & 토큰 발급 (User-Service)

1. 사용자가 로그인 요청 (/login)

2. User-Service가 ID/PW 검증 후 Access Token & Refresh Token 발급

3. Refresh Token은 Redis에 저장하여 관리

4. 클라이언트에게 Access Token & Refresh Token 반환

2️⃣ API 요청 처리 (API Gateway)

1. 클라이언트가 API 요청 시 **Bearer <Access Token>**을 포함

2. API Gateway가 JWT 유효성 검사 수행

3. 유효하면 X-Member-Id 헤더 추가 후 각 서비스에 전달

3️⃣ Access Token 만료 시 Refresh Token 사용

1. Access Token이 만료되면 클라이언트가 /refresh 요청

2. User-Service에서 Redis에 저장된 Refresh Token 검증

3. 새로운 Access Token 발급 후 클라이언트에 반환

- MSA 전환의 이점
  
✅ API Gateway에서 JWT 검증을 수행하여 개별 서비스의 인증 부담을 줄입니다.

✅ OAuth + Redis를 활용하여 보안성과 성능을 향상시킵니다.

✅ MSA 친화적인 구조로 확장성과 유지보수성이 뛰어난 인증 시스템을 제공합니다.

## 🚩 8. API 명세서
각 서비스에서 제공하는 API는 다음 링크에서 확인할 수 있습니다. MSA 구조로 전환하는 과정에서 기존의 레거시 api와 달라진 부분이 있어 반영하여 작성하였습니다.

[API 명세서](https://www.notion.so/1c35145985de80bbb1bcd92391e07245)

## 🚩 9. 트러블슈팅
- **문제**: 문제
  - 해결법: 해결볍
- **문제**: 문제
  - 해결법: 해결법

## 🚩 10. 향후 개선 여지
- **문제**: 해결법
- **문제**: 해결법



