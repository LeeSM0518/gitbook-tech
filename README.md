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
* 수신자 클래스는 _**ApplicationListener**_ 인터페이스를 구현해야 한다.

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

* _**ApplicationEventPublisherAware**_ 인터페이스를 발행자 클래스가 구현하는 방식으로도 가능하다.
* Spring Framework 4.2 부터 _**ApplicationEventPublisher**_ 인터페이스는 모든 객체를 이벤트로 허용하는 _**publishEvent(Object event)**_ 메서드에 대한 새로운 오버로드를 제공한다. 그러므로 더 이상 _**ApplicationEvent**_ 클래스를 확장할 필요가 없다.

### 2.3. Listener

수신자를 구현해보자. _**ApplicationListener**_ 인터페이스만 구현하면 된다.

```kotlin
@Component
class CustomSpringEventListener : ApplicationListener<CustomSpringEvent> {
    override fun onApplicationEvent(event: CustomSpringEvent) {
        println("Received spring custom event - ${event.message}")
    }
}
```

* 사용자 정의 이벤트가 제네릭 타입을 통해 매개변수화되어 _**onApplicationEvent()**_ 메서드를 type-safe 하게 구성

## 3. Creating Asynchronous Events

이벤트를 비동기로 처리해야 할 수도 있다.

_**ApplicationEventMulticaster**_ 빈 설정을 통해 비동기로 구성시킬 수 있다.

```kotlin
@Configuration
class AsynchronousSpringEventsConfig {

    @Bean(name = ["applicationEventMulticaster"])
    fun simpleApplicationEventMulticaster(): ApplicationEventMulticaster =
        SimpleApplicationEventMulticaster()
            .apply { setTaskExecutor(SimpleAsyncTaskExecutor()) }
}
```

* 수신자는 별도의 스레드에서 이벤트를 비동기적으로 처리하도록 설정된다.

## 4. Existing Framework Events

Spring의 _**ApplicationContext**_**는 **_**ContextRefreshedEvent, ContextStartedEvent, RequestHandledEvent**_ 등 다양한 이벤트를 발생시킨다.

이러한 이벤트는 개발자가 애플리케이션의 수명 주기와 컨텍스트를 연결하고 필요한 경우 직접 구현한 로직을 추가할 수 있도록 옵션을 제공한다.

```kotlin
@Component
class ContextRefreshedListener : ApplicationListener<ContextRefreshedEvent> {
    override fun onApplicationEvent(event: ContextRefreshedEvent) {
        println("Handling context refreshed event.")
    }
}
```

* Spring 에서 발생하는 이벤트들을 알아보기 위해서는 다음 [링크](https://www.baeldung.com/spring-context-events)를 확인

## 5. Annotation-Driven Event Listener

Spring 4.2부터 이벤트 수신자는 _**ApplicationListener**_ 인터페이스를 구현하는 빈일 필요가 없다.

_**@EventListener**_ 주석을 통해 관리되는 빈의 모든 _public_ 메소드에 등록할 수 있다.

```kotlin
@Component
class AnnotationDrivenEventListener {
    @EventListener
    fun handleContextStart(event: ContextStartedEvent) {
        println("Handling context started event.")
    }
}
```

* 메서드 시그니처는 사용하는 이벤트를 선언한다.
* 기본적으로 리스너는 동기적으로 호출된다.
* _**@Async**_ 주석을 추가하면 쉽게 비동기식으로 만들 수 있다.
  *   비동기로 동작시기키 위해서는 다음과 같은 설정이 필요하다.

      ```kotlin
      @Configuration
      @EnableAsync
      public class SpringAsyncConfig { ... }
      ```

      * 상세한건 다음 [링크](https://www.baeldung.com/spring-async#enable-async-support)를 확인

## 6. Generics Support

이벤트의 내용을 제네릭으로 선언하여 전달하는 것도 가능하다.

### 6.1. Generic Application Event

제네릭 이벤트 타입으로 만들어보자.

```kotlin
class GenericSpringEvent<T>(
    val what: T,
    val success: Boolean
)
```

* _**CustomSpringEvent**_와 다르게 _**GenericSpringEvent**_는 임의의 이벤트를 발행하는 유연성이 있으며 _**ApplicationEvent**_에서 확장할 필요가 없다.

### 6.2. Listener

제네릭 이벤트에 대한 수신자를 만들어보자.

먼저 _**ApplicationListener**_ 인터페이스를 구현하는 수신자를 만들어 보자.

```kotlin
@Component
class GenericSpringEventListener : ApplicationListener<GenericSpringEvent<String>> {
    override fun onApplicationEvent(event: GenericSpringEvent<String>) {
        TODO("Not yet implemented")
    }
}
```

* _**ApplicationListener**_의 제네릭 타입에는 _**ApplicationEvent**_이나 해당 클래스의 자식만 선언할 수 있으므로 컴파일 에러가 발생한다.
* 따라서 _**GenericSpringEvent**_가 _**ApplicationEvent**_를 상속해야만 사용이 가능하다.

상속하지 않고 _**@EventListener**_ 애노테이션에 boolean SpEL 표현식을 정의하여 이벤트 수신자를 조건부로 이벤트를 처리하도록 만드는 것도 가능하다.

```kotlin
@Component
class AnnotationDrivenEventListener {
    @EventListener(condition = "#event.success")
    fun handleSuccessful(event: GenericSpringEvent<String>) {
        println("Handling generic event (conditional).")
    }
}
```

### 6.3. Publisher

이벤트 발행자 클래스는 다음과 같이 만들면 된다.

```kotlin
@Component
class GenericSpringEventPublisher(
    val applicationEventPublisher: ApplicationEventPublisher,
) {
    fun publishEvent(message: String) {
        println("Publishing generic event. ")
        val genericEvent = GenericSpringEvent(message, true)
        applicationEventPublisher.publishEvent(genericEvent)
    }
}
```

만약 @EventListener 애노테이션을 명시한 메서드에서 null이 아닌 값을 반환할 경우 새로운 이벤트를 발행하게 된다.

```kotlin
@Component
class AnnotationDrivenEventListener {
    @EventListener
    fun handleCustomEvent(event: CustomSpringEvent): CustomSpringEvent2 {
        println("Handling custom event.")
        return CustomSpringEvent2("message")
    }

    @EventListener
    fun handleCustomEvent2(event: CustomSpringEvent2) {
        println("Handling custom event2.")
    }
}
```

* 위와 같이 구현되어 있을 경우 CustomSpringEvent가 발행될 경우 handleCustomEvent가 호출되고, handleCustomEvent 메서드는 CustomSprintEvent2를 발행하여 handleCustomEvent2가 그 다음으로 호출된다.

## 7. Transaction-Bound Events



## Reference

{% embed url="https://www.baeldung.com/spring-events" %}
