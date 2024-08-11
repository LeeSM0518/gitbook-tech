---
description: Pagination in Spring Webflux and Spring Data Reactive
cover: ../.gitbook/assets/Frame 81.png
coverY: 0
---

# Pagination

## 1. 개요

***

본 글에는 다음 세 가지에 대해 작성되어 있다.

1. _데이터 조회에 있어서 Pagination의 중요성_
2. _Pagination 사용에 있어서 Spring Data Reactive와 Spring Data의 차이점_
3. _Pagination 구현 예제_



## 2. Pagination의 중요성

***

Pagination은 대용량 데이터를 응답할 때 필수적이다. Pagination은 **대용량 데이터를 작은 덩어리인 페이지 단위로 나누어 데이터를 효율적으로 조회하고 응답**할 수 있도록 한다.



## 3. Spring Data와 Spring Data Reactive의 차이

***

**Spring Data**는 Java 애플리케이션의 **데이터 접근을 단순화하고 향상시키는 것을 목표**로 하는 Spring Framework의 프로젝트 중 하나이다. Spring Data는 보일러 플레이트 코드를 줄이고 모범 사례를 촉진하여 코드를 단순화하는 공통 추상화 및 기능을 제공한다.

[Spring Data Pagination example](https://www.baeldung.com/spring-data-jpa-pagination-sorting)에서 설명했듯이 _page, size, sort_ 로 구성된 [_PageRequest_](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/domain/PageRequest.html)를 사용하여 페이지를 구성하고 요청할 수 있다. Spring Data에는 pagination과 sorting 추상화를 사용하여 엔티티 검색 메서드를 제공하는 [_PagingAndSortingRepository_](https://docs.spring.io/spring-data/data-commons/docs/current/api/org/springframework/data/repository/PagingAndSortingRepository.html) 인터페이스가 정의되어 있다. 해당 Repository의 메서드에서 [_Page_](https://docs.spring.io/spring-data/data-commons/docs/current/api/org/springframework/data/domain/Page.html) 정보를 반환하기 위해 [_Pageable_](https://docs.spring.io/spring-data/data-commons/docs/current/api/org/springframework/data/domain/Pageable.html)과 [_Sort_](https://docs.spring.io/spring-data/data-commons/docs/current/api/org/springframework/data/domain/Sort.html) 객체를 사용할 수 있다. _Page_ 객체에는 내부적으로 쿼리를 실행할 때 사용되는 _totalElements_와 _totalPages_ 속성이 포함되어 있다. 이 정보들은 다음 페이지를 요청할 때 사용될 수 있다.

반면에, **Spring Data Reactive**는 **pagination을 완전하게 지원하지는 않는다.** 이러한 이유는 Spring Reactive는 비동기 논블로킹을 지원하므로 **특정 페이지 크기에 해당하는 모든 데이터가 반환될 때까지 블로킹이 되면 효율적이지 않기 때문**이다. 그러나 Spring Data Reacrive는 여전히 _Pageable_을 지원한다. _PageRequest_ 객체를 사용하여 특정 데이터의 청크와 데이터의 개수를 조회하도록 구성할 수 있다.&#x20;

페이지와 레코드에 대한 메타데이터가 포함된 Spring Data를 사용할 때 Page 대신 Flux로 구성된 응답을 받을 수 있다.



## 4. 예제

### 4.1. WebFlux와 Data Reactive를 활용한 Pagination 구현





