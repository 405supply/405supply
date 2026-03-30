## Marine Log Backend 프로젝트 - 이력서용 기술 요약

"Marine Log Backend" 프로젝트는 Java 21로 개발된 견고한 Spring Boot 애플리케이션으로, 다양한 활동을 관리하고 처리하며 지리 데이터 처리에 중점을 둡니다.

### 전반적인 프로젝트 기술 요점:

*   **백엔드 프레임워크:** Spring Boot (Java 21)로 개발되었으며, 신속한 애플리케이션 개발 및 배포를 위한 포괄적인 생태계를 활용합니다.
*   **데이터베이스 관리:** **PostGIS**와 함께 PostgreSQL을 활용하여 고급 공간 데이터 저장, 쿼리 및 분석을 수행하며 정교한 지리적 기능을 제공합니다.
*   **캐싱 및 메시징:** 효율적인 캐싱 메커니즘 및 잠재적으로 세션 관리를 위해 Redis와 통합되었습니다.
*   **보안 및 인증:** Spring Security를 사용하여 강력한 보안을 구현하며, 주요 제공업체(Google, Naver, Kakao)와의 OAuth2 인증 및 안전한 API 접근 및 권한 부여를 위한 JWT(JSON Web Token)를 지원합니다.
*   **클라우드 서비스 통합:** 확장 가능한 파일 저장을 위해 AWS S3를 활용하며, Thumbnailator를 사용한 이미지 처리 기능도 포함합니다.
*   **API 문서화:** Springdoc OpenAPI (Swagger UI)를 사용하여 명확하고 대화형 API 문서를 제공합니다.
*   **아키텍처 패턴:** 관심사 분리, 유지보수성 및 확장성을 촉진하는 클린 레이어드 아키텍처(Controller, Service, Repository, Domain)를 따르며, 효율적인 데이터 전송을 위해 DTO(Data Transfer Objects) 및 Projection을 사용합니다.
*   **비즈니스 로직 및 데이터 내보내기:** 복잡한 워크플로우 관리를 위한 Spring State Machine과 유연한 데이터 내보내기 기능을 위한 OpenCSV를 통합합니다.

### 주요 기술 요점:

`location` 모듈은 고급 지리 데이터 처리 및 통합을 보여주는 핵심 구성 요소입니다.

*   **PostGIS 통합:**
    *   PostgreSQL의 PostGIS 확장을 포괄적으로 사용하여 공간 데이터를 효율적으로 저장, 쿼리 및 분석합니다.
    *   `geometry(Point, 4326)` PostGIS 데이터 유형을 사용하여 지리적 지점을 저장하며, `4326`은 WGS84(위도/경도)의 표준 SRID입니다.
*   **공간 데이터 유형 (JTS):**
    *   Java 애플리케이션 내에서 지리적 좌표를 나타내기 위해 JTS(Java Topology Suite) `Point` 지오메트리 유형을 활용하여 정확한 공간 데이터 처리를 보장합니다.
*   **지리 위치 서비스(Geolocation Services)를 위한 외부 API 통합:**
    *   **카카오 지오코딩 API:** 카카오 지오코딩 API와 강력하게 통합하여 리버스 지오코딩(지리적 좌표를 사람이 읽을 수 있는 주소(지역 심도 이름, 전체 주소)로 변환)을 구현합니다. 이는 Spring의 `RestClient`를 사용하여 원활한 외부 통신을 통해 이루어집니다.
    *   **Google Maps 통합:** 전용 REST 엔드포인트를 통해 필요한 API 키 및 Map ID를 제공하여 Google Maps의 클라이언트 측 통합을 용이하게 하며, 대화형 지도 표시를 가능하게 합니다.
*   **고급 공간 쿼리 및 데이터 처리:**
    *   `LocationRepository` 내에서 강력한 PostGIS 함수를 활용하는 고급 네이티브 SQL 쿼리를 설계하고 구현했습니다.
        *   **지리적 집계:** `GROUP BY` 및 `ST_X`, `ST_Y`와 같은 공간 함수를 사용하여 평균 중심점을 계산하고 다양한 지역 수준(예: 전국, 광역 지역)별로 활동 데이터를 요약합니다.
        *   **공간 필터링:** 정의된 지리적 경계 상자(`ST_MakeEnvelope`, `geometry &&` 연산자) 내의 활동을 효율적으로 쿼리하여 특정 지도 보기에 관련 있는 데이터를 검색합니다.
        *   **가중치 기반 히트맵 데이터 생성:** 원시 위치 데이터를 처리하여 히트맵 시각화에 적합한 가중치 기반 그리드 데이터 세트를 생성합니다. 여기에는 지점을 클러스터링하기 위한 `ST_SnapToGrid` 사용과 효과적인 시각적 표현을 위한 가중치(1-10 스케일)를 정규화하는 사용자 정의 로직이 포함됩니다.
*   **데이터 프로젝션 및 DTO:**
    *   Spring Data JPA Projection(`LocationAggregationProjection`, `LocationSpecificProjection`, `LocationWeightedProjection`)을 활용하여 데이터의 특정 하위 집합을 효율적으로 검색합니다.
    *   다른 아키텍처 계층 간의 위치 데이터에 대한 구조화되고 명확한 통신을 위해 DTO(Data Transfer Objects)를 사용합니다.
*   **트랜잭션 관리:**
    *   Spring의 `@Transactional` 어노테이션을 사용하여 서비스 작업 전반에 걸쳐 효과적인 트랜잭션 관리를 수행하며, 특히 데이터 저장 및 검색 시 데이터 일관성과 무결성을 보장합니다.
