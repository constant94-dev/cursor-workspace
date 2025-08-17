# 코딩 스타일 가이드라인

## 🎯 개요

이 문서는 Cursor Workspace의 모든 프로젝트에서 일관된 코드 품질을 유지하기 위한 코딩 스타일 가이드를 정의합니다.

## 📋 Java 코딩 스타일

### 명명 규칙

#### 클래스명
```java
// ✅ 올바른 예시
public class PublicDataService { }
public class MCPController { }
public class RedisSessionManager { }

// ❌ 잘못된 예시
public class publicDataService { }
public class mcp_controller { }
public class redisSessionManager { }
```

#### 메서드명
```java
// ✅ 올바른 예시
public void processPublicData() { }
public String getMCPResponse() { }
public boolean isValidSession() { }

// ❌ 잘못된 예시
public void ProcessPublicData() { }
public String get_mcp_response() { }
public boolean IsValidSession() { }
```

#### 변수명
```java
// ✅ 올바른 예시
private String apiKey;
private List<PublicData> dataList;
private boolean isConnected;

// ❌ 잘못된 예시
private String API_KEY;
private List<PublicData> DataList;
private boolean connected;
```

#### 상수명
```java
// ✅ 올바른 예시
public static final String API_BASE_URL = "https://api.data.go.kr";
public static final int MAX_RETRY_COUNT = 3;
public static final String DEFAULT_ENCODING = "UTF-8";

// ❌ 잘못된 예시
public static final String apiBaseUrl = "https://api.data.go.kr";
public static final int maxRetryCount = 3;
public static final String defaultEncoding = "UTF-8";
```

### 패키지명
```java
// ✅ 올바른 예시
package com.datapublic.mcp;
package com.datapublic.spring.api;
package com.datapublic.spring.core;

// ❌ 잘못된 예시
package com.DataPublic.MCP;
package com.datapublic.spring.API;
package com.datapublic.spring.Core;
```

### 들여쓰기 및 포맷팅

#### 들여쓰기
- **탭 대신 스페이스 사용**: 4칸 공백
- **한 줄 최대 길이**: 120자

#### 중괄호 스타일
```java
// ✅ K&R 스타일 (열린 중괄호는 같은 줄)
public class Example {
    public void method() {
        if (condition) {
            // 코드
        } else {
            // 코드
        }
    }
}

// ❌ Allman 스타일 (열린 중괄호는 새 줄)
public class Example
{
    public void method()
    {
        if (condition)
        {
            // 코드
        }
        else
        {
            // 코드
        }
    }
}
```

## 📝 주석 가이드라인

### 클래스 주석
```java
/**
 * 공공데이터 API를 호출하여 MCP 도구로 제공하는 서비스 클래스
 * 
 * @author Ethan
 * @since 2025-08-17
 * @version 1.0.0
 */
public class PublicDataService {
    // 클래스 내용
}
```

### 메서드 주석
```java
/**
 * 공공데이터 API를 호출하여 결과를 반환합니다.
 * 
 * @param apiKey API 인증 키
 * @param requestData 요청 데이터
 * @return 공공데이터 응답 결과
 * @throws ApiException API 호출 실패 시
 * @throws ValidationException 유효하지 않은 데이터 시
 */
public PublicDataResponse callPublicDataApi(String apiKey, RequestData requestData) {
    // 메서드 구현
}
```

### 인라인 주석
```java
// API 키 유효성 검사
if (StringUtils.isEmpty(apiKey)) {
    throw new ValidationException("API 키는 필수입니다.");
}

// Redis에 세션 데이터 저장 (TTL: 30분)
redisTemplate.opsForValue().set(sessionKey, sessionData, Duration.ofMinutes(30));
```

## 🔧 Spring Boot 코딩 스타일

### 컨트롤러
```java
@RestController
@RequestMapping("/api/v1/public-data")
@Slf4j
public class PublicDataController {
    
    private final PublicDataService publicDataService;
    
    public PublicDataController(PublicDataService publicDataService) {
        this.publicDataService = publicDataService;
    }
    
    @GetMapping("/search")
    public ResponseEntity<ApiResponse<PublicDataResponse>> searchPublicData(
            @RequestParam String keyword,
            @RequestParam(defaultValue = "1") int page,
            @RequestParam(defaultValue = "10") int size) {
        
        log.info("공공데이터 검색 요청: keyword={}, page={}, size={}", keyword, page, size);
        
        try {
            PublicDataResponse response = publicDataService.searchData(keyword, page, size);
            return ResponseEntity.ok(ApiResponse.success(response));
        } catch (Exception e) {
            log.error("공공데이터 검색 실패", e);
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
                    .body(ApiResponse.error("검색 중 오류가 발생했습니다."));
        }
    }
}
```

### 서비스
```java
@Service
@Slf4j
public class PublicDataService {
    
    private final PublicDataRepository repository;
    private final RedisTemplate<String, Object> redisTemplate;
    
    public PublicDataService(PublicDataRepository repository, 
                           RedisTemplate<String, Object> redisTemplate) {
        this.repository = repository;
        this.redisTemplate = redisTemplate;
    }
    
    @Transactional(readOnly = true)
    public PublicDataResponse searchData(String keyword, int page, int size) {
        // 캐시에서 먼저 조회
        String cacheKey = generateCacheKey(keyword, page, size);
        PublicDataResponse cachedResponse = (PublicDataResponse) redisTemplate.opsForValue().get(cacheKey);
        
        if (cachedResponse != null) {
            log.debug("캐시에서 데이터 조회: {}", cacheKey);
            return cachedResponse;
        }
        
        // DB에서 조회
        PublicDataResponse response = repository.searchByKeyword(keyword, page, size);
        
        // 캐시에 저장 (TTL: 30분)
        redisTemplate.opsForValue().set(cacheKey, response, Duration.ofMinutes(30));
        
        return response;
    }
}
```

## 🧪 테스트 코딩 스타일

### 단위 테스트
```java
@ExtendWith(MockitoExtension.class)
class PublicDataServiceTest {
    
    @Mock
    private PublicDataRepository repository;
    
    @Mock
    private RedisTemplate<String, Object> redisTemplate;
    
    @InjectMocks
    private PublicDataService publicDataService;
    
    @Test
    @DisplayName("키워드로 공공데이터 검색 시 성공적으로 결과를 반환한다")
    void searchData_WithValidKeyword_ReturnsSuccessResponse() {
        // Given
        String keyword = "환경";
        int page = 1;
        int size = 10;
        PublicDataResponse expectedResponse = createMockResponse();
        
        when(repository.searchByKeyword(keyword, page, size))
                .thenReturn(expectedResponse);
        
        // When
        PublicDataResponse actualResponse = publicDataService.searchData(keyword, page, size);
        
        // Then
        assertThat(actualResponse).isNotNull();
        assertThat(actualResponse.getData()).hasSize(10);
        verify(repository).searchByKeyword(keyword, page, size);
    }
}
```

### 통합 테스트
```java
@SpringBootTest
@AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
@TestPropertySource(properties = {
    "spring.datasource.url=jdbc:h2:mem:testdb",
    "spring.jpa.hibernate.ddl-auto=create-drop"
})
class PublicDataControllerIntegrationTest {
    
    @Autowired
    private TestRestTemplate restTemplate;
    
    @Test
    @DisplayName("공공데이터 검색 API 호출 시 성공적으로 응답한다")
    void searchPublicData_ReturnsSuccessResponse() {
        // Given
        String url = "/api/v1/public-data/search?keyword=환경&page=1&size=10";
        
        // When
        ResponseEntity<ApiResponse> response = restTemplate.getForEntity(url, ApiResponse.class);
        
        // Then
        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
        assertThat(response.getBody()).isNotNull();
        assertThat(response.getBody().isSuccess()).isTrue();
    }
}
```

## 📊 코드 품질 기준

### 코드 커버리지
- **단위 테스트**: 80% 이상
- **통합 테스트**: 70% 이상
- **전체 커버리지**: 75% 이상

### 복잡도 제한
- **메서드 복잡도**: 10 이하
- **클래스 복잡도**: 50 이하
- **순환 복잡도**: 15 이하

### 코드 스멜 제거
- **중복 코드**: 제거 필수
- **긴 메서드**: 50줄 이하
- **큰 클래스**: 500줄 이하
- **매직 넘버**: 상수로 정의

## 🔍 코드 리뷰 체크리스트

### 기능적 검토
- [ ] 요구사항을 정확히 구현했는가?
- [ ] 예외 처리가 적절한가?
- [ ] 로깅이 충분한가?

### 코드 품질 검토
- [ ] 코딩 스타일을 준수했는가?
- [ ] 테스트 코드가 작성되었는가?
- [ ] 문서화가 충분한가?

### 보안 검토
- [ ] 민감한 정보가 노출되지 않았는가?
- [ ] 입력값 검증이 적절한가?
- [ ] SQL 인젝션 방지가 되어 있는가?

---

**마지막 업데이트**: 2025-08-17  
**작성자**: Ethan  
**상태**: 코딩 스타일 가이드 완성 ✅
