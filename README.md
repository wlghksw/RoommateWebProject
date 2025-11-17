# Roomerang (룸메이트 매칭 플랫폼)

룸메이트를 찾고 공유할 수 있는 웹 플랫폼입니다. 방이 있는 사용자와 방을 찾는 사용자를 연결하고, 중고 물품 공유 및 실시간 채팅 기능을 제공합니다.

## 목차

- [주요 기능](#주요-기능)
- [기술 스택](#기술-스택)
- [프로젝트 구조](#프로젝트-구조)
- [시작하기](#시작하기)
- [환경 설정](#환경-설정)
- [API 엔드포인트](#api-엔드포인트)
- [데이터베이스](#데이터베이스)

## 주요 기능

### 1. 사용자 인증 및 관리
- 회원가입 / 로그인 / 로그아웃
- 아이디 찾기 (보안 질문 기반)
- 비밀번호 재설정 (JWT 토큰 기반)
- 회원 정보 수정 및 비밀번호 변경
- 회원 탈퇴

### 2. 룸메이트 매칭 게시판
- **방 있음**: 방을 제공하는 사용자의 게시글
- **방 없음**: 방을 찾는 사용자의 게시글
- 게시글 작성/수정/삭제
- 이미지 업로드 (최대 8개, 각 10MB)
- 검색 기능
- 페이징 처리
- 즐겨찾기 기능
- 게시글 필터링:
  - 흡연 여부
  - 반려동물 여부
  - 생활 패턴 (아침형/밤형 인간)
  - 지역, 나이, 성별

### 3. 공유 게시판 (중고거래)
- 물품 공유 게시글 작성/수정/삭제
- 이미지 업로드
- 가격, 위치 정보
- 검색 기능
- 페이징 처리
- 즐겨찾기 기능

### 4. 실시간 채팅
- WebSocket 기반 실시간 메시징
- 1:1 채팅방 생성 및 관리
- 메시지 히스토리 저장

### 5. 기타 기능
- 파일 업로드/다운로드
- 즐겨찾기 목록 관리
- 전세 보증 정보 제공 (FAQ)

## 기술 스택

### Backend
- **Java 21**
- **Spring Boot 3.4.2**
- **Spring Security** - 인증 및 보안
- **Spring Data JPA** - 데이터베이스 접근
- **Spring WebSocket** - 실시간 채팅
- **JWT (JSON Web Token)** - 비밀번호 재설정 토큰
- **Lombok** - 보일러플레이트 코드 감소

### Database
- **MariaDB** - 메인 데이터베이스
- **H2 Database** - 개발/테스트용 (옵션)

### Frontend
- **Thymeleaf** - 서버 사이드 템플릿 엔진
- **HTML/CSS/JavaScript**
- **Bootstrap** (추정)

### Build Tool
- **Gradle**

## 프로젝트 구조

```
src/main/java/com/roomerang/
├── auth/                    # JWT 토큰 제공
├── config/                  # 설정 클래스
│   ├── SecurityConfig.java  # Spring Security 설정
│   ├── WebConfig.java       # 웹 설정
│   └── WebSocketConfig.java # WebSocket 설정
├── contoller/               # 컨트롤러
│   ├── AuthController.java  # 인증 관련
│   ├── PostController.java  # 룸메이트 매칭 게시판
│   ├── SharePostController.java # 공유 게시판
│   ├── ChatController.java  # 채팅
│   └── ...
├── dto/                     # 데이터 전송 객체
│   ├── request/            # 요청 DTO
│   └── response/           # 응답 DTO
├── entity/                  # JPA 엔티티
│   ├── User.java
│   ├── Post.java           # 룸메이트 매칭 게시글
│   ├── SharePost.java      # 공유 게시글
│   ├── ChatRoom.java
│   └── Message.java
├── repository/              # JPA 리포지토리
├── service/                 # 비즈니스 로직
├── util/                    # 유틸리티 클래스
└── exception/               # 예외 처리

src/main/resources/
├── application.properties   # 애플리케이션 설정
├── templates/               # Thymeleaf 템플릿
│   ├── auth/               # 인증 관련 페이지
│   ├── match/              # 매칭 게시판 페이지
│   ├── share/              # 공유 게시판 페이지
│   └── chat/               # 채팅 페이지
└── static/                  # 정적 리소스
    ├── css/
    └── images/
```

## 시작하기

### 필수 요구사항
- Java 21 이상
- MariaDB 10.x 이상
- Gradle 7.x 이상

### 설치 및 실행

1. **저장소 클론**
```bash
git clone https://github.com/wlghksw/RoomateWebProject.git
cd RoomateWebProject
```

2. **데이터베이스 설정**
   - MariaDB를 설치하고 실행합니다.
   - `roomerang` 데이터베이스를 생성합니다:
   ```sql
   CREATE DATABASE roomerang;
   ```

3. **애플리케이션 설정**
   - `src/main/resources/application.properties` 파일을 수정하여 데이터베이스 연결 정보를 설정합니다:
   ```properties
   spring.datasource.url=jdbc:mariadb://localhost:3306/roomerang
   spring.datasource.username=your_username
   spring.datasource.password=your_password
   ```

4. **애플리케이션 실행**
```bash
# Gradle Wrapper 사용
./gradlew bootRun

# 또는 IDE에서 RoomerangApplication.java 실행
```

5. **브라우저에서 접속**
   - http://localhost:8080

## 환경 설정

### application.properties 주요 설정

```properties
# 데이터베이스
spring.datasource.driver-class-name=org.mariadb.jdbc.Driver
spring.datasource.url=jdbc:mariadb://localhost:3306/roomerang
spring.datasource.username=springuser1
spring.datasource.password=springuser1

# JPA
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

# 파일 업로드
spring.servlet.multipart.max-file-size=50MB
spring.servlet.multipart.max-request-size=50MB
file.upload-dir=uploads/

# Thymeleaf
spring.thymeleaf.cache=false
```

### 파일 업로드 경로
- 기본 업로드 디렉토리: `uploads/`
- 프로젝트 루트에 `uploads/` 폴더가 자동 생성됩니다.

## API 엔드포인트

### 인증 (Auth)
- `GET /auth/signup` - 회원가입 페이지
- `POST /auth/signup` - 회원가입 처리
- `GET /auth/login` - 로그인 페이지
- `POST /auth/login` - 로그인 처리
- `POST /auth/logout` - 로그아웃
- `GET /auth/find-id` - 아이디 찾기
- `GET /auth/reset-password` - 비밀번호 재설정

### 룸메이트 매칭 게시판 (Post)
- `GET /board/rooms` - 방 있음 게시글 목록
- `GET /board/no-rooms` - 방 없음 게시글 목록
- `GET /board/post/create` - 게시글 작성 페이지
- `POST /board/post/save` - 게시글 저장
- `GET /board/post/{id}` - 게시글 조회
- `GET /board/post/edit/{id}` - 게시글 수정 페이지
- `POST /board/post/update` - 게시글 수정
- `POST /board/post/delete/{id}` - 게시글 삭제
- `GET /board/search` - 게시글 검색

### 공유 게시판 (Share)
- `GET /share/list` - 공유 게시글 목록
- `GET /share/{id}` - 공유 게시글 조회
- `GET /share/create` - 공유 게시글 작성 페이지
- `POST /share/create` - 공유 게시글 저장
- `GET /share/{id}/update` - 공유 게시글 수정 페이지
- `POST /share/{id}/update` - 공유 게시글 수정
- `POST /share/{id}/delete` - 공유 게시글 삭제
- `GET /share/search` - 공유 게시글 검색

### 채팅 (Chat)
- `GET /chat/list` - 채팅방 목록
- `GET /chat/room/{id}` - 채팅방 입장
- WebSocket: `/app/chat` - 메시지 전송
- WebSocket: `/topic/messages` - 메시지 수신

### 사용자 (User)
- `GET /users/myInfo` - 내 정보 조회
- `POST /users/update` - 내 정보 수정
- `GET /users/changePassword` - 비밀번호 변경 페이지
- `POST /users/changePassword` - 비밀번호 변경

## 데이터베이스

### 주요 엔티티

#### User (사용자)
- 사용자 기본 정보 (이름, 생년월일, 성별, 아이디)
- 보안 질문/답변
- 비밀번호

#### Post (룸메이트 매칭 게시글)
- 게시글 정보 (제목, 내용, 지역, 나이, 성별)
- 방 정보 (보증금, 월세)
- 선호사항 (흡연, 반려동물, 생활 패턴)
- 이미지 URL 목록

#### SharePost (공유 게시글)
- 게시글 정보 (제목, 내용, 가격, 위치)
- 이미지 URL 목록

#### ChatRoom (채팅방)
- 채팅방 정보
- 참여자 정보

#### Message (메시지)
- 메시지 내용
- 발신자 정보
- 전송 시간

#### FavoritePost (즐겨찾기)
- 사용자 ID
- 게시글 ID
- 게시글 타입 (MATCH, NO_ROOM, SHARE)


