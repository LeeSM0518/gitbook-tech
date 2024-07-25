---
cover: .gitbook/assets/Frame 81.png
coverY: 0
---

# Spring Events

## 1. Overview

_**ApplicationContext**_를 활용하여 이벤트를 발행할 수 있다.

이벤트 발행을 활용할 경우 다음 항목들을 따라야 한다.

* Spring Framework 4.2 이전 버전을 사용하는 경우에는 이벤트 클래스에서 _**ApplicationEvent**_ 클래스를 상속받아야 한다. (4.2 버전부터는 상속받을 필요 없음)
* 발행자 클래스에 _**ApplicationEventPublisher**_ 객체를 주입해야 한다.
* 구독자 클래스는 _**ApplicationListener**_ 인터페이스를 구현해야 한다.

## 2. Custom Event

Spring을 사용하여 동기적으로 사용자 정의 이벤트를 생성하고 발행할 수 있다.

이로 인해 구독자의 로직이 발행자의 트랜잭션에 속하여 동작할 수 있는 등의 여러 장점이 있다.

### 2.1. Simple Application Event

간단한 이벤트 클래스를 구현해보자.

```kotlin
class CustomSpringEvent(
    source: Any,
    val message: String,
) : ApplicationEvent(source)
```

### 2.2. Publisher

이벤트 발행자 클래스를 구현해보자.

_**ApplicationEventPublisher**_ 를 주입하고 _**publishEvent()**_ 를 호출하여 이벤트를 발행할 수 있다.

```kotlin
@Component
class CustomSpringEventPublisher(
    val applicationEventPublisher: ApplicationEventPublisher,
) {
    fun publishCustomEvent(message: String) {
        println("Publishing custom event.")
        val customSpringEvent = CustomSpringEvent(this, message)
        applicationEventPublisher.publishEvent(customSpringEvent)
    }
}
```

* _ApplicationEventPublisherAware_ 인터페이스를 발행자 클래스가 구현하는 방식으로도 가능하다.
* Spring Framework 4.2 부터 _ApplicationEventPublisher_ 인터페이스는 모든 객체를 이벤트로 허용하는 _publishEvent(Object event)_ 메서드에 대한 새로운 오버로드를 제공한다. 그러므로 더 이상 ApplicationEvent 클래스를 확장할 필요가 없다.

### 2.3. Listener

구독자를 구현해보자. _ApplicationListener_ 인터페이스만 구현하면 된다.

```kotlin
@Component
class CustomSpringEventListener : ApplicationListener<CustomSpringEvent> {
    override fun onApplicationEvent(event: CustomSpringEvent) {
        println("Received spring custom event - ${event.message}")
    }
}
```

## Reference

{% embed url="https://www.baeldung.com/spring-events" %}
