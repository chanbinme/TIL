# 목차
* [목차](#목차)
* [3Tier Architecture 배포 전략](#3tier-architecture-배포-전략)
    + [Client 배포](#client-배포)
    + [Server Application 배포](#server-application-배포)
    + [Database 배포](#database-배포)
    + [DNS](#dns)

# 3Tier Architecture 배포 전략

## Client 배포

- S3 서비스를 통해 이용자들에게 Client를 제공할 수 있다.
- 클라이언트 앱을 정적 파일로 빌드하여 제공한다.
- 따라서 S3를 이용해서 클라이언트를 배포한다.
- 이 때 빌드를 해주어야 한다.
    - 빌드란 불필요한 데이터를 없애고, 통합/압축하여 배포하기 최적화된 상태를 만드는 것을 말한다.
    - 빌드를 하면 데이터의 용량이 줄어들고 웹 사이트 로딩 속도가 빨라진다.
    - asset 자체가 정적인 경우, 있는 그대로 배포한다.
    - React의 경우 npm run build와 같은 명령을 사용해서, 정적 파일 형태의 결과물을 만들어 낸 후 배포한다.
    - 사용하고 있는 환경에 따라 빌드 과정은 다를 수 있다.

## Server Application 배포

- AWS EC2 서비스를 통해 손쉽게 서버를 구상하고 서비스를 제공할 수 있다.

## Database 배포

- AWS가 유지 보수 작업을 담당하는 RDS를 이용하여 즉시 데이터베이스를 사용할 수 있다.
- RDS 서비스를 이용하여 EC2를 통해 배포된 Server Application의 데이터를 저장, 제공하는 데이터베이스를 배포할 수 있다.

## DNS

- Google과 같은 웹 사이트에 접속할 때 www.google.com이라는 도메인을 통해 접속이 가능하다.
- 하지만 내가 만든 서비스는 현재 IP주소 혹은 AWS에서 제공한 서비스와는 전혀 연관이 없는 도메인으로 접속한다.
- 도메인을 통해 서비스에 접근하려면, AWS의 서비스 중 Routes53을 이용해야 한다.
- 이를 통해 직관적인 도메인 주소로 서비스에 접근할 수 있다.