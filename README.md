# Cursor Workspace 중앙 관리 저장소

MCP (Model Context Protocol) Tools를 활용한 다양한 프로젝트들을 관리하는 워크스페이스의 중앙 관리 저장소입니다.

## 🏗️ 저장소 구조

```
cursor-workspace/
├── PROJECTS.md                    # 📊 전역 프로젝트 개요 및 진행률
├── README.md                      # 📋 이 파일 (중앙 관리 저장소 개요)
└── docs/                          # 📚 공통 문서
    ├── architecture/              # 🏗️ 아키텍처 문서
    │   └── overview.md            # 전체 시스템 아키텍처
    └── guidelines/                # 📋 개발 가이드라인
        └── coding-style.md        # 코딩 스타일 가이드
```

## 🚀 프로젝트 현황

### 전체 진행률
- **public-data-mcp-server**: 40% 완료 (기본 MCP 서버 구현 완료)
- **spring-boot-mcp-integration**: 30% 완료 (멀티 모듈 구조 완성)
- **vue-mcp-integration**: 10% 완료 (프로젝트 기반 설정)
- **전체**: 약 27% 완료

### 상세 현황
자세한 프로젝트 현황은 [PROJECTS.md](https://github.com/constant94-dev/cursor-workspace/blob/main/PROJECTS.md)를 참조하세요.

## 📚 문서 가이드

### 🏗️ 아키텍처 문서
- **[전체 시스템 아키텍처](docs/architecture/overview.md)** - 워크스페이스 전체 구조 및 시스템 플로우

### 📋 개발 가이드라인
- **[코딩 스타일 가이드](docs/guidelines/coding-style.md)** - Java, Spring Boot, Vue.js 코딩 표준

## 🎯 개발 전략

### 단계별 접근
1. **1단계**: 백엔드 구현 (Spring Boot + MCP 서버)
2. **2단계**: 프론트엔드 구현 (Vue.js + Nuxt.js)
3. **3단계**: 통합 및 연동

### 기술 스택 통합
- **공통**: Java 21, Gradle, Docker
- **백엔드**: Spring Boot 3.4.0, Redis, JPA, MCP SDK
- **프론트엔드**: Vue.js 3, TypeScript, Nuxt.js, MCP 클라이언트
- **통신**: REST API, WebSocket, MCP STDIO

## 🔗 프로젝트 간 연관성

### 현재 연관성
- **독립적**: 각 프로젝트는 현재 독립적으로 개발 중
- **공통 목표**: MCP 시스템 구축을 위한 협력

### 향후 연관성 (계획)
- **MCP ↔ Spring Boot**: MCP 서버가 Spring Boot API 호출
- **Spring Boot ↔ Vue.js**: REST API를 통한 프론트엔드 연동
- **Vue.js ↔ MCP**: Vue.js에서 MCP 서버 직접 호출
- **통합 배포**: Docker Compose를 통한 통합 배포

## 📊 성능 및 품질 목표

### 성능 목표
- **처리량**: 1000 TPS (초당 1000 요청 처리)
- **응답 시간**: 200ms 이내 응답
- **가용성**: 99.9% 이상

### 품질 목표
- **코드 커버리지**: 80% 이상
- **API 문서화**: SpringDoc OpenAPI 활용
- **보안**: JWT 인증, 데이터 암호화

## 🤝 기여 가이드

### 문서 업데이트
1. 해당 섹션의 문서 수정
2. 변경사항을 커밋
3. Pull Request 생성

### 새 프로젝트 추가
1. 프로젝트 정보를 [PROJECTS.md](https://github.com/constant94-dev/cursor-workspace/blob/main/PROJECTS.md)에 추가
2. 관련 문서 생성
3. 아키텍처 문서 업데이트

## 📝 업데이트 히스토리

### 2025-08-17
- ✅ **중앙 관리 저장소 초기 구성**
- ✅ **아키텍처 문서 작성**
- ✅ **개발 가이드라인 작성**
- ✅ **3개 프로젝트 통합 관리 체계 구축**
- ✅ **워크스페이스 구조 최적화**
- ✅ **MCP 서버 연결 및 설정 완료**

---

**마지막 업데이트**: 2025-08-17  
**작성자**: Ethan  
**상태**: 3개 프로젝트 통합 관리 체계 완성 ✅
