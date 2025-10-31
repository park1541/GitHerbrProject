# 🌱 Plant Community Platform

> 식물 애호가들을 위한 커뮤니티 및 관리 플랫폼

## 📋 프로젝트 소개

식물을 사랑하는 사람들을 위한 커뮤니티 플랫폼입니다.<br>
실시간 날씨 정보와 라즈베리파이 연동을 통해 스마트팜 관리를 지원하며,<br>
사용자들이 식물 관련 정보를 공유하고 소통할 수 있는 공간을 제공합니다.

### 개발 기간
- 2025.09. ~ 2025.11 (3개월)

### 개발 인원
- **총 4명**
- **내 역할**: 커뮤니티 게시판 CRUD, 댓글/대댓글, 좋아요, 날씨 API 연동, 이미지 업로드 시스템

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

---

## ✨ 주요 기능

### 1. 게시판 시스템
- CRUD 기능 (생성, 조회, 수정, 삭제)
- 댓글 및 대댓글 작성
- 좋아요 기능
- Quill Editor 기반 리치 텍스트 작성
- 이미지 업로드 지원
- 게시판 페이지네이션

### 2. 이미지 관리 시스템
- URL 기반 이미지 저장 구조
- 미사용 이미지 자동 감지
- 스케줄러 기반 자동 정리 (매일 정각)
- 클라우드 스토리지 전환 대비 설계

### 3. 날씨 정보 연동
- 카카오 로컬 API 연동
- 지역별 실시간 날씨 조회
- 다단계 폴백 로직으로 안정적인 서비스 제공

### 4. 마이페이지
- 내 게시글 관리
- 식물 정보 관리
- 캘린더 기능

### 5. 관리자 페이지
- 회원 관리
- 게시판 관리
- 문의 관리
---

## 🗄️ ERD

<img width="1279" height="973" alt="community-ERD" src="https://github.com/user-attachments/assets/062f318e-0c51-4e18-b4ee-f20a4f3c31db" />

---

## 📝 본인 구현 기능

### 🖊️ 게시판
- 게시글 CRUD (작성, 조회, 수정, 삭제)
- Quill Editor를 활용한 리치 텍스트 편집
- 이미지 업로드 및 관리
- 게시판 페이지네이션

### 💬 소통 기능
- 댓글 및 대댓글 작성
- 좋아요 기능

### 🌤️ 날씨 정보
- 카카오 로컬 API 연동
- 지역별 실시간 날씨 조회
- 다단계 폴백 로직으로 안정적인 서비스 제공

---

## 🎯 핵심 구현 사항

### 📝 1. Quill Editor Library 도입

#### 문제 상황
ContentEditable을 사용하여 에디터를 직접 구현했지만, 기능을 추가할수록 코드가 복잡해져 유지보수가 어려워지는 문제 발생.

#### 해결 방법

**라이브러리 도입으로 안정성 확보**
- 검증된 오픈소스 라이브러리 Quill Editor를 도입

**기존 커스텀 코드를 라이브러리로 단순화**
- ContentEditable 직접 구현 제거
- 선언형 React 컴포넌트로 교체하여 코드 가독성 향상
- 상태 관리를 React state와 통합

**프로젝트 특화 커스터마이징**
- Quill의 toolbar 커스터마이징으로 필요한 기능만 선택
- 이미지 업로드 핸들러를 커스텀하여 서버 연동
- 게시글 작성에 필요한 기능(볼드, 이탤릭, 이미지, 리스트 등)만 제공

#### 성과
- 유지보수성 대폭 향상 (복잡한 브라우저 호환성 처리 제거)
- 개발 속도 단축 (기능 추가가 간단해짐)
- 안정적인 텍스트 에디팅 경험 제공
- 추후 라이브러리 버전 업데이트로 자동 개선

### 📸 2. 이미지 업로드 시스템

#### 문제 상황
사용자가 이미지를 업로드한 후 게시글 작성을 취소하거나, 수정 과정에서 이미지를 삭제하는 경우 미사용 이미지가 서버에 누적되는 문제 발생

#### 해결 방법

**URL 기반 저장**
- 파일명이 아닌 전체 URL을 DB에 저장하여 로컬/클라우드 전환이 용이한 구조로 설계
- AWS S3 등 클라우드 스토리지로의 전환을 대비

**사용 여부 추적**
- Jsoup 라이브러리를 활용하여 게시글의 HTML 콘텐츠를 파싱
- 실제로 사용된 이미지 URL만 추출하여 `used = 1`로 표시
- 사용되지 않은 이미지는 `used = 0`으로 유지

**자동 정리**
- Spring `@Scheduled` 어노테이션을 활용한 스케줄러 구현
- 매일 정각마다 `used = 0`인 이미지를 자동으로 삭제
- 파일 시스템과 DB에서 동시에 제거

#### 성과
- 서버 스토리지 절약 (자동 정리)
- 클라우드 전환이 용이한 확장 가능한 구조
- 수동 관리 불필요 (자동화)
- Jsoup을 활용한 정확한 이미지 사용 추적

---

![이미지 자동 정리](https://github.com/user-attachments/assets/ce4d046d-66e7-40f8-8028-fa9fb88158b9)

---

### 🌤️ 3. 날씨 API 연동

#### 문제 상황
카카오 주소 검색 API 호출은 성공하지만 프론트엔드에서 필요한 데이터가 계속 `null`로 반환되는 문제 발생

#### 해결 방법

**주소 검색 API 연동 안정화**
- 백엔드와 프론트엔드의 데이터 흐름을 로그로 추적하여 문제 지점 파악
- 카카오 주소 검색 API가 응답하지만 사용자 입력 형식과 API 요구 형식의 불일치 발견
- 예시: "울산" 검색 시 결과 없음, "울산광역시" 검색 시 정상 작동

**주소 형식 자동 변환**
- 사용자 입력 주소를 정규화하여 API 요청 형식으로 자동 변환
- 도시명 파싱 로직으로 부분 주소 추출 가능

**다단계 폴백 로직 구현**
- 1차 시도: 사용자가 입력한 전체 주소로 검색
- 2차 시도: 주소를 파싱하여 도시명만 추출 후 재검색
- 3차 시도: 키워드 기반 검색으로 유연한 매칭
- 4차 시도: 모두 실패 시 기본 좌표(서울)로 설정하여 서비스 중단 방지

#### 성과
- 주소 입력 형식에 관계없이 날씨 정보 조회 가능
- 4단계 폴백 로직으로 API 실패 상황 대응
- 사용자가 항상 날씨 정보를 확인할 수 있는 안정적인 서비스 제공
- 예외 상황에 대한 우아한 처리로 사용자 경험 개선


---
![날씨-API-_online-video-cutter com_](https://github.com/user-attachments/assets/c48ac4e3-23cf-4cdb-8117-e7a984e81fa0)

---

### 📖 4. 페이지네이션 구현

#### 문제 상황
모든 게시글을 한 번에 조회하도록 구현했는데, 게시글이 많아지면서 
사용자가 원하는 게시글을 찾기 위해 계속 스크롤을 내려야 하는 불편함 발생

#### 해결 방법

**데이터 조회 수정**
- MyBatis의 `LIMIT`, `OFFSET`을 활용하여 페이지별 필요한 데이터만 조회
- 페이지 번호와 한 페이지당 개수(10개)로 쿼리 수정

**페이지 계산 로직**
- 전체 게시글 개수를 조회하여 총 페이지 수 계산
- 사용자가 접근 가능한 최대 페이지 번호 검증
- 페이지 정보(현재 페이지, 총 페이지, 시작 행 번호 등)를 DTO로 전달

**프론트엔드 페이지 네비게이션**
- 현재 페이지 번호 하이라이트 표시
- 이전/다음 버튼으로 페이지 이동
- 페이지 번호를 클릭하여 직접 이동
- 마지막 페이지 도달 시 다음 버튼 비활성화

#### 성과
- 데이터베이스 부하 최소화 (필요한 데이터만 처리)
- 사용자 경험 향상 (스크롤이 부드러워지고 페이지 전환 빠름)
- 확장성 확보 (게시글이 계속 증가해도 성능 유지)

![페이지-네이션](https://github.com/user-attachments/assets/599a6a82-73ca-4735-bcce-90570477b497)

---
## 🎥 웹 시연영상

### 게시판 CRUD
https://github.com/user-attachments/assets/2b5de776-68ba-4fb4-bde6-aa827790db5e

### 좋아요, 덧글/더덧글

https://github.com/user-attachments/assets/a3dd8a85-269c-4f15-bda8-462e57d85b42

---
## 🔗 관련 링크

- **Frontend Repository**: https://github.com/park1541/frontend_plant_comunity.git
- **Backend Repository**: https://github.com/park1541/backend_plant_comunity.git
---

<div align="center">

**Made with 🌱 by Plant Community Team**

</div>
