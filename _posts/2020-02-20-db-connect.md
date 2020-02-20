---
title: "Spring Framework 라이브러리 - OJDBC"
excerpt: "Spring과 DB 연결"

categories:
  - Blog
tags:
 - Spring
 - Framework
 - library
 - oracle
 - Database
last_modified_at: 2020-02-20T11:32:00-05:00
---

## Spring Framework와 Oracle Database 연동

> HikariCP와 MyBatis를 연동하기 전에 Oracle Databse 연동하기

### Spring과 DB 구조

데이터 삽입의 경우

- Spring → MyBatis → HikariCP → OracleDriver(JDBC) → Oracle

데이터 불러오기일 경우

- Oracle → OracleDriver(JDBC) → HikariCP → MyBatis → Spring

1. 자동화된 프로그램을 만들기 위한 DB 구조
1. HikariCP는 속도 향상을 위해 Connection Pool 라이브러리를 사용하는 것
1. MyBatis는 DAO코드를 자동으로 생성해 준다.
1. 인터페이스와 SQL문만 저장해 놓으면 프로그램이 처리되도록 만드는 것

### 라이브러리 등록

> jar를 내 컴퓨터에 다운 받고 프로젝트 내에 lib 폴더를 만들어 복사한 후 pom.xml에 등록하는 방법을 이용
>> Oracle의 버전 확인 : select * from v$version;

### 1. Oracle ojdbc.jar 찾기

> Oracle 버전을 확인하고, 버전에 맞는 jdbc.jar를 사용한다.

1. sqldeveloper 실행 폴더의 jdbc 폴더 아래 lib 폴더 확인

1. Oracle 설치 시 지정한 Base 폴더(app 폴더) 확인

    - Ex) D:\app\사용자\virtual\product\12.2.0\dbhome_1\jdbc\lib

![ojdbc](/assets/images/ojdbc.png)

### 2. ojdbc.jar를 프로젝트로 복사

프로젝트의 적당한 위치에 'lib'폴더를 만든 후 ojdbc.jar를 복사한다.

![ojdbc](/assets/images/ojdbc2.png)

### 3. pom.xml에 라이브러리 등록

- dependencies tag 안에 dependency tag로 등록한다.

```
<dependency>
  <groupId> oracle </groupId>
  <artifactId>ojdbc8</artifactId>
  <version>12.2.0</version>
  <scope>system</scope>
  <systemPath>${project.basedir}/src/main/webapp/WEB-INF/lib/ojdbc8.jar</systemPath>
</dependency>
```

- systemPathsms basedir과 함께 ojdbc.jar를 복사한 경로이다.

### 4. Maven Dependencies에 등록이 되었는 지 확인한다.

![ojdbc](/assets/images/ojdbc3.png)
