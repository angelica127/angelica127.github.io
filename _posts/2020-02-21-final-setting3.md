---
title: "설정 정리 - root-context.xml과 servlet-context.xml"
excerpt: "프로젝트 시작 전 최종 정리"

categories:
  - Spring
tags:
 - Spring
 - Framework
 - root-context.xml
 - servlet-context.xml
date: 2020-02-21 18:05
---

## Spring Framework 시작 전 설정 정리

> Spring을 이용한 Project를 시작하기 전, 여기까지 잔행했던 설정 정리

Spring Framework를 시작하면서 Lombok, MyBatis 등의 Library를 설치하면서 pom.xml과 servlet-context.xml, root-context.xml 등을 여러 번 수정했다. 여태까지 수정했던 내용을 정리해서 올려 두려고 한다.

- root-context.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mybatis-spring="http://mybatis.org/schema/mybatis-spring"
	xsi:schemaLocation="http://mybatis.org/schema/mybatis-spring http://mybatis.org/schema/mybatis-spring-1.2.xsd
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd">

	<!-- Root Context: defines shared resources visible to all other web components -->
	<!-- beans 태그 안에 bean tag를 넣을 수있다. -->
	<!-- <bean> ==> 객체를 선언하는 것 : 자동 생성 (선언을 하는 순간 생성한다.) -->
	<bean id="hikariConfig" class="com.zaxxer.hikari.HikariConfig">
		<!-- 바로 오라클로 가는 거 기본 타입
		<property name="driverClassName" value="oracle.jdbc.driver.OracleDriver"></property>
		<property name="jdbcUrl" value="jdbc:oracle:thin:@402-oracle:1521:orcl"></property>
		-->
		<property name="driverClassName" value="net.sf.log4jdbc.sql.jdbcapi.DriverSpy"></property>

		<!-- DB 접속 정보를 설치한 것에 맞게 변경해 줘야 한다. -> 만약에 11g xe를 설치한 경우 -->
		<property name="jdbcUrl" value="jdbc:log4jdbc:oracle:thin:@402-oracle:1521:orcl"></property>

		<property name="username" value="c##java06"></property>
		<property name="password" value="java06"></property>
	</bean>

	<!-- HikariCP configuration -->
	<!-- 서버가 시작되면 일정 개수의 Connection을 만들어서 DBCP(DataBaseConnectionPool)에 넣어둔다. -->
	<bean id="dataSource" class="com.zaxxer.hikari.HikariDataSource"
		destroy-method="close">
		<!-- hikariConfig를 참조한다. 생성자의 파라미터를 뜻하는 태그(constructor - 생성자) -->
		<constructor-arg ref="hikariConfig" />
	</bean>

	<!-- MyBatis - DAO코드 자동 생성 -->
	<!-- HikariCP를 MyBatis에 넣는 것 -->
	<!-- Mapper Interface(메서드 이름) - Mapper xml(SQL) -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource"></property>
	</bean>

	<!-- 프로젝트 운영을 하는 상위 패키지로 변경해서 사용한다. -->
	<!-- dao(persitence) - 자동으로 만들어 주기 위한 위치 -->
	<mybatis-spring:scan base-package="org.zerock"/>

	<context:component-scan base-package="org.zerock"></context:component-scan>
</beans>
```

- servlet-context.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/mvc"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:beans="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

	<!-- DispatcherServlet Context: defines this servlet's request-processing infrastructure -->

	<!-- Enables the Spring MVC @Controller programming model -->
	<annotation-driven />

	<!-- Handles HTTP GET requests for /resources/** by efficiently serving up static resources in the ${webappRoot}/resources directory -->
	<!-- 프로젝트 운영을 하는 리소스 위치로 변경해서 사용한다. -->
	<resources mapping="/resources/**" location="/resources/" />

	<!-- 기본위치 : webapp -->
	<resources mapping="/js/**" location="/js/" />
	<resources mapping="/css/**" location="/css/" />
	<!-- file upload를 위한 설정 -->
	<resources mapping="/upload/**" location="/upload/" />

	<!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<beans:property name="prefix" value="/WEB-INF/views/" />
		<beans:property name="suffix" value=".jsp" />
	</beans:bean>

	<!-- 자동으로 스캔하도록 하는 설정: 여러 개 설정 가능 , org.zerock까지만 등록하면 그 밑으로 다 자동 스캔 -->
	<context:component-scan base-package="org.zerock" />

</beans:beans>
```
