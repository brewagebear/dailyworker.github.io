---
title: Java EE Servlet 입문
date: 2019-07-08 18:00:00 +0900
tags: 
- Java
- Servlet
- WebApplication
---

## 1. 서블릿(Servlet) 이란?
서블릿을 만드는 목적? -> 클라이언트에 서비스 하기 위해서!!
그렇다면 서버에 서블릿이 준비되어 있어야 하는데 어떻게 준비를 할 수 있을까?

## 2. 웹 어플리케이션 접근
서블릿은 웹서버에서 서비스되는 페이지이다. 
그래서 서블릿을 개발하였으면, 해당 서블릿 실행 파일을 웹서버에 올려두어야 한다.
그렇다면? 위치는 어디이며, 클라이언트는 어떻게 서블릿에 접근하는가?

	+ 클라이언트
	클라이언트는 웹서버에 서비스 요청할 때, URL 정보를 보낸다. 
        클라이언트가 요청한 문서를 찾아가고자 URL 정보를 활용하는데, URL 정보의 뜻은 다음과 같다. 
	> http://127.0.0.1:8080/top-ing/index.jsp
	이라고 나타내는 URL이 있으면, 8080까지는 웹서버의 포트와 아이피 그 뒤에 top-ing은 웹 어플리케이션을 뜻하며, index.jsp는 클라이언트가 요청한 최종 문서정보이다.

	+ 웹 어플리케이션
	서비스는 서버에서 어플리케이션 단위로 이루어진다.
	웹서버마다 시작될 때 자동으로 어플리케이션으로 인식하여 서비스를 올려주는 디렉터리가 존재하는데, 아파치 톰캣(Apache Tomcat)은 톰캣이 설치한 디렉터리 하위의 webapps 디렉터리에 있는 하위 디렉터리 또는 war 파일을 하나의 어플리케이션으로 인식한다. 

	+ 웹 어플리케이션 구조
	반드시 모든 웹 어플리케이션이 공통으로 가져야 하는 디렉터리와 파일이 존재한다.
		1. WEB-INF 디렉토리
		웹 어플리케이션에서 서비스하려는 클래스 파일이 있다면 WEB-INF/classes 
		클래스 파일들이 jar로 압축되어 있다면 WEB-INF/lib에 존재해야한다.
		이는, WAS를 구성하는 어플리케이션 서버들이 자동으로 인식하게끔 하기위함이다.
		2. web.xml
		web.xml은 WEB-INF 안에 있어야 한다. 
	
## 3. 서블릿 구현
### 서블릿 클래스 간의 관계
		+ 서블릿을 구현할 때 반드시 상속해야되는 클래스가 존재 (HttpServlet)
		-> 웹상에서 클라이언트 요청이 있을 때 해당 서블릿 실행하는 조건들이 포함되어있음.
	  HttpServlet은 GenericServlet을 상속받고, Seralizable, Servlet, ServletConfig 클래스를 상속받고 있다.
			1. Servlet 인터페이스
			Servlet 인터페이스는 서블릿 프로그램을 개발할 때, 반드시 구현해야 하는 메소드를 선언하고 있는 인터페이스이다. 
				 + 메소드
					1. init()
					2. service()
					3. destroy()
					4. getServletConfig()
					5. getServletInfo()
				이 각각의 메소드들은 서블릿  실행의 생명주기와 연관되어 있다.
			2. GenericServlet 클래스 
			Servlet 인터페이스를 상속하여, 클라이언트-서버 환경에서 서버단의 어플리케이션으로서 필요한 기능을 구현한 추상클래스이다. 
			3. HttpServlet 클래스
			GenericServlet 클래스를 상속하여 service() 메소드를 재정의함으로써 HTTP 프로토콜에 알맞은 동작을 수행하도록 구현한 클래스이다. 
			**즉, HTTP 프로토콜 기반으로 브라우저로부터 요청을 전달받아서 처리하도록 하는 클래스이다.**

### 서블릿 작성
이클립스 혹은 인텔리J를 통하여, 웹어플리케이션 프로젝트를 생성 후 아래와 같이 클래스를 작성하여, 서블리쇼을 작성할 수가 있다.
```java
package com.top-ing.study;
import javax.servlet.http.*;

public class FirstServlet extends HttpServlet {

}
```

여기서 FirstServlet이 HttpServlet을 상속받는 것을 확인할 수가 있는데, 웹상에서 HTTP 프로토콜을 이용해 서비스를 처리하기 위해 반드시 HttpServlet을 상속 받아야한다.

### 서블릿 실행 순서
	+ IoC(제어역전, Inversion of Control)
	서블릿의 실행 순서를 이해하기 위해서는 IoC 개념을 알아야한다. 
	Java SE 프로그램은 개발자가 main() 메소드 안에 구현한 순서대로 실행된다.
	**즉, 프로그램이 실행되는 순서를 개발자가 제어한다.**
	**하지만, Java EE 기반 프로그램은 실행의 흐름을 개발자가 제어하는 것이 아니라 컨테이너가 제어한다.**
	**이처럼 개발자가 아닌 제 3자가 프로그램의 실행 흐름을 제어하는 것을 IoC라 한다.** 여기에는 서블릿도 속하며, 따라서 Java EE 기반 프로그램을 개발할 때는 먼저 어프리케이션 컨테이너들이 프로그램을 어떤 순서로 동작시키는지 알고 해당 순서에 맞게 개발해야 된다. 

[image:73E9AEC0-8EA8-4758-8EE3-67A2DABD5AA8-75935-00009970E5FED1BD/43ED5269-D354-4BD3-9862-A480CBDA83E2.png]


위의 그림은 전반적인 서블릿 실행 순서를 나타낸다. 

	1. 클라이언트로부터 처리 요청받음
	클라이언특가 웹 브라우저를 통해 요청을 보내면, 웹서버는 이를 받아서 요청정보의 
	헤더 안에 있는 URI를 분석 -> 요청받은 페이지가 서블릿이면, 서블릿 컨테이너에 처
	리를 넘긴다. 
	2. 최초의 요청 여부 판단
	서블릿 컨테이너는 현재 실행할 서블릿이 최초의 요청인지 판단
	실행할 서블릿 객체가 메모리에 없으면  -> 최초 요청
	최초 요청이라면? 처음 요청할 때 딱 한번만 실행 
	아니라면? service() 수행
	3. 서블릿 객체 생성
	최초 요청이라면 서블릿을 메모리에 로딩하고, 객체를 생성한다.
	서블릿은 최초 요청이 들어왔을 때 한 번만 객체를 생성하고, 이때 생성된 객체를 
	계속 사용한다. 
	4. init() 메소드 실행
	init()은 서블릿 객체 생성 후 호출되는 메소드로서, 주로 서블릿 객체의 초기화 작업
	이  구현되어 있다. GenericServlet 클래스에 구현되어 있으므로, 구현된 내용을 바
	꾸고 싶다면,  init()을 오버라이딩하면 된다.
	5. service() 메소드 실행 
	service() 메소드는 실행하는 서블릿의 요청 순서에 상관없이 클라이언트의 요청이 	
	있을 때마다 실행된다. 따라서 service() 메소드에는 실제 서블릿에서 처리해야 
	하는 내용이 구현되어 있다.

```java
package com.top-ing.study;

import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;

public class FirstServlet extends HttpServlet{
	
	@Override
	public void init(ServletConfig config) throws ServletException {
		System.out.println("init() 실행됨!");
	}

	@Override
	public void service(ServletRequest arg0, ServletResponse arg1) throws ServletException, IOException {
		System.out.println("service() 실행됨!");
	}
}
```

이런식으로 init()과 service() 메소드를 재정의하여, 서블릿 파일을 완성할 수가 있다.

### 콜백 메소드와 서블릿 객체의 생명주기
여기서 콜백 메소드란? **어떤 객체에서 어떤 상황이 발생하면, 컨테이너가 자동으로 호출항녀 실행되는 메소드를 의미**

앞서 설명했던 함수들의 콜백 메소드들이 존재하는데 바로, init(), service()가 콜백 메소드에 속한다. 또한, destroy() 메소드도 콜백 메소드인데 이 메소드는 메모리가 서블릿 객체가 삭제될 때 호출되는 메소드 이다.

| 메소드명  | 메소드 실행시점                          | 실행 횟수 | 기능 구현                         |
|-----------|------------------------------------------|-----------|-----------------------------------|
| init()    | 최초로 서블릿 요청이 들어올 때           | 1         | 초기화 작업                       |
| service() | 클라이언트로부터 요청이 있을 때마다 실행 | n         |  실제 서블릿이 처리해야 하는 작업 |
| destroy() | 서블릿 객체가 메모리에서 삭제될 때       | 1         |  자원 해제 작업                   |

지금까지 본 내용 중에서 서블릿 객체의 생성과 삭제 시점에 관련해서 서술하고자 한다.

	+ 서블릿 객체의 생성
	서버 입장에서 클라이언트로부터 최초로 서블릿 실행 요청이 있을 때
		+ 세부 로직
		클라이언트 요청 -> 서블릿 컨테이너 -> 서블릿 객체 생성 (메모리 로드)
		-> init() -> service() 순으로 실행.
		이 후 요청에 대해서는 생성한 서블릿 객체의 service() 메소드 실행.

	즉, 여기서 최초 요청 여부란?
		**서블릿 객체가 생성되어있는지? 아닌지?** 로 판별이 된다.
		따라서, 요청이 있을때마다 서블릿 객체를 생성하는  것이 아니라, 
		최초 요청 때 생성한 서블릿 객체를 계속 사용하는 것이다.
	
	이 부분을 좀 더 상세하게 보자면 Java EE 스펙에서 강제하지는 않지만, 서블릿은 
	대부분 멀티스레드 환경에서 **싱글톤**으로 동작한다. 
	서블릿 클래스당 하나의 오브젝트만 만들어두고, 사용자의 요청을 담당하는
	여러 스레드에서 하나의 오브젝트를 공유해 동시에 사용한다.
	
	+ 서블릿 객체의 삭제
	서버를 중지시켜 웹 어플리케이션 서비스를 중지할 때이다.
	웹서버에서는 전체 서비스를 중지할 수도 있고, 일부 서비스만 중지할 수도 있다.
	어떤 상황이든지 서블릿 객체가 삭제되는 시점은 웹서버에서 웹 어플리케이션 서비
	스가 중지되는 시점이다. 이때 destroy() 메소드가 호출되어 실행된다. 
