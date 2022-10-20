# Spring MvC의 동작 방식과 구성요소

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/279d8d1d-bc1c-4d7e-b367-13af5e5cb036/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221020T115019Z&X-Amz-Expires=86400&X-Amz-Signature=29ee03e8a3b0c7aabbb7060d87857feac2bc8cc28ed847c86fb1678d8792c2ae&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

1. 클라이언트가 요청을 전송하면 `DispatcherServlet` 이라는 클래스에 요청이 전달된다.
2. `DispatcherServlet`은 클라이언트의 요청을 처리할 Controller에 대한 검색을 HandlerMapping 인터페이스에게 요청한다. 
3. `HandlerMapping` 은 클라이언트 요청과 매핑되는 핸들러 객체를 다시 DispatcherServlet 에 리턴해준다.


>핸들러 객체는 해당 핸들러의 Handler 메서드 정보를 포함하고 있다. 
>Handler 메서드는 Controller >클래스 안에 구현된 요청 처리 메서드를 의미한다.

1. 요청을 처리할 Controller 클래스를 찾았으면 실제로 클라이언트 요청을 처리할 Handler 메서드를 찾아서 호출해야 한다. `DispatcherServlet` 은 Handler 메서드를 직접 호출하지 않고, HandlerAdapter 에게 Handler 메서드 호출을 위임한다.
2. `HandlerAdapter` 는 DispatcherServlet 으로부터 전달 받은 Controller 정보를 기반으로 해당 Controller의 Handler 메서드를 호출한다. 
3. `Controller` 의 Handler 메서드는 비즈니스 로직 처리 후 리턴 받은 Model 데이터를 HandlerAdapter에게 전달한다.
4. `HandlerAdapter` 는 전달받은 Model 데이터와 View 정보를 다시 DispatchServlet 에게 전달한다.
5. `DispatcherServlet` 은 전달 받은 View 정보를 다시 ViewResolver 에게 전달해서 View 검색을 요청한다.
6. `ViewResolver` 는 View 정보에 해당하는 View를 찾아서 View를 다시 리턴해준다.
7. `DispatcherServlet` 은 ViewResolver 로 부터 전달 받은 View 객체를 통해 Model 데이터를 넘겨주면 클라이언트에게 전달할 응답 데이터 생성을 요청한다. 
8. `View` 는 응답 데이터를 생성해서 다시 DispatcherServlet에게 전달한다.
9. `DispatcherServlet` 은 View로부터 전달 받은 응답 데이터를 최종적으로 클라이언트에게 전달한다.

### DispatcherServlet의 역할

- DispatcherServlet이 굉장히 많은 역할을 하는 것처럼 보이지만 실제로 요청에 대한 처리는 다른 구성 요소들에게 위임(Delegate)하고 있다.
- 이처럼 DispatcherServlet이 애플리케이션의 가장 앞단에 배치되어 다른 구성요소들과 상호작용하면서 클라이언트의 요청을 처리하는 패턴을 Front Controller Pattern이라고 한다.

![0YLGGxNG1m0dPcKiOrzMe-1655088839178.gif](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/00d12965-66d3-4a2f-8957-69c4bc1e3035/0YLGGxNG1m0dPcKiOrzMe-1655088839178.gif?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221020T115032Z&X-Amz-Expires=86400&X-Amz-Signature=513f0335b221af592c972d804ce397bd64e24954e047fe82108f44a961f33a32&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%220YLGGxNG1m0dPcKiOrzMe-1655088839178.gif%22&x-id=GetObject)