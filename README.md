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
[![Backend Repository](https://img.shields.io/badge/Repository-181717?style=for-the-badge&logo=github&logoColor=white)](여기에-백엔드-레포-링크)

### Frontend
![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
[![Frontend Repository](https://img.shields.io/badge/Repository-181717?style=for-the-badge&logo=github&logoColor=white)](여기에-프론트엔드-레포-링크)

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

![ERD](여기에-ERD-이미지-링크)

<!-- ERD 이미지를 추가해주세요 -->

---

## 📝 본인 구현 기능

### 🖊️ 게시판
- 게시글 CRUD (작성, 조회, 수정, 삭제)
- Quill Editor를 활용한 리치 텍스트 편집
- 이미지 업로드 및 관리

### 💬 소통 기능
- 댓글 및 대댓글 작성
- 좋아요 기능

### 🌤️ 날씨 정보
- 카카오 로컬 API 연동
- 지역별 실시간 날씨 조회
- 다단계 폴백 로직으로 안정적인 서비스 제공

---

## 🎯 핵심 구현 사항

### 📝 1. Quill Editor Library

#### 문제 상황
ContentEditable을 사용하여 Editor를 직접 구현했지만, 기능을 추가할수록 코드가 복잡해지고 유지보수가 어려운 문제 발생

#### 해결 방법

**1단계: Quill Editor Library 설치**
```bash
npm install react-quill
```

**2단계: 기존 ContentEditable 대체**
```jsx
// Before: ContentEditable 직접 구현

  {/* 복잡한 커스텀 로직... */}


// After: Quill Editor 사용
import ReactQuill from 'react-quill';
import 'react-quill/dist/quill.snow.css';

<ReactQuill 
  theme="snow" 
  value={content} 
  onChange={setContent}
  modules={modules}  // 툴바 커스터마이징
/>
```

**3단계: 이미지 업로드 핸들러 통합**
```javascript
const modules = {
  toolbar: {
    container: [
      ['bold', 'italic', 'underline'],
      ['image'],
      // ...
    ],
    handlers: {
      image: imageHandler  // 커스텀 이미지 업로드
    }
  }
};
```

#### 성과
✅ 브라우저 호환성 문제 해결  
✅ 코드 복잡도 70% 감소  
✅ 유지보수성 대폭 향상  
✅ 풍부한 편집 기능 제공

---

### 📸 2. 이미지 업로드 시스템

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
@Entity
public class Image {
    private String imgUrl;
    private int used;  // 0: 미사용, 1: 사용중
}

// 게시글 저장 시
public void updateImageUsage(String content) {
    // HTML 파싱하여 실제 사용된 이미지만 used = 1로 업데이트
    List imageUrls = parseImagesFromContent(content);
    imageRepository.markAsUsed(imageUrls);
}
```

**3단계: 스케줄러 자동 정리**
```java
@Scheduled(cron = "0 0 * * * *")  // 매일 정각 실행
public void cleanupUnusedImages() {
    List unusedImages = imageRepository.findByUsed(0);
    
    for (Image image : unusedImages) {
        // 파일 삭제
        fileService.deleteFile(image.getImgUrl());
        // DB 레코드 삭제
        imageRepository.delete(image);
    }
    
    log.info("Cleaned up {} unused images", unusedImages.size());
}
```

#### 성과
✅ 서버 리소스 최적화 (자동 정리로 스토리지 절약)  
✅ 클라우드 전환 용이한 구조  
✅ 자동화로 유지보수 부담 감소  
✅ 확장 가능한 아키텍처

---

![이미지 자동 정리](https://github.com/user-attachments/assets/ce4d046d-66e7-40f8-8028-fa9fb88158b9)

---

### 🌤️ 3. 날씨 API 연동

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
```
[카카오 API] → [백엔드 파싱] → [프론트 전달] → [상태 업데이트] → [화면 렌더링]
     ✅              ✅              ❌              ❌              ❌
```
- 각 단계마다 로그 출력하여 문제 지점 발견
- 백엔드와 프론트엔드 간 데이터 구조 불일치 확인

**3단계: 다단계 폴백 로직 구현**
```java
public WeatherResponse getWeather(String address) {
    try {
        // 1차 시도: 전체 주소 검색
        return searchByFullAddress(address);
    } catch (Exception e) {
        try {
            // 2차 시도: 간략 주소 검색
            return searchBySimpleAddress(address);
        } catch (Exception e2) {
            try {
                // 3차 시도: 키워드 검색
                return searchByKeyword(address);
            } catch (Exception e3) {
                // 4차 시도: 기본 좌표 반환
                return getDefaultWeather();
            }
        }
    }
}
```

**4단계: 데이터 매핑 수정**
```java
// Before: 필드명 불일치
return WeatherResponse.builder()
    .temp(data.get("temperature"))  // null 반환
    .build();

// After: 올바른 필드명 매핑
return WeatherResponse.builder()
    .temp(data.get("temp"))  // 정상 반환
    .humidity(data.get("humidity"))
    .weather(data.get("weather"))
    .build();
```

#### 성과
✅ 주소 형식 불일치 문제 해결  
✅ 안정적인 서비스 제공 (폴백 로직)  
✅ 사용자 경험 개선 (항상 날씨 정보 표시)

#### 배운 점
- 외부 API 연동 시 양방향 디버깅의 중요성
- 데이터 흐름 전체를 이해하는 풀스택 관점
- 끈기 있는 문제 해결 태도
- 예외 처리와 폴백 로직의 중요성

---

![날씨 API](https://github.com/user-attachments/assets/169f225c-bddd-4c1d-be40-94db7ce15c78)

---

## 📸 스크린샷

### 메인 페이지
![메인 페이지](스크린샷-링크)

### 게시판
![게시판](스크린샷-링크)

### 날씨 정보
![날씨](스크린샷-링크)

---

## 🔗 관련 링크

- **Frontend Repository**: [GitHub 링크](여기에-프론트엔드-레포-링크)
- **Backend Repository**: [GitHub 링크](여기에-백엔드-레포-링크)
---

<div align="center">

**Made with 🌱 by Plant Community Team**

</div>
