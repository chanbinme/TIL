# 목차
* [목차](#목차)
* [TCP/IP](#tcp--ip)
    + [LAN과 WAN](#lan과-wan)
	+ [인터네트워킹(internetworking)](#인터네트워킹internetworking)
    + [프로토콜(protocol)](#프로토콜protocol)
    + [TCP/IP](#tcp--ip)
* [주소(address)](#주소address)
    + [IP주소(Internet Protocol address)](#ip-주소internet-protocol-address)
    + [MAC주소](#mac-주소)    
    + [패킷](#패킷)

# TCP/IP

## LAN과 WAN

- LAN (Local Area Network) : 좁은 범위에서 연결된 네트워크
- WAN (Wide Area Network) : 수많은 LAN들이 모여세계를 연결하는 네트워크

### 우리는 왜 인터넷 비용을 지불할까?

LAN이 WAN으로 확장하기 위해서는 각 거점을 연결하는 통신회선 서비스를 이용해야 한다.
KT나 LGU+, SKT 등의 통신 회사가 이러한 회선 서비스를 구성하고 고객에게 서비스를 제공한다. 우리가 사용하는 인터넷은 이러한 회선 서비스를 이용한 WAN에 접속이 가능하기 때문에, 비용을 지불한다.

## 인터네트워킹(internetworking)

> 여러 네트워크를 연결하는 것
> 

네트워크를 확장하는 방식은 크게 두 가지 방법이 있다.

- 한 네트워크를 확장하는 방법
- 네트워크와 네트워크를 연결하는 방법

이 중 네트워크와 네트워크를 연결하는 것을 인터네트워킹이라고 한다. 전 세계적으로 인터네트워킹하는 것이 우리가 사용하는 인터넷(The Internet)이다.

### 장점

- 네트워크 일부에서 고장이 나도 영향이 광범위하게 퍼지지 않는다
- 불필요한 통신이 네트워크 전체로 확산하지 않는다.
- 개별 네트워크의 방침에 따라 관리가 가능하다.

## 프로토콜(protocol)

> 인터넷이 연결되어 있는 컴퓨터들이 일관되게 네트워크를 사용할 수 있게 하는 공통언어. 대표적으로 TCP/IP가 있다.
> 

## TCP / IP

인터넷 통신 스위트(Internet Protocol Suite)는 인터넷에서 컴퓨터들이 서로 정보를 주고 받는데 쓰이는 통신규약의 모음이다.

이 모음은 다른 컴퓨터나, 다른 운영체제, 다른 회선간의 통신이 가능하게 해준다.

인터넷이 처음 시작하던 시기에 정의되어 현재까지 표준으로 사용하고 있는 TCP(Transmission Control Protocol)와 IP(Internet Protocol)에서 가져와 TCP / IP라고 부른다.

### TCP / IP 4계층 모델

|  | TCP/IP 4계층 모델 | 주요 프로토콜 | 역할  |
| --- | --- | --- | --- |
| 4층 | 응용 계층 (Application Layer) | HTTP, DNS, FTP, … | 애플리케이션에 맞추어 통신한다. |
| 3층 | 전송 계층(Transport Layer) | TCP, UDP, … | IP와 어플리케이션을 중개해 데이터를 확실하게 전달한다. |
| 2층 | 인터넷 계층(Internet Layer) | IP, ICMP, ARP, RARP | 네트워크 주소를 기반으로 데이터를 전송한다. |
| 1층 | 네트워크 접근 계층(Network Access Layer) | Ethernet, wifi, … | 컴퓨터를 물리적으로 네트워크에 연결해서 기기 간에 전송이 가능하게 한다. |

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f9e0a773-66c2-4716-a92e-44ca4bfd4402/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220930T091011Z&X-Amz-Expires=86400&X-Amz-Signature=5801f77ae9614527a5c13d8f48b321a553c082dedc2230dc1409ec34074ecaa9&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

각 Layer가 실제로 어떻게 사용되는지 알아보자

# 주소(address)

> 네트워크에 연결된 특정 PC의 주소를 나타내는 체계를 address(Internet Protocol address, IP 주소)라고 한다.
> 

## IP 주소(Internet Protocol address)

> TCP/IP 구조에서 컴퓨터를 식별하기 위해 사용되는 주소
> 
- 컴퓨터나 휴대전화, 서버, 인터넷 라우터 등 네트워크 장비에 각각의 IP주소가 할당되게 된다.
- IP주소에는 Private 주소와 Public 주소가 있다.
    - Private IP 주소 : LAN 네트워크 내부에서 사용
    - Public IP 주소 : 인터넷에서 사용
- IP는 인터넷상에서 사용하는 주소체계를 의미한다.
- 인터넷에 연결된 모든 PC는 IP 주소체계를 따라 네 덩이의 숫자로 구분된다. 이렇게 네 덩이의 숫자로 구분된 IP 주소체계를 IPv4라고 한다.
    - IPv4 : Internet Protocol version 4의 줄임말로, IP 주소체계의 네 번째 버전을 뜻한다.
- 터미널에서 `nslookup [주소]` 를 입력하면 해당 사이트에 IPv4 주소를 확인할 수 있다.
    
    ```java
    happy_bin@gimchanbin-ui-MacBookPro ~ % nslookup dev-jambin.github.io
    Server:		168.126.63.1
    Address:	168.126.63.1#53
    
    Non-authoritative answer:
    Name:	dev-jambin.github.io
    Address: 185.199.111.153
    Name:	dev-jambin.github.io
    Address: 185.199.108.153
    Name:	dev-jambin.github.io
    Address: 185.199.109.153
    Name:	dev-jambin.github.io
    Address: 185.199.110.153
    ```
    
    - localhost, 127.0.0.1 : 현재 사용 중인 로컬 PC를 지칭한다.
    - 0.0.0.0, 255.255.255.255 : broadcast address, **로컬 네트워크에 접속된 모든 장치와 소통하는 주소이다.** 서버에서 접근 가능 IP 주소를 broadcast address로 지정하면, 모든 기기에서 서버에 접근할 수 있다.
- 개인 PC의 보급으로 IPv4로 할당할 수 있는 PC가 한계를 넘어서게 되었고, 이를 위해 세상에 나오게 된 것이 IPv6이다. 하지만 여전히 IPv4를 메인으로 사용하고 있다.

### IP 주소 구조

- IPv4 주소는 OOO.OOO.OOO.OOO의 형식으로 되어 있다.
- 10진수로 표기되어 있지만, 마침표로 구분된 4개의 8비트 필드(8자리 2진수 4개)로 되어있다.
- 각 8비트 필드는 IPv4 주소에서 1바이트를 나타낸다. 이러한 형식을 점으로 구분된 십진수 형식이라고도 한다.
- IP 주소는 네트워크부와 호스트부로 나뉜다.
    - 네트워크부 : 어떤 네트워크인지를 알 수 있는 정보
    - 호스트부 : 그 네트워크 안의 특정 컴퓨터를 지칭하는 정보

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/8b2a84ec-a3bc-4190-a91d-49d9fc58afc0/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220930T091026Z&X-Amz-Expires=86400&X-Amz-Signature=2a0b86865c64d38c1a8ea76bacb1031666cf4ae3d8e644bb7efe664d2e03201a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- IPv4 주소에서 네트워크부가 어디까지인지 나타내는 것이 서브넷 마스크이다.
    - IP 주소 : 192.168.1.1
    - 서브넷 마스크 : 255.255.255.0
    - 네트워크 주소 : 192.168.1.0
    - 브로드캐스트 주소 : 192.168.1.255
- 8자리의 2진수 묶음을 옥텟이라고 부른다. IPv4 주소는 4개의 옥텟으로 이루어져 있고, 각각을 1옥텟, 2옥텟, 3옥텟, 4옥텟이라고 부른다.
- 위 서브넷 마스크의 경우, 1에서 3까지의 옥텟을 네트워크부로 사용하는 서브넷 마스크이다. 따라서 4옥텟은 호스트부로 사용하고 있음을 알 수 있다.

### IP 주소의 할당과 관리

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/978cd0e6-258a-418c-9d74-75d6aee87c26/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220930T091041Z&X-Amz-Expires=86400&X-Amz-Signature=bac8ce27437fa3e36dde63660a6ed1fd5cc2f696e767ffaa2cf59338c60a0a79&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- MAC 주소와 달리, IP주소는 처음부터 주어지는 것이 아니라 할당되는 것이다.
- 호스트부를 변경해 가면서 IP 할당이 이루어진다.
- 호스트부는 이루어진 2진수이므로, 할당할 수 없는 시작(0)과 끝 숫자(255)를 제외한 번호로 할당이 가능하다.
- 따라서 시작(0)과 끝(255)를 제외한 254개의 주소만이 할당 가능한 주소이다.

<aside>
💡 호스트부가 0으로만 이루어진 것을 네트워크 주소로 그 네트워크를 의미한다.
호스트부가 1로만 이루어진 것은 브로드캐스트 주소로 ARP와 같은 기능을 사용하기 위해 사용한다.

</aside>

### IP 프로토콜의 한계

- 비연결성
- 비신뢰성
- 패킷을 받을 대상이 없거나 특정한 이유로 서비스 불능 상태에 빠져도 데이터를 받을 상대의 상태 파악이 불가능하기 때문에 패킷을 그대로 전송하는 비연결성 문제가 있다.
- 중간에 패킷이 사라지더라도 보내는 기기 측에서는 알 수 있는 방법이 없다. 또한 서로 다른 노드를 거쳐서 전송되는 특성상, 보내는 기기측에서 의도한 순서대로 데이터가 도착하지 않을 수 있다.
- 한 IP에서 여러 애플리케이션이 작동하는 경우 특정할 수 없는 한계가 있다.
- 이러한 한계들을 극복하기 위해 TCP와 UDP가 사용된다.

## MAC 주소

> 제조사에서 각 네트워크 기기에 할당하는 고유 시리얼 주소
> 

IP address만 가지고는 네트워크 상에서 송수신을 할 수 없다. MAC 주소를 IP 주소와 조합해야만 네트워크를 통한 통신이 가능하다.

이더넷에서는 네트워크의 송수신 상대를 특정하고자 MAC 주소를 사용하고, TCP/IP에서는 IP address를 사용한다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/72da6ab8-63d4-4c46-bcd4-97b8e80e8f62/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220930T091104Z&X-Amz-Expires=86400&X-Amz-Signature=7c2d48751da11d540b65b0e54b254193411d6518a078c8848658ab9057d4c8e9&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

같은 LAN에 속한 기기끼리 통신을 할 때는 상대방의 MAC 주소를 파악하는 과정이 필요하다.

이 때 사용하는 것이 ARP(address resolution protocol)이다. 

1. MAC 주소를 파악하기 위해 네트워크 전체에 [브로드캐스트](https://www.notion.so/fe5a5b1017e147cf8b17e547ea93d520)를 통해 패킷을 보낸다.
2. 해당 IP를 갖고 있는 컴퓨터가 자신의 MAC 주소를 Response한다.
3. 전달받은 MAC주소를 통해 통신하게 해준다. 

## 패킷

기기끼리의 통신에는 두 가지 방식이 있다.

### 회선 교환(Circuit Switching) 방식

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/6ef4d3a9-3150-4e59-a723-1aea0a5eb3d1/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220930T091119Z&X-Amz-Expires=86400&X-Amz-Signature=85d2b0e098379380dd29328b8e85a3984ee662f873e86db1810781e8b83b8cd5&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

통신 회선을 설명하여 데이터 교환을 하는 회선교환 방식은 주로 음성전화 시스템에 사용된다.

전화는 일대일로 데이터를 교환하고, 전화간 통화 중에는 다른 상대와 전화통화가 불가능하다.

### 패킷 교환(Packet Switching) 방식

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/e7a4c6c4-f9a8-438b-8f31-70f02917a0c7/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220930T091130Z&X-Amz-Expires=86400&X-Amz-Signature=cd91694f3cb9abd04a885a7fd2b80199309ba0f3801d89387877b5432bfcac29&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

컴퓨터 네트워크는 여러 상대와 통신이 가능해야 한다. 따라서 회선 교환방식은 컴퓨터 네트워크에 적합하지 않다. 그래서 이를 극복하기 위해 패킷 교환방식이 생겨났다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/84477ffb-4c26-4d3a-961b-3450552a38b6/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220930T091143Z&X-Amz-Expires=86400&X-Amz-Signature=cfddb8b7d1b758cb3f09c58fdb00cf25f308b9b1b0833132c4a18fff63b474c1&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

패킷 교환 방식은 원본 데이터를 패킷(packet)이라고 하는 작은 단위로 나누고, 여러 회선을 공용해 통신을 주고 받는다.

하나의 패킷은 헤더와 페이로드로 구성되어 있고, 헤더에는 어떤 데이터의 몇 번째 데이터인지의 정보와 보내는 곳이나 최종 목적지에 대한 정보 등이 들어 있다. 

이렇게 주고받을 데이터를 작게 분할하여 전송하더라도, 도착한 곳에서 원래대로 복원이 가능하다.

## 심화 학습

네트워크 접속 기기가 많아지게 되면 IP 주소를 별도로 관리해야 한다. 이를 위한 소프트웨어인IPAM 에 대해 알아 보자

- [https://en.wikipedia.org/wiki/IP_address_management](https://en.wikipedia.org/wiki/IP_address_management)
- [https://docs.microsoft.com/en-us/windows-server/networking/technologies/ipam/ipam-top](https://docs.microsoft.com/en-us/windows-server/networking/technologies/ipam/ipam-top)