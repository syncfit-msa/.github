#  Legacy Application의 MSA 전환 프로젝트


## 🚩 1. 프로젝트 개요

본 프로젝트의 목표는 기존 레거시 시스템을 마이크로서비스 아키텍처(MSA)로 전환하여, 운영 효율성을 높이고 서비스 확장성을 극대화하는 것입니다. 단일 애플리케이션을 여러 개의 독립적인 서비스로 분리함으로써 각 팀은 독립적으로 개발하고 배포할 수 있으며, 부분적인 장애가 전체 시스템에 영향을 미치는 위험을 줄여 서비스의 안정성을 향상시킬 수 있습니다.

- 전환 프로젝트 : 음악 추천 서비스, Syncfit [소스코드](https://)

- 프로젝트 개발 기간: 2025.03.27 - 2025.04.02 (7일)

- Notion: [프로젝트 노션 링크](https://www.notion.so/InspireCamp-10-1c35145985de80e39edef7eff44dda3d)

## 🚩 2. 서비스 구조
![image](https://github.com/user-attachments/assets/c8104e7b-cb6c-483f-82eb-ef2bc94e4c92)

![image](https://github.com/user-attachments/assets/0a50f40e-b52a-4142-8470-40c3f1ba14d8)


## 🚩 3. 서비스 목록
본 프로젝트에서는 마이크로서비스 아키텍처로 전환하는 과정에서 다음과 같은 서비스들이 새롭게 구축되었습니다.
- Api-GateWay: 클라이언트 요청을 적절한 마이크로서비스로 라우팅하는 게이트웨이
  
- Eureka: 마이크로서비스 간 서비스 등록과 발견을 지원하는 서버
  
- Config-Server: 중앙에서 애플리케이션 설정을 관리하는 서버.


- User Service: 사용자 관련 서비스

- Auth Service 사용자 인증 및 권한 관리 서비스
 
- Wishlist Service: 사용자 맞춤형 위시리스트 관리 서비스

- Track Service: 트랙 정보 관리 서비스

- YouTube Client: YouTube: 영상 검색 및 연동 서비스

- Recommend Service: gemini api 와 spoitfy api를 활용한 음악 추천
