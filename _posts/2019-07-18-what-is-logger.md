---
layout: post
title: "[Spring] Logback을 이용한 웹서버 Logger 만들기"
author: "forget1026"
date: '2019-07-18 18:00:00 +0900'
categories: [spring, blog, markdown]
---

## Logback을 이용한 Spring Boot에서 Logger 구현해 보기

### 목차
1. Logback이란 무었인가?
1. Logback을 이용해서 구현해보기
    1. Logback 설정하기
    1. Logback 사용하기
    1. Log4jdbc 설정하기
    1. Log4jdbc를 이용해서 쿼리 로그 확인하기
1. Spring의 인터셉터(interceptor)를 이용해서 REST API 추적해 보기

---

1. Logback이란?

    Logback의 장점

        1. 과거에 사용였던 Log4j를 바탕으로 만들어서 검증되어 있습니다.

        2. Log4j부터 진행한 테스트 경험을 바탕으로 더욱 광범위한 테스트를 거쳤습니다.

        3. 로그 설정이 변경될 경우 서버 재시작을 안하더라도 반영이 됩니다.
    
    위와 같은 장점을 바탕으로 Logback은 slf4j를 이용해서 사용하게 됩니다.

    물론 스프링 부트로 프로젝트를 만들경우 Logback을 기본으로 작성됩니다.


2. Logback을 이용해서 구현해보기

      1. Logback 설정하기

   스프링 부트를 WebApplication을 시작하게 되면 우선 classpath내의 logback-spring.xml을 검색하고 자동으로 등록해서 사용하게 한다.

   ```logback-spring.xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE xml>
   <configuration debug="true">
   	<!-- Appenders -->
   	<appender name="console" class="ch.qos.logback.core.ConsoleAppender">
   		<encoder>
   			<Pattern>%d %5p [%c] %m%n</Pattern>
   		</encoder>   
   	</appender>
   
   	<!-- 로거 -->
   	<logger name="로거를 사용할 패키지 네임" level="DEBUG" appender-ref="console"/>
   	
       <!-- 루트 로거 -->
       <root level="off">
           <appender-ref ref="console"/>
       </root>
   </configuration>
   ```

   Appender :   로그를 어디에 출력할지 결정하는 역할.

   encoder : appender에 포함되서 출력할 로그를 지정된 형식으로 변환해주는 역할

   logger : 로그를 출력하는 요소. level의 속성을 이용해서 지정할 수 있습니다. 
   
   ex) Debug레벨의 로그를 출력하는 형식은 위의 지정된 appender를 통해서 지정할 수 있습니다.

   ※ 참고 log의 레벨

   ​    trace : 모든 로그를 출력

   ​    debug : 개발할 때 debug용으로 주로 사용합니다.

   ​	info : 상태 변경과 같은 정보성 메시지

   ​	warn : 프로그램의 실행에는 문제가 없으나 경고성 메시지를 띄워 줍니다.

   ​	error : 에러가 발생

   설정을 어떻게 하냐에 따라서 로거의 출력범위가 달라 집니다.

   ex) Debug로 설정 했을 경우 : debug, info, warn, error

   ​	Info로 설정 했을 경우 : info, warn, error

   ※ 부분의 위로 갈수록 더 많은 로그를 출력하게 됩니다.

   ​	2.Logback 사용하기

   로거를 사용하고자 하는 자바 클래스에서 위와 같이 선언해 준다.

   ```java
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   
   Logger log = LoggerFactory.getLogger(this.getClass());
   ```

   그리고 사용법은 다음과 같다.

   ```java
   log.trace("Log message trace level");
   log.debug("Log message debug level");
   log.info("Log message info level");
   log.warn("Log message warn level");
   log.error("Log message error level");
   ```
   
   3. Log4jdbc 설정하기

   왜 Log4jdbc를 사용하는가? 

        길이가 긴 쿼리로그들도 보기 쉽게 만들기 위한 목적입니다.

   우선 사용하기 위해서 build.gradle의 dependencies에 다음과 같이 추가 합니다.

   ```groovy
   implementation group:'org.bgee.log4jdbc-log4j2', name:'log4jdbc-log4j2-jdbc4.1', version: '1.16'
   ```

   그리고 log4jdbc를 설정하기 위한 propertity를 다음과 같이 작성해줍니다.

   ```properties
   log4jdbc.spylogdelegator.name = net.sf.log4jdbc.log.slf4j.Slf4jSpyLogDelegator
   log4jdbc.dump.sql.maxlinelength = 0
   ```

   그리고 jdbc를 연결할 dirver name과 url을 다음과 같이 바꿉니다.

   ```java
   setDriverClassName("net.sf.log4jdbc.sql.jdbcapi.DriverSpy");
   setJdbcUrl("jdbc:log4jdbc:mysql://localhost:3306/codev?serverTimezone=Asia/Seoul")
   ```

   사용하실 분의 dataSource에서 위와 같이 셋팅해주시면 됩니다. 

    ※ 위의 글을 작성한 작성자의 경우 mySQL을 사용하였습니다.

   그리고 logback-spring.xml에 각자의 입맛에 맞게 name을 설정해주시면 됩니다.

   ```  xml
   <!-- 예시 -->
   <logger name="jdbc.sqlonly" level="INFO" appender-ref="console"></logger>
   ```

   ※ 참고 log4jdbc는 다음과 같은 name을 지원합니다.

        jdbc.sqlonly : SQL 쿼리문을 보여줍니다. 

        jdbc.sqltiming : SQL문과 SQL문의 실행 시간을 milisecond단위로 보여줍니다.

        jdbc.audit : ResultSet을 제외한 모든 정보를 다 보여줍니다.

        jdbc.resultset : 위의 결과값에서 ResultSet까지 보여줍니다.

        jdbc.resulttable : SQL의 결과를 테이블로 표현해줍니다.

        jdbc.connection :  Connection의 연결과 종료에 관련된 로그를 보여줍니다.

   4. Log4jdbc를 이용해서 쿼리 로그 확인하기

        ![LogResult](/assets/img/Logger/Log4jdbc결과.PNG)


3. Spring의 인터셉터(interceptor)를 이용해서 REST API 추적해 보기

        ![SpringInterceptor](/assets/img/Logger/스프링인터셉터.png)

   위의 그림은 스프링 MVC 요청 사이클 입니다.

   위에서 보듯이 사용자의 요청은 Filter를 거치거나 Contoller를 가기 위해서는 interceptor를 거치게 됩니다.

   여기서 filter와 interceptor의 차이점은 **필터는 Dispatcher Servlet에 가기전에 동작**을 하고 **interceptor는 Handler Controller로 가기 전에 동작**합니다. 그리고 Interceptor는 filter와 다르게 스프링 빈을 사용할 수 있습니다.

   Interceptor는 다음과 같이 구현 할 수 있습니다.

   ```java
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   import org.springframework.web.servlet.ModelAndView;
   import org.springframework.web.servlet.handler.HandlerInterceptorAdapter;
   
   public class LoggerInterceptor extends HandlerInterceptorAdapter {
       private Logger log = LoggerFactory.getLogger(this.getClass());
       
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           return super.preHandle(request, response, handler);
       }
   
       @Override
       public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception{
           
       }
   }
   ```

   preHandle : 컨트롤러 실행 전에 수행됩니다.

   postHandle : 컨트롤러 수행 후 결과를 뷰로 보내기 전에 수행됩니다.

   afterCompletion : 뷰의 작업까지 완료된 후 수행됩니다.


   그리고 위의 interceptor를 Configuration에 등록해 합니다.

   ```java
   import org.springframework.context.annotation.Configuration;
   import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
   import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
   
   import com.codev.interceptor.LoggerInterceptor;
   
   @Configuration
   public class WebMvcConfiguration implements WebMvcConfigurer{
       @Override
       public void addInterceptors(InterceptorRegistry registry){
           registry.addInterceptor(new LoggerInterceptor());
       }
   }
   ```

   그러면 위와 같은 결과를 얻을 수 있습니다.

   ![SpringInterceptor](/assets/img/Logger/LoggerInterceptor.PNG)

   
## 참고문헌

스프링부트 시작하기. 김인우 저

Toast Meetup Logging과 Profile 전략 :  https://meetup.toast.com/posts/149