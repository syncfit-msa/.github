#  Legacy Application의 MSA 전환 프로젝트


## 🚩 1. 프로젝트 개요

본 프로젝트의 목표는 기존 레거시 시스템을 마이크로서비스 아키텍처(MSA)로 전환 및 클라우드화 하여, 운영 효율성을 높이고 서비스 확장성을 극대화하는 것입니다. 단일 애플리케이션을 여러 개의 독립적인 서비스로 분리함으로써 각 팀은 독립적으로 개발하고 배포할 수 있으며, 부분적인 장애가 전체 시스템에 영향을 미치는 위험을 줄여 서비스의 안정성을 향상시킬 수 있습니다.

- 전환 프로젝트 : 음악 추천 서비스, Syncfit [소스코드](https://)

- 프로젝트 개발 기간: 2025.03.27 - 2025.04.02 (7일)

- Notion: [프로젝트 노션 링크](https://www.notion.so/InspireCamp-10-1c35145985de80e39edef7eff44dda3d)

## 🚩 2. 서비스 구조
![image](https://github.com/user-attachments/assets/45ac807e-2853-4757-8cc7-221251c2a333)



## 🚩 3. 서비스 목록
본 프로젝트에서는 마이크로서비스 아키텍처로 전환하는 과정에서 다음과 같은 서비스들이 구축되었습니다. 
서비스는 서비스가 담당하는 특정 기능이나 역할을 기준으로 분리하였습니다.

- [User Service](https://github.com/syncfit-msa/user-service/blob/develop/README.md): 사용자 관련 서비스

- Auth Service 사용자 인증 및 권한 관리 서비스
 
- [Wishlist Service](https://github.com/syncfit-msa/wishlist-service/blob/wish/sep/README.md): 사용자 맞춤형 위시리스트 관리 서비스

- [Track Service](https://github.com/syncfit-msa/track-service/blob/develop/README.md): 트랙 정보 관리 서비스

- YouTube Client: YouTube: 영상 검색 및 연동 서비스

- [Recommend Service](https://github.com/syncfit-msa/recommend-service/blob/develop/README.md): gemini api 와 spoitfy api를 활용한 음악 추천

## 🚩 4. 기술적 요구 사항 및 통신 방식
- Eureka: 각 마이크로서비스가 실행될 때 자신을 Eureka 서버에 등록하고, 다른 서비스들이 자신을 Eureka 서버에서 검색하여 통신할 수 있게 합니다.
  
- API Gateway: 클라이언트의 요청을 적절한 마이크로서비스로 라우팅하는 역할을 합니다. 클라이언트는 API Gateway를 통해 모든 마이크로서비스와 통신하게 되며 이로 인해 클라이언트가 여러 서비스들을 직접 호출할 필요가 없어집니다.
  
- Spring Cloud: 애플리케이션 설정을 한 곳에서 관리하여 운영 효율성을 높였습니다.

- RabbitMQ: 서비스 간 비동기 메시징을 처리하기 위해 RabbitMQ를 사용하여 큐 기반 메시징을 구현했습니다. 서비스 간의 데이터 전달과 이벤트 처리를 비동기적으로 처리하고, 트래픽이 많거나 처리 시간이 긴 작업을 효율적으로 분배하도록 하였습니다.

- Feign: 마이크로서비스들 간의 HTTP 호출을 간편하게 처리하기 위해 Feign을 사용했습니다. 각 서비스는 Feign을 통해 다른 서비스와의 HTTP 호출을 최소화하고, 이를 통해 코드의 간결함과 유지보수성을 높였습니다.


## 🚩 5. 서비스별 설정 방법

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

## 🚩 6. API 명세서
각 서비스에서 제공하는 API는 다음 링크에서 확인할 수 있습니다. MSA 구조로 전환하는 과정에서 기존의 레거시 api와 달라진 부분이 있어 반영하여 작성하였습니다.

[API 명세서](https://www.notion.so/1c35145985de80bbb1bcd92391e07245)

## 🚩 7. 트러블슈팅
- **문제**: 문제
  - 해결법: 해결볍
- **문제**: 문제
  - 해결법: 해결법

## 🚩 8. 향후 개선 여지
- **문제**: 해결법
- **문제**: 해결법



