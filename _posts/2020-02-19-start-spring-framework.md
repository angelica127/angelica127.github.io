---
title: "Spring Framework 라이브러리 - lombok"
excerpt: "lombok과 HikariCP, MyBatis까지"

categories:
  - Blog
tags:
 - Spring
 - Framework
 - library
 - lombok
last_modified_at: 2020-02-19T22:32:00-05:00
---

## Spring Framework에서 많이 사용하는 라이브러리 3가지 설치하기

> Spring Framework를 처음 시작하면서 하는 여러 설정들은 [github](https://github.com/angelica127/SpringFramwork/)를 참고하도록 한다.

### lombok 라이브러리 설치

> getter, setter, toString(), 생성자 등을 자동으로 생성해 주는 라이브러리

이클립스와 스프링 플러그인 만으로도 스프링 개발은 가능하지만, Lombok을 이용하면 Java 개발 시 자주 사용하는 getter, setter, toString(), 생성자 들을 자동으로 생성해 주므로 클래스를 설계할 때 유용하다.

Lombok은 다른 jar 파일과 달리 프로젝트의 코드에서만 사용되는 것이 아니라 Eclipse 에디터 내에서도 사용되어야 하기 때문에 별도로 설치한다.

[Annotation](#Annotation) 설명

### 1. Lombok jar 파일 다운로드

[lombok](https://projectlombok.org/download) 홈페이지의 Downlod 탭에서 jar 파일을 다운로드 받는다.

![lombok home](/assets/images/lombok.png)

### 2. 명령 프롬프트에서 다운받은 lombok.jar 파일이 있는 경로로 이동한다.

Ex) D:\dwon\api

### 3. 명령 프롬프트에서 java -jar lombok.jar 명령어를 실행한다.

![lombok2](/assets/images/lombok2.png)

### 4. 사진과 같은 창이 나오고 STS나 Eclipse의 설치 경로를 자동으로 찾는다.

![lombok3](/assets/images/lombok3.png)

이때 경로를 찾지 못한다면 Specify location 버튼을 눌러 경로를 직접 지정해준다.

- 경로를 찾은 화면

![sts dir](/assets/images/lombok4.png)

경로를 찾은 후 Install / Update 버튼을 눌러 Install을 진행한다.

- Install 완료

![설치 완료](/assets/images/lombok5.png)

### 5. Install이 완료된 후 STS나 Eclipse의 실행위치에 lombok.jar가 추가되었는지 확인한다.

- lombok.jar 추가된 것 확인

![설치 확인](/assets/images/lombok6.png)

### 6. STS에 만든 Spring Project의 pom.xml에 lombok.jar를 등록한다.

> `<dependencies>` 태그 밑에 `<dependency>`태그로 등록해야 한다.

```
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-test</artifactId>
  <version>${org.springframework-version}</version>
</dependency>
<dependency>
  <groupId>org.projectlombok</groupId>
  <artifactId>lombok</artifactId>
  <version>1.18.0</version>
  <scope>provided</scope>
</dependency>
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.12</version>
  <scope>test</scope>
</dependency>   
```


  1. test를 위해 springframework의 test 라이브러리를 등록하고
  1. lombok 라이브러리를 버전에 맞게 등록한다.
  1. Test를 위한 junit 라이브러리를 Up version 시킨다.

### 7. pom.xml에 라이브러리를 정의한 후 STS를 재 실행한다.

### 8. Lombok을 이용한 의존성(DI) 주입

스프링에서는 생성자를 이용한 의존성 주입과 setter 메서드를 이용한 주입으로 의존성을 주입한다. 설정 방식은 주로 XML이나 Annotation을 이용해서 처리한다.

Lombok을 이용해서 setter 메서드를 자동으로 구현되도록 하기 위해 component를 scan하기 위한 기본 패키지를 설정해야 하며, 이를 위해서 `'root-context.xml'`의 `'NameSpaces'` 탭에서 `'context'`라는 항목을 체크한다.

[![root-context](/assets/images/lombok7.png)](/assets/images/lombok7.png)
[![root-context](/assets/images/lombok7-1.png)](/assets/images/lombok7-1.png)

### Annotation

@Data - setter, getter, toString(), 생성자를 자동으로 만들기

@Setter - setter를 이용한 DI 적용

- Setter를 이용해서 DI를 적용할 때

    - @Autowired - Spring annotation

    - @Inject - Java annotation - service or dao

- up to JDK 1.7

  - @Setter(on Method = @__({@Autowired}))

- from JDK 1.8

  - @Setter(onMethod_ = {@Autowired})
