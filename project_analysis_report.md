# 프로젝트 분석 보고서 (이력서 작성용)

이 프로젝트는 "Golden Harvest Inventory"라는 이름의 재고 관리 시스템으로, Spring Boot 프레임워크 기반으로 구축되었으며, 최신 Java 버전(Java 21)과 다양한 엔터프라이즈급 기술 스택을 활용하고 있습니다.

## 1. 주요 기술 스택:

*   **백엔드 프레임워크:** Spring Boot 4.x
*   **언어:** Java 21
*   **빌드 도구:** Gradle
*   **데이터베이스:** MariaDB (H2 for 테스트/개발)
*   **ORM/데이터 액세스:** Spring Data JPA, MyBatis
*   **메시징 시스템:** Apache Kafka (Spring Kafka 연동, AWS MSK IAM Auth 사용)
*   **캐싱/인메모리 데이터 저장:** Redis (Spring Data Redis 연동)
*   **보안:** Spring Security (OAuth2 Resource Server를 통한 JWT 인증/인가, 역할 기반 권한 부여 `hasRole('ADMIN')`, `hasRole('USER')`)
*   **웹:** Spring Boot Starter Web (RESTful API), Spring Boot Starter RestClient (외부 API 연동)
*   **클라우드 서비스 연동:** AWS SDK S3 (파일/객체 저장 추정), AWS MSK IAM Auth
*   **유효성 검사:** Spring Boot Starter Validation (`jakarta.validation`, `@Valid`, `@Min`, `@Max` 등)
*   **비동기 처리:** `@EnableAsync` 어노테이션을 활용한 비동기 로직 처리
*   **유틸리티:** Lombok, Google Guava, Commons IO

## 2. 아키텍처 및 디자인 패턴:

*   **레이어드 아키텍처:** Controller, Service, Repository 계층으로 명확하게 분리되어 있습니다.
*   **RESTful API 설계:** 자원 기반의 일관된 API 엔드포인트(`DiscardController`, `PricePolicyController`, `InventoryQueryController` 등)를 구현했습니다.
*   **CQRS (Command Query Responsibility Segregation) 패턴:**
    *   `command` 패키지(예: `DiscardController`, `PricePolicyController`)는 데이터 변경(생성, 수정, 삭제) 작업을 담당합니다.
    *   `query` 패키지(예: `DiscardQueryController`, `InventoryQueryController`, `PricePolicyQueryController`, `MetricsQueryController`)는 데이터 조회 작업을 담당합니다.
    *   이를 통해 각 책임이 명확히 분리되고, 성능 최적화 및 유지보수 용이성을 확보합니다.
*   **이벤트 기반 아키텍처 (Event-Driven Architecture):** Kafka를 사용하여 비동기 메시징 및 이벤트 처리(`ItemMasterConsumer`, `PurchaseOrderConsumer`, `SalesOrderConsumer`, `MasterDataUpdateEventListener`, `PurchaseOrderEventListener`, `SalesOrderEventListener` 등)를 구현했습니다. 이는 시스템 간의 느슨한 결합(Loose Coupling)을 제공하고 확장성을 높입니다.
*   **공통 모듈 설계:** `common` 패키지(broker, config, exception, idempotent, response, security)를 통해 재사용 가능한 컴포넌트들을 모듈화했습니다.
    *   **예외 처리:** `GlobalExceptionHandler`를 통해 전역적인 예외 처리를 구현하여 안정적인 API 응답을 제공합니다.
    *   **Idempotency:** `BloomFilterManager`를 사용하여 메시지 처리의 멱등성을 보장하는 로직이 포함될 수 있습니다.
    *   **API 응답 표준화:** `ApiResponse` 클래스를 통해 일관된 API 응답 형식을 제공합니다.

## 3. 핵심 기능 (도메인):

*   **재고 관리 (Inventory Management):**
    *   폐기(Discard) 관리: 폐기 요청, 폐기 기록 조회, 폐기량, 폐기 손실, 품목별 폐기율 조회.
    *   가격 정책(Price Policy) 관리: 가격 정책 등록, 업데이트, 리스트 조회.
    *   입고(Inbound) 및 출고(Outbound) 이력 관리: 상세한 입/출고 기록 조회 및 필터링.
    *   재고 현황 조회: 실 판매 가능 재고 리스트, 관리자용 전체 재고 리스트 (과거 기록 포함).
*   **마스터 데이터 관리:** `ItemMasterConsumer`, `ItemMasterMirrorService` 등으로 미루어 볼 때, 품목 마스터 데이터의 동기화 또는 미러링 기능이 포함되어 있습니다.
*   **주문 처리 연동:** `PurchaseOrderConsumer`, `SalesOrderConsumer` 등으로 미루어 구매 주문 및 판매 주문 시스템과의 연동을 통해 재고를 업데이트합니다.
*   **메트릭/모니터링:** Lot 관련 메트릭 조회 기능을 통해 시스템 운영 및 재고 현황 모니터링을 지원합니다.

## 4. 이력서에 어필할 수 있는 주요 역량:

*   **대규모 트래픽 처리 및 분산 시스템 경험:** Spring Boot, Kafka, Redis를 활용한 마이크로서비스 또는 분산 시스템 개발 경험. 특히 Kafka를 통한 이벤트 기반 통신 및 멱등성 처리 경험.
*   **보안 전문가:** Spring Security와 OAuth2 Resource Server를 이용한 API 보안 설계 및 구현, 역할 기반 권한 제어(`ADMIN`, `USER`) 경험.
*   **견고한 API 설계:** RESTful API 원칙 준수, CQRS 패턴 적용, 요청 유효성 검사, 전역 예외 처리 및 표준화된 응답 형식을 통한 안정적인 API 개발 역량.
*   **데이터 관리 및 최적화:** Spring Data JPA, MyBatis를 활용한 관계형 데이터베이스 연동 및 쿼리 최적화, Redis를 활용한 캐싱 또는 세션 관리 경험.
*   **클라우드 환경 이해:** AWS S3 및 MSK IAM Auth 사용 경험을 통해 클라우드 기반 서비스 개발 및 운영에 대한 이해.
*   **도메인 주도 설계 (DDD) 및 비즈니스 로직 구현:** 인벤토리, 폐기, 가격 정책, 입출고 등 복잡한 비즈니스 도메인을 모델링하고 서비스 계층에서 효과적으로 비즈니스 로직을 구현하는 능력.
*   **높은 품질의 코드:** Lombok을 통한 코드 간결화, `ApiResponse`를 통한 응답 표준화, `@Validated`를 통한 입력 검증 등 코드 품질 및 유지보수성 고려.

## 5. 이력서 작성 제안:

1.  **프로젝트 개요:** Golden Harvest Inventory 시스템이 무엇인지 (재고 관리 시스템), 어떤 문제를 해결하는지 명확히 설명합니다.
2.  **담당 역할 및 기여:** 프로젝트에서 본인이 어떤 역할을 담당했으며, 어떤 모듈 또는 기능 개발에 기여했는지 구체적으로 명시합니다. (예: "재고 폐기 관리 API 개발", "Kafka를 이용한 주문 시스템 연동 모듈 구현" 등)
3.  **주요 기능 구현:** 위에서 언급된 핵심 기능 중 본인이 담당했거나 중요하다고 생각하는 부분을 2~3가지 선정하여 설명합니다. 각 기능에 대해 "무엇을(What)", "어떻게(How - 사용 기술/패턴)", "왜(Why - 문제 해결/성과)"를 포함하여 작성합니다.
    *   **예시:** "Kafka를 활용하여 외부 주문 시스템의 변경 이벤트를 비동기적으로 처리하고 재고를 실시간으로 업데이트하는 모듈을 개발하여 시스템 간의 결합도를 낮추고 처리량을 향상시켰습니다."
4.  **기술 스택 상세 설명:** 사용한 주요 기술 스택(Spring Boot, Kafka, Redis, Spring Security 등)을 나열하고, 각 기술을 왜 사용했는지, 어떤 이점을 얻었는지 간략히 설명합니다.
5.  **아키텍처/패턴 적용:** CQRS, 이벤트 기반 아키텍처, RESTful API 설계 등 적용한 아키텍처 패턴을 설명하고, 이를 통해 얻은 이점(확장성, 유지보수성, 성능 등)을 강조합니다.
