# CaringNote Back-end README

## 기술 스택

 ### 주요 프레임워크 및 언어

| 구성요소      | 사용 기술                 |
| ------------- | ------------------------- |
| Language      | Java 21                   |
| Build         | Gradle 8.10.2             |
| Framework     | Spring Boot 3.3.4         |
| ORM           | Spring Data JPA, QueryDSL |
| MVC           | Spring Web                |
| Test          | Junit5                    |
| AI            | Spring AI                 |
| API documents | Swagger                   |

### 데이터베이스

| 요소  | 사용 기술         |
| ----- | ----------------- |
| RDBMS | postgresql 16.4.0 |
| Cache | caffeine          |

### 외부 연동 API

| 요소         | 외부 API                                                     |
| ------------ | ------------------------------------------------------------ |
| SpeechToText | [naver cloud  clova speech api](https://www.ncloud.com/product/aiService/clovaSpeech) |
| TextAnalysis | openai api                                                   |



## 프로젝트 아키텍처

### 패키지 구조

* 도메인 별로 패키지 분리(DDD는 아님)
* 도메인 내에서 3 layer architecture로 구성

```sh
📦 com.springboot.api
├── common              
│   ├── config           # Spring Boot 실행 시 필요한 config(캐시, 시큐리티 스케쥴러 등)
│   ├── exception        # 사용자 예외 정의 및 핸들러
│   ├── aspect           # api 호출 시 Logging,권한 관련 apsect
│   ├── message          # 사용자 정의 응답/에러 메시지
│   ├── convertor        # Spring security,jpa 에서 사용되는 convertor(권한, json2String 등)
│   ├── util             # api 개발 시 유용한 util (file 처리, date 처리)
│   ├── validator        # request input 관련 검증
├── counselcard          # 상담 카드 도메인(케어링 노트에서 기초 상담 관련) ⭐️⭐️
│   ├── controller
│   ├── service
│   ├── repository
├── counselee            # 내담자 도메인(케어링 노트 내 상담 받는 사람 관련)⭐️⭐️
│   ├── controller
│   ├── service
│   ├── repository
├── counselor            # 상담사 도메인(케어링 노트 내 약사 관련) ⭐️
│   ├── controller
│   ├── service
│   ├── repository
├── counselsession       # 상담 도메인(복약 상담 관련)⭐️⭐️⭐️
│   ├── controller
│   ├── service
│   ├── eventlistener
├── medication           # 복약 정보 도메인(의약물 검색 관련) ⭐️⭐️
│   ├── controller
│   ├── service
│   ├── eventlistener
├── infra.external       # 외부 API 연동 ⭐️⭐️
└── enums                # 공통 enum 정의

```

### 인증 Process



### 케어링 노트 API 호출 Process







## 환경 설정

### 사전 설치 

* java21
* ffmpeg
  * audio file convert cli tool
  * 사용 목적
    * STT/TA 진행시, client 에서 들어오는 webm file은
      clova speech api에서 지원하지 않기 때문에 mp4로 변환할때 사용 
* postgresql 16.4(옵션)
  * Bare-metal 환경에서 실행 시 필요
* Docker-compose(옵션)
  * container 환경에서 실행 시 필요

### properties 사전 설정

* application-secret.yaml

  * 중요한 정보들은 application-secret.yaml 으로 분리
    실제 동작 시키기 위해서는 아래 정보 기입 필요

  ```yaml
  spring:
    jpa:
      properties:
        hibernate:
          dialect: org.hibernate.dialect.PostgreSQLDialect
          jdbc:
            time_zone: UTC
          ddl-auto: update
    datasource:
      url: <<database-url>>
      driver-class-name: org.postgresql.Driver
      username: <<database-username>>
      password: <<database-password>>
  
    ai:
      openai:
        api-key: <<open-url>>
  
  stt:
    file:
      path:
        origin: <<stt-audio-origin-file-upload-path>>
        convert:<<stt-audio-convert-file-upload-path>>
        
  ffmpeg:
    path: <<ffmpeg-bin-path>>
  
  naver:
    clova:
      api-key: <<naver-clova-api-keyz>>
  ```

### local 환경 실행 방법

* Bare-metal
  ```sh
  ## 사전 설치 및 설정 이후
  ./gradlew clean build
  ./gradlew bootRun
  ```

  

* Docker-compose
  ```sh
  docker-compose up --build -d
  ```

  



## 배포 방법

* gitAction



## 주요 서비스 

### 상담

 

### AI 요약



## API 정보

* https://caringnote.co.kr/api/swagger-ui/index.html#/



## 코드 작성 룰

### API 코드

### 테스트 코드
