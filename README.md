# CS2-ZephyR-Server

> CS2 내전 커뮤니티 ZephyR 서버, 스팀 로그인·스킨 설정 (Spring Boot · MongoDB)

ZephyR는 2024년에 만든 Counter-Strike 2 내전용 커뮤니티 서버다. 이 저장소는 웹에서 유저 정보와 스킨 설정을 다루는 API 서버로, 같은 MongoDB를 게임 서버 플러그인과 디스코드 봇이 함께 읽는다.

## 주요 기능
- 스팀 ID로 유저 조회. 처음 들어온 유저는 Steam Web API로 닉네임·아바타를 받아 가입 대기 상태로 저장하고, 디스코드 연동 전까지 401을 돌려준다
- 스킨 설정 저장: 무기 스킨(페인트·시드·마모도·이름), 칼, 장갑, 에이전트 모델(CT·T), 뮤직 키트, 연막 색상
- 웹 프런트엔드(React) 정적 페이지 서빙
- `/connect` 접속 시 `steam://connect/...`로 리다이렉트해 게임 서버에 바로 접속

API

| 메서드 | 경로 | 설명 |
|---|---|---|
| POST | `/api/user` | 유저 정보 조회 (`Authorization` 헤더에 스팀 ID) |
| POST | `/api/skin` | 스킨 설정 조회 |
| PUT | `/api/skin/detail` · `/model` · `/music` · `/glove` · `/knife` · `/smoke` | 항목별 스킨 설정 변경 |

## 기술 스택
| 영역 | 기술 |
|---|---|
| 언어 | Java 17 |
| 프레임워크 | Spring Boot 3.2.3 (Web), Lombok, Gson |
| 데이터 | MongoDB (Spring Data MongoDB) |
| 인프라 | Gradle (Kotlin DSL) |

## 구조
```
src/main/java/com/github/ioloolo/zephyr/
  ZephyrApplication.java
  domain/user/      유저 조회, Steam Web API 호출
  domain/skin/      스킨 설정 조회·변경
  domain/react/     React 페이지 서빙, /connect 리다이렉트
src/main/resources/application_template.yaml   설정 템플릿
```

## 실행
```bash
cp src/main/resources/application_template.yaml src/main/resources/application.yaml
./gradlew bootRun
```
`application.yaml`에 서버 포트, MongoDB 접속 정보(`spring.data.mongodb.*`), Steam Web API 키(`app.steam`), 게임 서버 주소·비밀번호(`app.server.host`, `app.server.password`)를 채운다.

## 관련 저장소
- [CS2-ZephyR-Client](https://github.com/son-daehyeon/CS2-ZephyR-Client) — 스킨 설정 웹
- [CS2-ZephyR-Bot](https://github.com/son-daehyeon/CS2-ZephyR-Bot) — 디스코드 봇, 가입 연동과 내전 관리
- [CS2-ZephyR-Plugin](https://github.com/son-daehyeon/CS2-ZephyR-Plugin) — 게임 서버 플러그인

---
2024 · 개인 프로젝트 · 보관됨
