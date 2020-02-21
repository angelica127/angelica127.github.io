---
title: "Spring Framework Project 시작하기 정리"
excerpt: "프로젝트 시작 최종 정리"

categories:
  - Blog
tags:
 - Spring
 - Framework
 - pom.xml
date: 2020-02-21 17:55
---

## Spring Framework 시작 정리

포스트 중...

1. 개발 환경
- 자바 : oracle.com
- STS : IDE(통합개발툴)
 > spring 3.xx -> 재시작
- Oracle : oracle.com : 12c -> 11xe / 18xe
 > 사용자정의 11g : java00,
           12c이상 : c##java00
- SQL Developer : oracle.com
 > 사용자 생성/권한
 > 사용자로 접속
 > board table 생성

1_1. Rombok 라이브러리 셋팅 -> 카페
  --> 만약에 라이브러리 등록이 제대로 되지 않으면 getter와 setter에 오류가 있다고 나온다.
 => DTO에 getter, setter, toString(), 생성자를 만들어 주면 된다.
 => @Setter -> DI 적용을 @Autowired나 @Inject로 변경

2. 프로젝트 생성
- 개발 환경 Spring변경
 > new > Spring Legacy Project
   > Spring MVC Project
	org.zerock.main.controller
- 오라클 드라이버 복사 : /WEB-INF/lib
 >프로젝트 안에 ojdbc8.jar(oracle12c)
- pom.xml의 property 이하 복사 붙여 넣기 -> 다운로드 -> 빌드 -> 확인(기다림)
- Maven > Update Project
- 경고 : log4j.xml -> 카페 참조 수정

3. 프로젝트 설정
- web.xml -> 이전 것을 복사
- root-context.xml :DB 관련 설정-내컴
 > 복사
- servlet-context.xml : web 관련설정
 > 복사
- jdbc-log4j2를 사용하기 위해서
 > log4jdbc.log4j2.properties 복사
 > 위에 파일이 없으면 root-context.xml에서 기본 정보를 사용하도록 한다.

4. tomcat 서버 만들기
 > tomcat 다운로드 -> 설치
   >> http://tomcat.apache.org/ - 9.0
 > sts - 서버 생성 -> 설정

5. 코드 작성(board)
 > src/main/java
org.zerock.board.controller.BoardController
org.zerock.board.service.BoardService
org.zerock.board.dto.BoardDTO
org.zerock.board.mapper.BoardMapper
 > src/main/resource
org/zerock/board/mapper/BoardMapper.xml
 -> mapper.xml을 복사 -> 수정해서 사용
