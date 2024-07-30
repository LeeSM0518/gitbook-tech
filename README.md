---
cover: .gitbook/assets/Frame 81.png
coverY: 0
---

# @Transactional

## 1. 개요

Spring 트랜잭션을 올바르게 구성하는 방법과 _@Transactional_ 을 잘 사용하는 방법 및 잘못된 사용 방법에 대해 알아보자.

## 2. 트랜잭션 설정

_@Configuration_ 클래스에 _**@EnableTransactionManagement**_ 를 사용하여 트랜잭션을 활성화 할 수 있다.

```kotlin
@Configuration
@EnableTransactionManagement
class PersistenceJPAConfig {
    
    @Bean
    fun entityManagerFactory(): LocalContainerEntityManagerFactoryBean {
        // ...
    }
    
    @Bean
    fun transactionManager(): PlatformTransactionManager {
        val transactionManager = JpaTransactionManager()
        transactionManager.setEntityManager(entityManagerFactory().getObject())
        return transactionManager
    }
}
```

* spring-data-\* 또는 spring-tx 의존이 있는 경우에는 기본적으로 트랜잭션이 활성화된다.

## 3. _@Transactional_ 어노테이션

클래스나 메서드에 _@Transactional_ 어노테이션을 정의할 수 있다.

```kotlin
@Service
@Transactional
class FooService {
	// ...
}
```

* 다음과 같은 설정도 지원한다.
  * _**Propagation Type** :_ 전파 유형
  * _**Isolation Level** :_ 격리 수준
  * _**Timeout**_ : 시간 초과
  * _**readOnly**_ : 읽기 전용
  * _**Rollback**_ : 롤백 규칙

## 4. 잘못된 사용

### 4.1. 트랜잭션과 프록시

Spring은 _@Transactional_ 이 달린 모든 클래스에 대한 프록시를 생성한다. Spring은 프록시를 활용해 트랜잭션을 시작하고 커밋하기 위해 메서드 실행 전후에 트랜잭션 로직을 실행한다.
