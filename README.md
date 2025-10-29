# 🌱 Plant Community Platform

> 식물 애호가들을 위한 커뮤니티 및 관리 플랫폼

## 📋 프로젝트 소개

식물을 사랑하는 사람들이 모여 정보를 공유하고 소통할 수 있는 커뮤니티 플랫폼입니다.
게시글 작성, 댓글, 좋아요 기능과 함께 실시간 날씨 정보를 제공하여 식물 관리에 도움을 줍니다.
또한, 라즈베리파이를 활용하여 스마트팜을 운영하는 사람들이 더욱 쉽게 관리할 수 있도록 

### 개발 기간
- 2025.09. ~ 2025.11 (3개월)

### 개발 인원
- **총 4명**
- **내 역할**: 커뮤니티 게시판 CRUD, 덧글 더덧글, 좋아요, 날씨API, 이미지 업로드 시스템

---

## 🛠️ 기술 스택

### Backend
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-6DB33F?style=for-the-badge&logo=springboot&logoColor=white)
![MariaDB](https://img.shields.io/badge/MariaDB-003545?style=for-the-badge&logo=mariadb&logoColor=white)
![Java](https://img.shields.io/badge/Java-007396?style=for-the-badge&logo=openjdk&logoColor=white)

### Frontend
![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

### Version Control
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)

### Collaboration
![Notion](https://img.shields.io/badge/Notion-000000?style=for-the-badge&logo=notion&logoColor=white)

## ✨ 주요 기능

### 1. 게시판 시스템
- CRUD 기능 (생성, 조회, 수정, 삭제)
- 댓글 및 좋아요 기능
- 이미지 업로드 지원
- Quill Editor 기반 리치 텍스트 작성

### 2. 이미지 관리 시스템
- URL 기반 이미지 저장 구조
- 미사용 이미지 자동 감지
- 스케줄러 기반 자동 정리
- 클라우드 전환 대비 설계

### 3. 날씨 정보 연동
- 카카오 날씨 API 연동
- 실시간 날씨 정보 제공
- 에러 핸들링 및 폴백 로직

### 4. 마이페이지
- 내 게시글 관리
- 식물 정보 관리
- 캘린더 기능

### 5. 관리자 페이지
- 회원 관리
- 게시판 관리
- 문의 관리

---

## 🎯 핵심 구현 사항

### 📸 1. 이미지 업로드 시스템

#### 문제 상황
사용자가 이미지를 업로드한 후 게시글 작성을 취소하거나, 수정 과정에서 이미지를 삭제하는 경우 미사용 이미지가 서버에 누적되는 문제 발생

#### 해결 방법

**1단계: URL 기반 저장 구조**
```java
// 파일명만 저장 (X)
String fileName = "abc123.jpg";

// 전체 URL 저장 (O)
String imgUrl = "http://localhost:8080/upload/abc123.jpg";
```
- 로컬/클라우드 전환 시 URL만 변경하면 됨
- AWS S3 전환 대비 설계

**2단계: 사용 여부 추적**
```java
@Scheduled(cron = "0 0 3 * * *")  // 매일 새벽 3시
public void cleanupUnusedImages() {
    // used = 0 인 이미지 삭제
}
```
- `used` 필드로 사용 여부 추적
- 게시글 저장 시 HTML 파싱하여 실제 사용된 이미지만 `used = 1`로 업데이트

**3단계: 스케줄러 자동 정리**
- Spring `@Scheduled` 활용
- 매일 자동으로 미사용 이미지 삭제

#### 성과
✅ 서버 리소스 최적화  
✅ 클라우드 전환 용이한 구조  
✅ 자동화로 유지보수 부담 감소

---

### 🌤️ 2. 날씨 API 연동

#### 문제 상황
카카오 날씨 API 호출은 성공하지만 프론트엔드에서 필요한 데이터가 계속 `null`로 반환되는 문제 발생

#### 해결 과정

**1단계: 문제 범위 좁히기**
```javascript
// 백엔드 응답 확인
console.log('Backend response:', response);

// 프론트엔드 수신 확인
console.log('Frontend received:', data);
```

**2단계: 데이터 흐름 추적**
- 백엔드: API 응답 → 데이터 파싱 → 프론트 전달
- 프론트엔드: 데이터 수신 → 상태 업데이트 → 화면 렌더링
- 각 단계마다 로그 출력하여 문제 지점 발견

**3단계: 근본 원인 파악**
- API 응답 구조와 프론트 기대 형식 불일치
- 데이터 매핑 로직 수정으로 해결

#### 배운 점
- 외부 API 연동 시 양방향 디버깅의 중요성
- 데이터 흐름 전체를 이해하는 풀스택 관점
- 끈기 있는 문제 해결 태도

---

## 🗂️ ERD

## 📸 스크린샷

(스크린샷 추가 예정)

## 📝 회고

### 잘한 점
- 확장 가능한 아키텍처 설계 (URL 기반 이미지 관리)
- 자동화를 통한 유지보수 부담 감소 (스케줄러)
- 문제 해결 과정에서 끈기 있게 원인 파악

### 아쉬운 점
- 테스트 코드 부재
- 프론트엔드 상태 관리 미흡 (Redux 등 미사용)
- API 문서화 부족

### 배운 점
- 단순 기능 구현을 넘어 장기적 관점에서 설계하는 능력
- 외부 API 연동 및 에러 핸들링 경험
- 풀스택 개발을 통한 전체 흐름 이해

---

## 📖 프로젝트 소개


## ✨ 주요 기능

### 📝 게시판
- 게시글 CRUD (작성, 조회, 수정, 삭제)
- 이미지 업로드 기능
- Quill Editor를 활용한 리치 텍스트 편집

### 💬 소통 기능
- 댓글 및 대댓글 작성
- 좋아요 기능

### 🌤️ 날씨 정보
- 카카오 로컬 API 연동
- 지역별 실시간 날씨 조회
- 다단계 폴백 로직으로 안정적인 서비스 제공

## 💡 주요 기술적 구현

### 이미지 업로드 시스템
- URL 전체를 DB에 저장하여 환경 독립적인 구조 구현
- 사용되지 않은 이미지 자동 정리 (Spring Scheduler)
- 로컬 스토리지에서 클라우드 스토리지로 쉬운 마이그레이션 지원
## 📹 
### 날씨 API 안정성
- 다단계 폴백 로직 구현
  1. 전체 주소 검색
  2. 간략 주소 검색
  3. 키워드 검색
  4. 기본 좌표 반환
- 주소 형식 불일치 문제 해결

### 에디터 개선
- ContentEditable에서 Quill Editor로 전환
- 브라우저 호환성 문제 해결
- 유지보수성 향상


