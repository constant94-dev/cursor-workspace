# 워크스페이스 아키텍처 개요

## 🏗️ 전체 시스템 구조

```
┌─────────────────────────────────────────────────────────────┐
│                    Cursor Workspace                         │
│  /Users/ethan/Cursor/                                       │
├─────────────────────────────────────────────────────────────┤
│  cursor-workspace/          # 중앙 관리 저장소               │
│  ├── docs/                  # 공통 문서                     │
│  ├── shared/                # 공유 라이브러리               │
│  └── README.md              # 워크스페이스 개요             │
├─────────────────────────────────────────────────────────────┤
│  mcp-public-data/           # MCP 공공데이터 프로젝트        │
│  ├── src/main/java/         # MCP Java SDK 서버             │
│  ├── run-build-gradle.sh    # Java MCP 서버 실행            │
│  └── run-github-mcp.sh      # GitHub MCP 서버 실행          │
├─────────────────────────────────────────────────────────────┤
│  spring-boot-mcp-integration/ # Spring Boot 통합 프로젝트    │
│  ├── api/                   # API 모듈                     │
│  ├── core/                  # 공통 모듈                     │
│  ├── web/                   # 웹 모듈                       │
│  └── Redis                  # 세션 관리                     │
└─────────────────────────────────────────────────────────────┘
```

## 🔄 시스템 플로우

### 1. MCP 서버 플로우
```
Cursor Chat → MCP Protocol → mcp-public-data → 공공데이터 API
```

### 2. Spring Boot 통합 플로우
```
MCP Server → Spring Boot API → Redis Session → Database
```

### 3. 전체 통합 플로우
```
Cursor Chat → MCP Protocol → mcp-public-data → Spring Boot API → Redis → Database
```

## 🎯 각 프로젝트의 역할

### mcp-public-data
- **역할**: MCP 프로토콜 기반 공공데이터 서버
- **기능**: 
  - 공공데이터 API 호출
  - MCP 도구 제공
  - JSON-RPC 응답 생성
- **기술**: Java 21, MCP Java SDK, Gradle

### spring-boot-mcp-integration
- **역할**: Spring Boot 기반 통합 서버
- **기능**:
  - REST API 제공
  - 세션 관리
  - 데이터 처리 및 변환
- **기술**: Spring Boot 3.x, Redis, Gradle

## 🔗 프로젝트 간 통신

### 현재 통신 방식
- **독립적**: 각 프로젝트는 독립적으로 운영

### 향후 통신 방식 (계획)
- **HTTP API**: MCP 서버가 Spring Boot API 호출
- **Redis**: 세션 데이터 공유
- **공통 라이브러리**: 코드 재사용

## 📊 기술 스택 매트릭스

| 구성 요소 | mcp-public-data | spring-boot-mcp-integration | 공통 |
|-----------|----------------|----------------------------|------|
| **언어** | Java 21 | Java 21 | Java 21 |
| **빌드 도구** | Gradle | Gradle | Gradle |
| **프레임워크** | MCP Java SDK | Spring Boot 3.x | - |
| **데이터베이스** | - | Redis | - |
| **컨테이너** | Docker | Docker | Docker |
| **통신** | STDIO | HTTP/REST | - |

## 🚀 확장 계획

### 1단계: 기본 기능 구현
- [x] MCP Java SDK 서버 구성
- [ ] Spring Boot 멀티 모듈 구성
- [ ] 공공데이터 API 연동

### 2단계: 통합 기능 구현
- [ ] MCP ↔ Spring Boot 통신
- [ ] Redis 세션 관리
- [ ] 공통 라이브러리 개발

### 3단계: 고급 기능 구현
- [ ] 마이크로서비스 아키텍처
- [ ] 모니터링 및 로깅
- [ ] CI/CD 파이프라인

## 🔒 보안 아키텍처

### 인증/인가
- **MCP 서버**: API 키 기반 인증
- **Spring Boot**: JWT 토큰 기반 인증
- **Redis**: 패스워드 기반 인증

### 데이터 보호
- **민감 정보**: 환경 변수로 분리
- **API 키**: `.secrets.env` 파일로 관리
- **세션 데이터**: Redis 암호화 저장

## 📈 성능 목표

### 처리량
- **MCP 서버**: 1000 TPS
- **Spring Boot API**: 2000 TPS
- **전체 시스템**: 1000 TPS

### 응답 시간
- **MCP 서버**: 200ms 이내
- **Spring Boot API**: 100ms 이내
- **전체 시스템**: 300ms 이내

### 가용성
- **목표**: 99.9% 이상
- **모니터링**: Actuator 기반 헬스 체크
- **복구**: 자동 재시작 및 장애 복구

---

**마지막 업데이트**: 2025-08-17  
**작성자**: Ethan  
**상태**: 아키텍처 설계 완료 ✅
