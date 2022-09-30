# 목차
* [목차](#목차)
* [TCP, UDP](#tcp-udp)
    + [TCP 3-way handshake](#tcp-3-way-handshake)
	+ [UDP](#udp-1)

# TCP, UDP

|  | TCP/IP 4계층 모델 | 주요 프로토콜 | 역할  |
| --- | --- | --- | --- |
| 4층 | 응용 계층 (Application Layer) | HTTP, DNS, FTP, … | 애플리케이션에 맞추어 통신한다. |
| 3층 | 전송 계층(Transport Layer) | TCP, UDP, … | IP와 어플리케이션을 중개해 데이터를 확실하게 전달한다. |
| 2층 | 인터넷 계층(Internet Layer) | IP, ICMP, ARP, RARP | 네트워크 주소를 기반으로 데이터를 전송한다. |
| 1층 | 네트워크 접근 계층(Network Access Layer) | Ethernet, wifi, … | 컴퓨터를 물리적으로 네트워크에 연결해서 기기 간에 전송이 가능하게 한다. |
- TCP와 UDP는 TCP/IP 4계층 모델을 기준으로 IP 프로토콜의 계층의 상위에서 동작을 한다.
- 전송계층에 속하는 TCP와 UDP는 2계층에서 동작하는 IP와 4계층에서 동작하는 애플리케이션(http 등)을 중개하는 역할을 한다.
- TCP와 UDP는 중개하는 역할을 하는 점에서는 동일하지만, 각각 다른 특징을 가지고 있다.

|  | TCP (Transmission control protocol) | UDP (User datagram protocol) |
| --- | --- | --- |
| 서비스 타입 | 연결 지향적 프로토콜 | 데이터그램 지향적 프로토콜 |
| 신뢰성 | 데이터 전송 표적 기기까지의 전송을 보장한다. | 표적 기기까지의 전송이 보장되지 않는다. |
| 순서 보장 | 전송하는 패킷들이 순서가 보장된다. | 패킷순서 보장이 안된다. 패킷 순서를 보장하고 싶다면, 애플리케이션 레이어에서 관리되어야 한다. |
| 속도 | UDP와 비교해 느리다. | TCP와 비교해 빠르고, 단순하며 더 효율적인 속도를 가지고 있다.  |

### TCP

- 통신 신뢰성을 높이는 기능이 구현되어 있다.
- 데이터의 신뢰성을 필요로하는 애플리케이션은 TCP 사용
- 웹 애플리케이션에서 많이 사용하는 HTTP의 경우 모든 데이터를 제대로 송수신이 가능해야 하는 특성상, TCP를 사용한다.

### UDP

- 신뢰성을 높이는 기능이 없는 대신 보다 높은 속도와 효율성을 제공한다.
- 빠른 속도나 실시간 통신이 중요한 애플리케이션의 경우 UDP 사용

## TCP 3-way handshake

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ecf31657-197d-4260-a82e-2716883bad8c/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220930T091306Z&X-Amz-Expires=86400&X-Amz-Signature=149aa30b853ec05876f44be88c26e57fd1c444867e50a90fed1f08f5bdda3cbf&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

> 양 끝단의(end to end) 기기의 신뢰성 있는 데이터 통신을 위해, TCP 방식이 연결을 설정하는 방식
> 

이 방식은 세 단계를 통해 연결 설정을 한다.

1. SYN : 처음으로, sender는 reciever와 연결 설정을 위해, segment를 랜덤으로 설정된 SYN(Synchronize Sequence Number)와 함께 보낸다. 이 요청은 reciever에게 sender가 통신을 시작하고 싶다고 알린다. 
2. SYN / ACK : reciever는 받은 요청을 바탕으로 SYN/ACK 신호 세트를 응답한다. Acknowledgement(ACK) 응답으로 보내는 segment가 유효한 SYN 요청을 받았는지를 의미한다.
3. ACK : 마지막 단계에서, sender는 받은 ACK를 receiver에게 전송을 하면서, 신뢰성 있는 연결이 성립되었다는 사실을 sender와 receiver 양쪽에서 알 수 있고, 실제 데이터 전송이 시작된다.

## UDP

신뢰성 좋은 TCP를 두고 왜 UDP를 사용할까?

- 애플리케이션의 정교한 제어가 가능하다 : TCP의 경우 receiver가 전송 받을 준비가 될 때까지 세그먼트를 반복적으로 재전송한다. 실시간 전송에 대한 요구가 큰 애플리케이션들은 높은 latency를 지양하므로 약간의 데이터 손실을 감수한다. 대신 개발자 스스로가 이를 보완하기 위해 애플리케이션에 추가 기능을 구현할 수 있다.
- 연결설정에 무관한다 : TCP 3-way handshake가 없는 UDP는 예비과정 없이 바로 전송을 시작한다. 설정 단계에서 발생하는 지연이 없는 만큼 반응속도가 빠르다. 또한, TCP는 신뢰성을 위해 많은 파라미터와 정보 전달이 필요함과 비교해 UDP는 연결설정 관리를 하지 않기 때문에 어떠한 파라미터도 기록하지 않는다. 이 때문에 서버에서도 TCP와 비교해 더 많은 클라이언트를 수용할 수 있다.