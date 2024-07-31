---
title: "Annotation & Reflection"
date: 2024-07-31
update: 2024-07-31
tags:
  - Java
  - Interview
---

## Annotation이란 무엇이고 사용사례는 무엇인가요?

`Annotation`은 소스코드에 바인딩된 메타데이터이며, `Annotation`이 코드의 실행에 영향을 주지는 않습니다.

일반적인 사용사례는 먼저 **컴파일러를 위한 정보를 제공합니다**. 이는 `Annotation`을 사용하면 컴파일러가 오류를 감지하거나 경고를 억제할 수 있습니다.
두번째로, 소프트웨어 도구는 컴파일 시간에 `Annotation`을 처리하고 **코드, 구성파일 등을 생성할 수 있습니다**.(e.g. Lombok)
마지막으로, **런타임시** `Annotation`을 검사하여 프로그램의 동작을 정의할 수 있습니다.

## Annotation의 장점은?

먼저, `annotation`은 메타데이터(정보)를 프로그램 요소(e.g. 인스턴스변수, 생성자, 메소드, 클래스등)에 연결하는 데 도움이 됩니다.
또한, 기본적으로 추가 정보를 제공하는 데 사용되므로 `XML`과 `Java 마커 인터페이스`의 대안이 될 수 있습니다.

또한, 코드 단순화와 가독성에도 중요한 역할을 합니다. 

## @Retention 정책 각각의 차이 

- `RetentionPolicy.SOURCE` : 컴파일 중에 삭제합니다. 이러한 Annotation은 컴파일 완료된 후에는 의미가 없으므로, 바이트코드에 기록되지 않습니다. 
- `RetentionPolicy.CLASS` : 클래스 로드 중에 삭제됩니다. 바이트코드 레벨에서 후처리를 수행할 때 유용합니다.
- `RetentionPolicy.RUNTIME` : 삭제되지 않습니다. 즉, 런타임 중에 리플렉션을 위해 이용이 가능합니다.  

## Annotation을 어떻게 런타임 중에 이용하나요?

JVM은 클래스 메타데이터(정보)를 클래스로더에 의해 `Method Area`에 저장합니다. 리플렉션은 이 `Method Area`에 저장된
클래스 메타데이터, 어노테이션 정보를 얻거나 수정하는 데 사용하는 API 입니다. 

즉 `Java Reflection`의 도움으로 특정 어노테이션이 달린 클래스나 특정 클래스의 어노테이션이 달린 메서드를 검색할 수 있습니다.

### 리플렉션은 어떻게 이용하나요?

`java.lang`에서 제공하는 **Class 객체**를 사용하면 리플렉션을 활용할 수 있습니다. 
그런데 Class 생성자는 접근제어자가 `private`이라 직접 생성이 불가능합니다. 하지만 이용할 방법이 있습니다. 

`.class` 파일을 JVM에 로드한 후, JVM은 힙 영역에 `java.lang.Class` 유형의 객체를 생성합니다.
이 Class 객체를 사용하여(e.g. 클래스의 `.class 리터럴`, `getClass()`, forName에 `FQCN전달`) 클래스 수준 정보를 얻을 수 있습니다.

### 리플렉션은 왜 성능이 느릴까요?

리플렉션을 사용할 때 수행하는 모든 단계는 **수행할 때마다 유효성을 검사**해야 합니다. 
예를 들어, 메소드를 호출할 때 대상이 실제로 메소드 선언자의 인스턴스인지, 인수 개수가 올바른지, 각 인수의 유형이 올바른지 등을 확인해야 합니다.
또한, **JVM 최적화에 방해**할 수 있습니다. 이로 인해 성능이 느려질 수 있습니다. 

## 어노테이션 프로세서는 뭔가요?

`Annotation Processor`는 **annotation을** 분석하고 처리하기 위해 Java 컴파일 시간동안 작동하여 코드를 검사하고, 추가 소스파일을 생성하거나, 기존파일을 
수정하는 과정을 말합니다. 
