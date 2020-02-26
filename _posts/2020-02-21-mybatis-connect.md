---
title: "Spring Framework 라이브러리 - 4. MyBatis"
excerpt: "lombok과 HikariCP, MyBatis까지"

categories:
  - Spring
tags:
 - Spring
 - Framework
 - library
 - ORM
 - MyBatis
date: 2020-02-21 11:00
---

# Spring Framework에서 많이 사용하는 라이브러리 3가지 설치하기

> Spring Framework를 처음 시작하면서 하는 여러 설정들은 [github](https://github.com/angelica127/SpringFramwork/)를 참고하도록 한다.

## Spring과 DB 구조

데이터 삽입의 경우

- Spring → MyBatis → HikariCP → OracleDriver(JDBC) → Oracle

데이터 불러오기일 경우

- Oracle → OracleDriver(JDBC) → HikariCP → MyBatis → Spring

1. 자동화된 프로그램을 만들기 위한 DB 구조
1. HikariCP는 속도 향상을 위해 Connection Pool 라이브러리를 사용하는 것
1. MyBatis는 DAO코드를 자동으로 생성해 준다.
1. 인터페이스와 SQL문만 저장해 놓으면 프로그램이 처리되도록 만드는 것

## MyBatis 라이브러리 등록([MyBatis](http://www.mybatis.org/mybatis-3/))

MyBatis는 '**SQL Mapping 프레임워크**'로 __ORM__(Object Relational Mapping, 객체-관계 매핑)의 한 종류이다. MyBtis는 기존의 SQL을 그대로 활용할 수 있다는 장점이 있고, 진입장벽이 낮은 편이어서 JDBC의 대안으로 많이 사용한다. Spring Framework의 특징 중 하나는 다른 Framework들을 배척하는 대신에 다른 Framework들과의 연동을 쉽게 하는 추가적인 Library들이 많다는 것으로, MyBatis 역시 '__mybatis-spring__'이라는 Library를 통해서 쉽게 연동작업을 처리할 수 있다.
<br>

- MyBatis 장점

  1. 자동으로 Connection close() 가능
  1. MyBatis 내부적으로 PreparedStatement 처리
  1. #{prop}와 같이 속성을 지정하면 내부적으로 자동 처리
  1. 리턴 타입을 지정하는 경우 자동으로 객체 생성 및 ResultSet 처리

### 1. pom.xml에 라이브러리 등록

  - dependency 태그로 필요한 라이브러리를 등록한다.

```
<!-- ORM 프로그램 : MyBatis -->
<!-- https://mvnrepository.com/artifact/org.mybatis/mybtis -->
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>3.4.6</version>
</dependency>

<!-- mybatis와 spring을 연동하기 위한 라이브러리 -->
<!-- https://mvnrepository.com/artifact/org.mybatis/mybtis-spring -->
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis-spring</artifactId>
  <version>1.3.2</version>
</dependency>

<!-- spring의 JDBC 관련 라이브러리 -->
<!-- transaction 처리 관련 : commit, rollback -->
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-tx</artifactId>
  <version>${org.springframework-version}</version>
</dependency>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-jdbc</artifactId>
  <version>${org.springframework-version}</version>
</dependency>
```

### 2. root-contexnt.xml에서 설정

MyBatis에서 가장 핵심적인 객체는 SQLSession과 SQLSessionFactory이다. SQLSessionFactory는 내부적으로 SQLSession을 만들어 내는 존재이다. 개발에서는 SQLSession을 통해서 Connection을 생성하거나 원하는 SQL을 전달하고, 결과를 리턴 받는 구조로 작성하게 된다.

스프링에 SQLSessionFactory를 등록하는 작업은 SQLSessionFactoryBean을 이용한다. 패키지를 보면 MyBatis의 패키지가 아니라 mybatis-spring 라이브러리의 클래스임을 알 수 있다.

- root-contexnt.xml 추가 내용

  - root-contexnt.xml의 'NameSpaces' 탭을 확인하여 'mybatis-spring'이 체크되어 있는 지 확인한다.

  - `<mybatis-scan>`이라는 태그를 이용하여 MyBatis가 동작할 때 Mapper를 인식할 수 있도록 root-context.xml에 추가하기 위함

    - `<mybatis-spring:scan base-package="org.zerock"/>`

  - `<mybatis-spring:scan base-package="org.zerock"/>` 태그의 'base-package' 속성은 지정된 패키지의 모든 MyBatis와 관련된 것을 찾아서 처리한다.

![mybatis](/assets/images/mybatis.png)

  - 코드

```
<!-- MyBatis - DAO코드 자동 생성 -->
<!-- HikariCP를 MyBatis에 넣는 것 -->
<!-- Mapper Interface(메서드 이름) - Mapper xml(SQL) -->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
  <property name="dataSource" ref="dataSource"></property>
</bean>

<!-- 프로젝트 운영을 하는 상위 패키지로 변경해서 사용한다. -->
<!-- dao(persitence) - 자동으로 만들어 주기 위한 위치 -->
<mybatis-spring:scan base-package="org.zerock"/>

<!-- 자동으로 스캔하도록 하는 설정: 여러 개 설정 가능 , org.zerock까지만 등록하면 그 밑으로 다 자동 스캔 -->
<context:component-scan base-package="org.zerock"></context:component-scan>
```

### 3. Spring에서 Mybatis 사용하기

Mapper를 xml과 Interface + Annotation의 형태로 작성한다. Mapper는 SQL과 그에 대한 처리를 지정하는 역할을 한다.

여기에서는 Annotation을 이용하지 않고 '__Interface와 XML__'을 이용하여 '__Mapper를 작성__'한다. Interface에는 Method를 선언하고 Mapper.xml 파일에 SQL문을 작성한다.

1) Mapper Interface를 만든다.

- Interface에는 Method를 정의하고, 구현하지 않는다.

![interface](/assets/images/mybatis2.png)

2) Mapper.xml을 작성한다.

- Mapper.xml에는 MyBatis의 XML Mapper에서 이용하는 태그에 대한 설정이 필요하다.

[mybatis.org 참조 - '시작하기 > 매핑된 SQL 구문 살펴보기 탭'](https://mybatis.org/mybatis-3/ko/getting-started.html)

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
    PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
```

- MyBatis를 이용해서 SQL을 처리할 때 Annotation을 이용하는 방식이 편리하지만, SQL이 복잡하거나 길어지는 경우에는 '__XML__'을 이용하는 방식을 더 선호한다. mybatis-spring의 경우 '__Mapper Interface와 XML__'을 동시에 이용할 수 있다.

![xml](/assets/images/mybatis3.png)

XML을 작성해서 사용할 때에는 '**XML의 파일 위치**'와 XML 파일에 지정하는 '**namespace 속성**'이 중요하다.

XML 파일 위치의 경우 Mapper Interface가 있는 곳에 같이 작성하거나 다음 그림처럼 src/main/resources 구조에 XML을 저장할 폴더를 생성할 수 있다. XML 파일의 이름은 가능하다면 '**Mapper Inteface**'와 '**같은 이름**'을 이용하는 것이 가독성을 높여준다.

XML Mapper를 이용할 때 신경써야 하는 부분은 `<mapper>`태그의 '**namespace 속성값**'이다. MyBatis는 Mapper Interface와 XML을 Interface의 이름과 namespace 속성값을 가지고 판단한다. 위와 같이 org.zerock.mapper.BoardMapper interface가 존재하고, XML의 `<mapper namespace='org.zerock.mapper.BoardMapper'>`와 같이 동일한 이름이 존재하면, 이를 병합해서 처리한다.

`<select>`태그의 id 속성의 값은 '**Method의 이름**'과 동일하게 맞춰야 한다. `<select>`태그의 경우 resultType 속성을 가지는데 이 값은 interface에 선언된 '**Method의 return type**'과 동일하게 작성한다.
