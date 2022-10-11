* [목차](#목차)
* [Spring Framework](#spring-framework)
    + [장점](#장점)
    + [Spring Framework을 배워야 하는 이유](#spring-framework을-배워야-하는-이유)
        + [JSP](#jsp)
        + [서블릿(Servlet)](#서블릿servlet)
        + [Spring MVC](#spring-mvc)
        + [Srping Boot](#spring-boot)
    + [핵심 정리](#핵심-정리)
    + [심화 학습](#심화-학습)

# Spring Framework

> Java로 웹 애플리케이션을 개발하기 위한 Framework 중 하나
> 

## 장점

1. POJO(Plan Old Java Object)기반의 구성
2. DI(Dependency Injection) 지원
3. AOP(Aspect Oriented Programming, 관점지향 프로그래밍) 지원
4. Java 언어를 사용함으로써 얻는 장점

Java 언어를 사용하는 Framework는 다양하다.(Apache Struts2, Apache Wicket, JSF, Grails 등) 그런데 사람들은 왜 Spring Framework에 더 열광하는 걸까?

### 기업용 엔터프라이즈 시스템

기업용 엔터프라이즈 시스템이란 기업의 업무(기업 자체 조직의 업무, 고객을 위한 서비스 등)를 처리해주는 시스템을 의미한다. 기업용 엔터프라이즈 시스템은 대량의 사용자 요청을 처리해야 하기때문에 서버의 자원 효율성, 보안성, 시스템의 안정성이나 확장성 등을 충분히 고려해야 시스템을 구축하는 것이 일반적이다.

Spring Framework은 개발 생산성을 향상 시키고 애플리케이션의 유지 보수를 용이하게 하는 Framework의 기본 목적 그 이상을 달성할 수 있게 해준다.

자세한 내용은 이후 알아보자

## Spring Framework을 배워야 하는 이유

초창기 Java 시절의 코드부터 보자

### JSP

Java 초창기에는 JSP(Java Server Page)를 통해 웹 애플리케이션 개발이 이루어졌다. JSP 개발 방식은 사용자에게 보여지는 View 페이지 코드와 요청을 처리하는 서버 코드가 섞여있는 형태의 개발 방식이다. 쉽게 말해 html/Javascript 코드와 Java 코드가 뒤섞여 잇는 방식이다.

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/functions" prefix="fn" %>
<%
    request.setCharacterEncoding("UTF-8");
    response.setContentType("text/html;charset=UTF-8");

    System.out.println("Hello Servlet doPost!");

    String todoName = request.getParameter("todoName");
    String todoDate = request.getParameter("todoDate");

    ToDo.todoList.add(new ToDo(todoName, todoDate));

    RequestDispatcher dispatcher = request.getRequestDispatcher("/todo_model1.jsp");
    request.setAttribute("todoList", ToDo.todoList);

    dispatcher.forward(request, response);
%>
<html>
<head>
    <meta http-equiv="Content-Language" content="ko"/>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/>

    <title>TODO 등록</title>
    <style>
        #todoList {
            border: 1px solid #8F8F8F;
            width: 500px;
            border-collapse: collapse;
        }

        th, td {
            padding: 5px;
            border: 1px solid #8F8F8F;
        }
    </style>
    <script>
        function registerTodo(){
            var todoName = document.getElementById("todoName").value;
            var todoDate = document.getElementById("todoDate").value;

            if(!todoName){
                alert("할일을 입력해주세요..");
                return false;
            }
            if(!todoDate){
                alert("날짜를 입력해주세요.");
                return false;
            }

            var form = document.getElementById("todoForm");
            form.submit();

        }
    </script>
</head>
<body>
    <h3>TO DO 등록</h3>
    <div>
        <form id="todoForm" method="POST" action="/todo_model1.jsp">
            <input type="text" name="todoName" id="todoName" value=""/>
            <input type="date" name="todoDate" id="todoDate" value=""/>
            <input type="button" id="btnReg" value="등록" onclick="registerTodo()"/>
        </form>
    </div>
    <div>
        <h4>TO DO List</h4>
        <table id="todoList">
            <thead>
                <tr>
                    <td align="center">todo name</td><td align="center">todo date</td>
                </tr>
            </thead>
            <tbody>
                <c:choose>
                    <c:when test="${fn:length(todoList) == 0}">
                        <tr>
                            <td align="center" colspan="2">할 일이 없습니다.</td>
                        </tr>
                    </c:when>
                    <c:otherwise>
                        <c:forEach items="${todoList}" var="todo">
                            <tr>
                                <td>${todo.todoName}</td><td align="center">${todo.todoDate}</td>
                            </tr>
                        </c:forEach>
                    </c:otherwise>
                </c:choose>
            </tbody>
        </table>
    </div>
</body>
</html>
```

실제로 이 방식은 애플리케이션의 유지 보수 측면에서 최악의 방식이라고 볼 수 있다. 이제 JSP보다 조금 나은 개발 방식인 서블릿(Servlet) 개발 방식을 살펴보자

### 서블릿(Servlet)

JSP 방식 역시 내부적으로는 Servlet 방식을 사용한다. Servlet은 클라이언트 웹 요청처리에 특화된 Java 클래스의 일종이라고 보면 되는데, **Spring을 사용한 웹 요청을 처리할 때에도 내부적으로는 Servlet을 사용**한다. 

Servlet을 사용한다는 의미는 **Servlet을 위한 Java 코드가 클라이언트 측 코드에서 분리되어 별도의 Java 클래스로 관리된다는 것을 의미**한다.

```java
@WebServlet(name = "TodoServlet")
public class TodoServlet extends HttpServlet {
    // (1) Database를 대신한다.
    private List<ToDo> todoList;

    @Override
    public void init() throws ServletException {
        super.init();
        this.todoList = new ArrayList<>();
    }

		// (2)
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        request.setCharacterEncoding("UTF-8");
        response.setContentType("text/html;charset=UTF-8");

        String todoName = request.getParameter("todoName");
        String todoDate = request.getParameter("todoDate");

        todoList.add(new ToDo(todoName, todoDate));

        RequestDispatcher dispatcher = 
                request.getRequestDispatcher("/todo.jsp");
        request.setAttribute("todoList", todoList);

        dispatcher.forward(request, response);
    }

		// (3)
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        System.out.println("Hello Servlet doGet!");

        RequestDispatcher dispatcher = 
                request.getRequestDispatcher("/todo.jsp");
        dispatcher.forward(request, response);
    }
}
```

JSP 코드에서 Java 코드만 별도의 서블릿 클래스로 분리된 모습이다. 하지만 여전히 코드 자체가 너무 길어보인다.

데이터를 가공하는 비즈니스 로직이 있는 것도 아니고, 가공된 데이터를 데이터베이스에 저장하는 등의 데이터 액세스 로직 역시 존재하지 않는데도 불구하고 코드 자체가 너무 길어보인다.

이 부분을 개선한 Spring을 보자

### Spring MVC

```java
@Controller
public class ToDoController {
    @RequestMapping(value = "/todo", method = RequestMethod.POST)
    @ResponseBody
    public List<ToDo> todo(@RequestParam("todoName")String todoName,
                               @RequestParam("todoDate")String todoDate) {
        ToDo.todoList.add(new ToDo(todoName, todoDate));
        return ToDo.todoList;
    }

    @RequestMapping(value = "/todo", method = RequestMethod.GET)
    @ResponseBody
    public List<ToDo> todoList() {
        return ToDo.todoList;
    }
}
```

훠어어얼씬 간결해졌다. 눈에 보이지는 않지만 다양한 작업들을 Spring에서 알아서 처리해주기 때문이다.

이러한  장점에도 불구하고 Spring 기반의 애플리케이션 기본 구조를 잡는 설정 작업이 여전히 불편하다는 단점이 있었다. 

**Spring MVC 설정 파일 코드**

```html
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/spring-config/applicationContext.xml</param-value>
    </context-param>
    <listener>
        <listener-class>
            org.springframework.web.context.ContextLoaderListener
        </listener-class>
    </listener>
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>
            org.springframework.web.servlet.DispatcherServlet
        </servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>/WEB-INF/spring-config/dispatcher-servlet.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    <filter>
        <filter-name>CORSFilter</filter-name>
        <filter-class>com.codestates.filter.CORSFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>CORSFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

</web-app>
```

Spring 애플리케이션을 정상적으로 구동하기 위해서는 위와 같은 설정 파일들이 필요했다. 다행히 이러한 문제가 대부분 개선된 Spring Boot가 탄생한다.

### Spring Boot

```java
@RestController
public class TodoController {
    private TodoRepository todoRepository;

    @Autowired
    TodoController(TodoRepository todoRepository) {
        this.todoRepository = todoRepository;
    }

    @PostMapping(value = "/todo/register")
    @ResponseBody
    public Todo register(Todo todo){
        todoRepository.save(todo); 
        return todo;
    }

    @GetMapping(value = "/todo/list")
    @ResponseBody
    public List<Todo> getTodoList(){
        return todoRepository.findAll();
    }
}
```

Spring Boot는 Spring보다 기능이 추가했는데도 불구하고 코드의 길이는 바뀌지 않고 오히려 더 깔끔해지기까지 했다.

**Spring Boot 설정 파일**

```java
spring.h2.console.enabled=true
spring.h2.console.path=/console
spring.jpa.generate-ddl=true
spring.jpa.show-sql=true
```

Spring MVC에서 겪어야 했던 설정의 복잡함을 Spring Boot에서는 찾아볼 수 없다.

Spring의 복잡한 설정 작업마저도 Spring이 대신 처리를 해주기 때문에 개발자는 애플리케이션의 핵심 비즈니스 로직에만 집중할 수 있게 되었다.

## 핵심 정리

- Spring Framework가 도입되기 전에는 JSP나 Servlet 기술을 사용하여 Java 웹 애플리케이션을 제작하였다.
- Spring MVC 방식이 도입됨으로써 Java 웹 애플리케이션의 제작 방식이 획기적으로 변하게 되었다.
- Spring MVC 설정의 복잡함과 어려움을 극복하기 위해 Spring Boot이 탄생했다.

## **심화 학습**

- Java 서블릿(Servlet)이란?
    - Java Servlet 자체를 사용하는 기술은 현재 거의 사용하고 있지 않지만 **Servlet은 Spring MVC 같은 Java 기반의 웹 애플리케이션 내부에서 여전히 사용**이 되고 있다.
    - Servlet에 대해서 더 알고 싶다면 아래 링크를 클릭!
        - 자바 서블릿: [https://ko.wikipedia.org/wiki/자바_서블릿](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_%EC%84%9C%EB%B8%94%EB%A6%BF)
- 서블릿 컨테이너(Servlet Container)란?
    - 서블릿 컨테이너(Servlet Container)는 서블릿(Servlet) 기반의 웹 애플리케이션을 실행해주는 것부터 시작해서 Servlet의 생명 주기를 관리하며, 쓰레드 풀(Thread Pool)을 생성해서 Servlet과 Thread를 매핑 시켜주기도 한다.
    - 서블릿 컨테이너에 대한 자세한 내용이 궁금하다면 아래 링크를 클릭!
        - 서블릿 컨테이너: [https://ko.wikipedia.org/wiki/웹_컨테이너](https://ko.wikipedia.org/wiki/%EC%9B%B9_%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88)
    - **아파치 톰캣(Apache Tomcat)**은 서블릿 컨테이너의 한 종류로써 Spring MVC 기반의 웹 애플리케이션 역시 **기본적으로 아파치 톰캣에서 실행**이 된다.
    - 아파치 톰캣에 대한 내용은 아래 링크에서 확인하자!
        - 아파치 톰캣
            - [https://ko.wikipedia.org/wiki/아파치_톰캣](https://ko.wikipedia.org/wiki/%EC%95%84%ED%8C%8C%EC%B9%98_%ED%86%B0%EC%BA%A3)
            - [https://tomcat.apache.org/](https://tomcat.apache.org/)