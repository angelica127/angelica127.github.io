---
title: "Spring Framework 시작하기"
excerpt: "Spring Framewokr 처음부터 "

categories:
  - Blog
tags:
 - Spring
 - Framework
last_modified_at: 2020-02-11T23:32:00-05:00
---

## These are the resources for studying Spring Framwork.

## Contents
* Development Environment
* Start Spring Project
* Change Java Version
* Connect with Server

## Development Environment

1. JAVA JDK 설치: JDK 1.8
- `JDK와 JRE의 차이점`
  - `JDK: javac(컴파일러)가 포함되어 있다.`
  - `JRE: javac를 포함하고 있지 않다`

- Server는 우리가 사용할 WAS인 Apache Tomcat에 맞기고 Java SE 버전을 사용한다.

    - 설치경로: [Oracle.com](https://www.oracle.com/technetwork/java/javase/downloads/index.html)

- 설치는 모드 Default로 설치한다.

- 설치 후 환경변수 설정(JAVA_HOME) - 환경변수 설정은 개발 후 운영을 위한 것이다.
    - `JAVA_HOME = JAVA가 설치된 폴더 경로`
    - `Java가 설치된 폴더(JAVA_HOME) + bin = %JAVA_HOME%\bin`

- CLASSPATH 설정
  - `.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar;`

- 환경변수 설정 확인: windows - 명령 프롬프트
  - path
  - set JAVA_HOME
    - `JAVA_HOME=C:\Program Files\Java\jdk1.8.0_221`
  - set CLASSPATH
    - `CLASSPATH=.;C:\Program Files\Java\jdk1.8.0_221\lib\tools.jar;`

2. STS 설치 (IDE) - [STS](https://www.spring.io/tools)

`스프링 프레임워크를 지원하는 이클립스 (이클립스에서 플러그인을 설치해서 사용해도 된다.)`

`STS는 Maven을 통해 라이브러리를 관리한다.`

- 압출 파일을 해제하는데 알집을 사용하는 경우 정상적으로 압축이 안 풀릴수도 있다.(파일명 혹은 폴더명이 긴 경우가 이에 해당한다.)
  - [7zip](https://www.7-zip.org/) 혹은 [반디집](https://kr.bandisoft.com/bandizip/) 등을 이용하여 압축 파일을 풀어주자

- contents.zip에 실행파일이 들어있다. 압축을 해제한 후 안에 들어 있는 폴더를 C 드라이브로 옮겨준다.

- 작업 파일 한글 인코딩 : 기본 EUC-KR을 UTF-8로 수정

  - SpringToolSuite4.ini을 워드패드로 오픈
  - 맨 마지막에 `-Dfile.encoding=utf-8` 한 줄 추가한다.
  - eclipse는 eclipse.ini 파일을 수정

3. [Apache Tomcat](https://tomcat.apache.org/) 설치 및 연동
  - zip 파일 다운 후 압축을 해제하고 C 드라이브로 복사해 넣는다.

  - path 설정: CATALINA_HOME

    - `CATALINA_HOME = Tomcat 실행 폴더`
    - `CATALINA_HOME + bin =  = %CATALINA_HOME%\bin;`
    - ` set CATALINA_HOME = CATALINA_HOME=C:\tomcat 9`

  - path 설정 후 명령 프롬프트에서 `startup` 명령어를 실행하여 tomcat이 정상적으로 실행되는지 확인한다.

    - Oracle XE 버전을 설치 후 정상적으로 tomcat이 실행되지 않는 경우

      - OracleXE가 웹 서비스 해주는 port와 Tomcat이 실행되는 기본 port번호가 8080으로 겹치기 때문에 충돌이 일어난다.
      - 수동으로 Tomcat의 port번호를 바꾸기 위해서는 tomcat 폴더의 conf\server.xml 파일에서 port 번호를 수정한다.

## Start Spring Project
- STS를 실행 후 `Eclipse Market Place`에서 `Spring Framework 3.XXX`를 `Plug-in`해야 한다. Spring Framewor를 Plugin 해야 Spring 환경을 사용할 수 있다.

  1. Help - Eclipse Market Place
  1. Spring 검색 후 ` Spring Tools 3 Add-On for Spring Tools xxxx`설치

<center><img src="/assets/images/1.png"></center>

<div style="width: 100%">
<center>
<span style="width: 35%;"><img src="/assets/images/2.png"></span>
<span style="width: 35%;"><img src="/assets/images/3.png"></span>
</center>
</div>


## Change Java Version


## Connect with Server
