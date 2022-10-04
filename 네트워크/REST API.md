# 목차
* [목차](#목차)
* [REST API](#rest-api)
    + [좋은 REST API를 디자인하는 법](#좋은-rest-api를-디자인하는-방법)
	+ [0단계](#rest-성숙도-모델---0단계)
    + [1단계](#rest-성숙도-모델---1단계)
    + [2단계](#rest-성숙도-모델---2단계)
    + [3단계](#rest-성숙도-모델---3단계)

# REST API

> REST는 “Representational State Transfer”의 약자로, REST API는 웹에서 사용되는 데이터나 자원(Resource)을 HTTP URI로 표현하고, HTTP 프로토콜을 통해 요청과 응답을 정의하는 방식을 말한다. 로이 필딩의 박사학위 논문에서 웹(http)의 장점을 최대한 활용할 수 있는 아키텍처로써  처음 소개되었다.
> 

웹 애플리케이션에서는 HTTP 메서드를 이용해 서버와 통신한다. 클라이언트와 서버가 HTTP 통신을 할 때는 어떤 요청을 보내고 받느냐에 따라 메서드의 사용이 달라진다. 

요청과 응답을 할 때, ‘제대로 보내고 받을 수 있는’ 일종의 규약이 존재한다. 클라이언트와 서버 사이에도 데이터와 리소스를 요청하고 요청에  따른 응답을 전달하기 위한 메뉴판이 필요하다. 이 메뉴판을 보고 클라이언트는 식당에서 식사를 주문하듯 서버에 요청하고, 이에 대한 응답을 메뉴판에 있는 사진이나 음식에 대한 설명처럼 다시 서버에서 클라이언트로 전송하게 된다. 

따라서 HTTP 프로토콜 기반으로 요청과 응답에 따라 리소스를 주고받기 위해서는 알아보기 쉽고 잘 작성된 메뉴판이 필요한데, 이 역할을 API가 수행해야 하므로 서로 잘 알아볼 수 있도록 작성하는 것이 중요하다.

## 좋은 REST API를 디자인하는 방법

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/8107e040-53c0-42af-be48-cb00c183c143/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221004T114531Z&X-Amz-Expires=86400&X-Amz-Signature=75c353e553de664dfa11f491630586a2b626765b29605af223766c92545cb1c7&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

로이 필딩이 제시한 REST 방법론을 보다 더 실용적으로 적용하기 위해 레오나르드 리차드슨은 REST API를 잘 적용하기 위한 4단계 모델을 만들었다.

REST 성숙도 모델은 총 4단계(0~3단계)로 나누어진다.

로이 필딩은 이 모델의 모든 단계를 충족해야 REST API라고 부를 수 있다고 주장했다. 하지만 실제로 3단계까지 지키기 어렵기 때문에 2단계까지만 적용해도 좋은 API 디자인이라고 볼 수 있고, 이런 경우 HTTP API라고 부른다. 

## REST 성숙도 모델 - 0단계

0단계는 좋은 REST API를 작성하기 위한 기본 단계이다. 0단계에서는 단순히 HTTP 프로토콜을 사용하기만 해도 된다. 물론 이 경우, 해당 API를 REST API라고 할 수는 없다.

### 예시
![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/e95b38a1-ff95-4169-88f0-4f56a7ecad00/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221004T114751Z&X-Amz-Expires=86400&X-Amz-Signature=008be3db7f936e6d23ebde0d8fc162d0331b05ca1dc1563eb3f6deda5a9e6892&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)
위 예시처럼 단순히 HTTP 프로토콜을 사용하는 것이 REST API의 출발점이다.

## REST 성숙도 모델 - 1단계

1단계에서는 개별 리소스와의 통신을 준수해야 한다.

조금 더 쉽게 말해보자면, REST API는 웹에서 사용되는 모든 데이터나 자원(Resource)을 HHTP URI로 표현하다고 이야기했다. 그래서 모든 자원은 개별 리소스에 맞는 엔드포인트(Endpoint)를 사용해야 한다는 것과 요청하고 받은 자원에 대한 정보를 응답으로 전달해야 한다는 것이 1단계에서 의미하는 바이다.

앞서 0단계에서는 모든 요청에서 엔드포인트로 `/appointment` 를 사용했다. 하지만 1단계에서는 요청하는 리소스가 무엇인지에 따라 각기 다른 엔드포인트로 구분하여 사용해야 한다. 

### 예시

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/5d817d0d-c974-4ea2-b763-d6917030db43/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221004T114906Z&X-Amz-Expires=86400&X-Amz-Signature=6ca1bff735fbd5d23ee0da8a5b0d51a7688cecaba110388589427b86a3c985d2&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)
위의 예시에서 예약 가능한 시간 확인이라는 요청의 응답으로 받게 되는 자원(리소스)은 허준이라는 의사의 예약 가능한 시간대이다. 그렇기 때문에 요청 시 `/doctors/허준`이라는 엔드포인트를 사용한 것을 볼 수 있다. 그뿐만 아니라, 특정 시간에 예약하게 되면, 실제 slot이라는 리소스의 123이라는 id를 가진 리소스가 변경되기 때문에, 하단의 특정 시간에 예약이라는 요청에서는 `/slots/123`으로 실제 변경되는 리소스를 엔드포인트로 사용하였다.

예시와 같이, 어떤 리소스를 변화시키는지 혹은 어떤 응답이 제공되는지에 따라 각기 다른 엔드포인트를 사용하기 때문에, **적절한 엔드포인트를 작성하는 것이 중요하다.**

**엔드포인트 작성 시에는 동사, HTTP 메서드, 혹은 어떤 행위에 대한 단어 사용은 지양하고, 리소스에 집중해 명사 형태의 단어로 작성하는 것이 바람직한 방법이다.**

요청에 따른 응답으로 리소스를 전달할 때에도 사용한 리소스에 대한 정보와 함께 리소스 사용에 대한 성공/실패 여부를 반환해야 한다. 예를 들어 김찬빈 환자가 허준 의사에게 9시 예약을 진행했지만, 해당 시간이 마감되어 예약이 불가능하다고 가정할 때, 아래와 같이 리소스 사용에 대한 실패 여부를 포함한 응답을 받아야 한다.

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/590c9485-0e5c-493f-8d83-7a7d7ab23fa5/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221004T114952Z&X-Amz-Expires=86400&X-Amz-Signature=94d25c759fd39aedc1659c6817531a5fe6fa8175752127dbfe30785c6c6908f6&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

## REST 성숙도 모델 - 2단계

2단계에서는 CRUD에 맞게 적절한 HTTP 메서드를 사용하는 것에 중점을 둔다. 앞서 0단계와 1단계 예시에서 보았듯, 모든 요청을 CRUD에 상관없이 POST하고 있다.  2단계에 따르면 이는 CRUD에 적합한 메서드를 사용한 것이 아니다. 

예약 가능한 시간을 확인한다는 것은 예약 가능한 시간을 조회(READ)하는 행위를 의미하고, 특정 시간에 예약한다는 것은 해당 특정 시간에 예약을 생성(CREATE)한다는 것과 같다.

그렇기 때문에 조회(READ)하기 위해서는 `GET` 메서드를 사용하여 요청을 보내고, 이 때 `GET` 메서드는 `body` 를 가지지 않기 때문에 query parameter를 사용하여 필요한 리소스를 전달한다.

예약을 생성(CREATE)하기 위해서는 `POST` 메서드를 사용하여 요청을 보내는 것이 바람직하다. 그리고 2단계에서는 `POST` 요청에 대한 응답이 어떻게 반환되는지도 중요하다.

응답은 새롭게 생긴 리소스를 보내주기 때문에, 응답 코드도 `201 Create` 로 명확하게 작성해야 하며, 관련 리소스를 클라이언트가 Locatiton헤더에 작성된 URI를 통해 확인할 수 있도록 해야한다.

### 예시

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4f2d49db-436c-447e-a214-312548dec817/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221004T115039Z&X-Amz-Expires=86400&X-Amz-Signature=faa6000164a9ecf02fd846e0428fe9784696a642b5218c9c359a3373c056bbed&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

메서드를 사용할 때 규칙이 있다.

- `GET` : 서버의 데이터를 변화시키지 않는 요청에 사용해야 한다.
- `POST` : 요청마다 새로운 리소스를 생성하고 `PUT` 은 요청마다 같은 리소스를 반환한다. 이렇게 매 요청마다 같은 리소스를 반환하는 특징을 멱등(idempotent)하다고 한다. 그렇기 때문에 멱등성을 가지는 메서드 `PUT` 과 그렇지 않은 `POST` 는 구분하여 사용해야 한다.
- `PUT` 과 `PATCH` 도 구분하여 사용해야 한다. `PUT` 은 교체, `PATCH` 는 수정의 용도로 사용한다.
    
    API를 작성할 때, REST 성숙도 모델의 2단계까지 적용을 했다면 대체적으로 잘 작성된 API라고 여긴다. 모범적인 API 디자인조차도 REST 성숙도 모델의 3단계까지 적용한 경우는 극히 드물다. 따라서 3단계까지 무조건적으로 모두 적용해야 하는 것은 아니다.
    
    ## REST 성숙도 모델 - 3단계
    
    마지막 단계는 HATEOAS(Hypertext As The Engine Of Application State)라는 약어로 표현되는 하이퍼미디어 컨트롤을 적용한다. 3단계의 요청은 2단계와 동일하지만, 응답에는 리소스의 URI를 포함한 링크 요소를 삽입하여 작성한다는 것이 다르다. 
    
    이 때 응답에 들어가게 되는 링크 요소는 응답을 받은 다음에 할 수 있는 다양한 액션들을 위해 많은 하이퍼미디어 컨트롤을 포함하고 있다. 
    
    ![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/90f79a76-90ca-4f71-a3a3-840a211711e3/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221004T115104Z&X-Amz-Expires=86400&X-Amz-Signature=90c8c6730078b2984b52ec44db9547de046b4f9e85f7bb70793fcfcd78db7273&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)
    
    위와 같이 허준이라는 의사의 예약 가능 시간을 확인한 후에는 그 시간대에 예약을 할 수 있는 링크를 삽입하거나, 특정 시간에 예약을 완료하고나서는 그 예약을 다시 확인할 수 있도록 링크를 작성해 넣을 수도 있다. 이렇게 으답 내에 새로운 링크를 넣어 새로운 기능에 접근할 수 있도록 하는 것이 3단계의 중요 포인트이다.
    
    만약 클라이언트 개발자들이 응답에 담겨 있는 링크들을 눈여겨본다면, 이러한 링크들은 조금 더 쉽고, 효율적으로 리소스와 기능에 접근할 수 있게 하는 트리거가 될 수 있다.
    
    ## **Reference**
    
    위의 원칙에 따라 API를 작성했다면 가장 좋겠지만, 개발자 혹은 개발 상황에 따라 위의 원칙을 꼭 지키지 못할 수도 있다. 다음의 다양한 API를 참고하여, 모범적인 API 디자인을 할 수 있도록 학습해 보자.
    
    - [5가지의 기본적인 REST API 디자인 가이드](https://blog.restcase.com/5-basic-rest-api-design-guidelines/)
    - [호주 정부 API 작성 가이드](https://api.gov.au/standards/national_api_standards/)
    - [구글 API 작성 가이드](https://cloud.google.com/apis/design?hl=ko)
    - [MS의 REST API 가이드라인](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md)