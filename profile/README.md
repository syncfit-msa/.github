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

### 5.2. 개별 서비스 설정
각 서비스는 독립적으로 실행될 수 있도록 설정 파일과 실행 방법을 제공하고 있습니다.

- **User Service**: `application.yml`에서 DB 연결 정보를 설정한 후, `mvn spring-boot:run`으로 실행할 수 있습니다.
- **Auth Service**: `application.yml`에서 인증 서버 URL을 설정하고, 서비스 실행 후 JWT를 통한 인증을 처리합니다.
- **Recommend Service**: Gemini API와 Spotify API를 활용하여 음악 추천 기능을 제공합니다. API 키는 환경 변수로 설정합니다.

### 5.3. 실행 방법
- **전체 프로젝트 실행**: 모든 서비스를 로컬에서 실행하려면, Config-Service, Eureka, Api-gateway, 기타 서비스 순으로 실행합니다. 

## 🚩 6. API 명세서
각 서비스에서 제공하는 API는 다음 링크에서 확인할 수 있습니다. MSA 구조로 전환하는 과정에서 기존의 레거시 api와 달라진 부분이 있어 반영하여 작성하였습니다.

[API 명세서](https://www.notion.so/1c35145985de80bbb1bcd92391e07245)

## 🚩 7. 트러블슈팅 가이드
- **문제**: 문제
  - 해결법: 해결볍
- **문제**: 문제
  - 해결법: 해결법

## 🚩 8. 향후 개선 여지
- **문제**: 해결법
- **문제**: 해결법



