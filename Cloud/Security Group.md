# 목차
* [목차](#목차)
* [Security Group](#security-group)
    + [보안 그룹(Security Group)이란?](#보안-그룹security-group이란)
        + [인바운드](#인바운드)
        + [아웃바운드](#아웃바운드)

# Security Group

## 보안 그룹(Security Group)이란?

![r0ZCpGXZ_002.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/5863d20c-b612-4f1c-8261-687600e66b63/r0ZCpGXZ_002.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221204%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221204T070429Z&X-Amz-Expires=86400&X-Amz-Signature=56bf1e71332b21fed1526a9e53658983f6f07aa82363d01458db30cde763b28b&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22r0ZCpGXZ_002.png%22&x-id=GetObject)

> 보안 그룹이란 인스턴스로 들어가고 인스턴스에서 나가는 트래픽에 대한 가상 방화벽이다. 인바운드와 아웃받운드에 대한 규칙을 설정할 수 있다.
> 
- 인스턴스로 들어가는 트래픽은 인바운드, 인스턴스에서 나가는 트래픽을 아웃받운드라고 한다.

### 인바운드

![csHW_WGF_003.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ebac8316-d842-4c86-ad3b-3604b535b058/csHW_WGF_003.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221204%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221204T070443Z&X-Amz-Expires=86400&X-Amz-Signature=46a85bb2b65b8ee47277efc37a422fe5f41fc59cf863e4231424add364e62da1&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22csHW_WGF_003.png%22&x-id=GetObject)

- 인바운드 규칙에 허용되지 않은 규칙은 인스턴스로 접근하지 못하도록 필터링된다.
- EC2 인스턴스를 생성하면 기본적으로 SSH 접속을 위한 SSH 규칙만 생성되어 있다.

### 아웃바운드

- 아웃바운드 규칙은 EC2 인스턴스에서 나가는 트래픽에 대한 규칙이다.
- EC2 인스턴스를 생성한 후 따로 설정하지 않으면 기본적으로 EC2에서 나가는 모든 트래픽을 허용한다.

![r0ZCpGXZ_004.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/162b1ac7-0314-46cc-891e-45652bd268e1/r0ZCpGXZ_004.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221204%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221204T070457Z&X-Amz-Expires=86400&X-Amz-Signature=7f81258aa6594e1937602218ddabccbb9b014e4fcca01a5455108907cbcd43f4&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22r0ZCpGXZ_004.png%22&x-id=GetObject)