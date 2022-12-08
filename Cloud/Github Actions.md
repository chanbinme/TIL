# 목차
* [목차](#목차)
* [Github Actions](#github-actions)
    + [Github Actions를 통한 배포 Flow](#github-actions를-통한-배포-flow)


# Github Actions

> GIthub Actions는 Github가 공식적으로 제공하는 빌드, 테스트 및 배포 파이프라인을 자동화할 수 있는 CI/CD 플랫폼이다.
> 
- 레포지토리에서 Pull Request나 Push 같은 이벤트를 트리거로 GitHub 작업 워크플로(Workflow)를 구성할 수 있다.
- 워크플로는 하나 이상의 작업이 실행되는 자동화 프로세스로, 각 직업은 자체 가상 머신 또는 컨테이너 내부에서 실행된다.
- 워크플로는 `.yml` 파일에 의해 구성되고 테스트, 배포 등 기능에 따라 여러개의 워크플로도 만들 수 있다. 생성된 워크플로는 `.github/workflows` 디렉토리 이하에 위치한다.
- 비공개 레포지토리의 경우 GitHub Actions가 작동할 때의 용량과 시간이 제한되어있으며 공개 레포지토리는 무료로 사용 가능하다.

Github Actions 공식문서 : [https://docs.github.com/en/actions](https://docs.github.com/en/actions)

## ****Github Actions를 통한 배포 Flow****

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/396bfcc6-41b0-4e39-ac94-c2cdae7d4d88/Untitled.png)

### Github Actions

- Github Actions는 설정 파일(`.yml`)에 따라 Github Repository에 특정 변동사항을 트리거로 작동된다.
- Github Actions는 `main` 브랜치에 적용된 변동 사항을 기준으로 프로젝트를 빌드한다.
- 빌드를 마친 프로젝트를 AWS의 S3 버킷에 저장하고, Code Deploy의 S3에서 EC2로 배포 명령을 내린다.

### S3

- 일반 배포 때는 S3를 정적 웹 페이지 배포하는데에 사용했는데, 이번에는 S3를 저장소로써 사용한다.
- Gitbub Actions에서 빌드한 결과물이 압축되어 S3로 전송되고, 버킷에 저장된다.

### Code Deploy

- Github Actions에서 배포 명령을 받은 Code Deploy는 S3에 저장되어 있는 빌드 결과물을 EC2 인스턴스로 이동한다. 프로젝트 최상단에 위치한 `appepec.yml` 설정 파일에 의해 쉘 스크립트 등 단계에 따라 특정 동작을 한다.
- Code Deploy가 S3 버킷에서 EC2 인스턴스로 프로젝트를 이동할 수 있도록 EC2 인스턴스에 Code Deploy Agent의 설치가 필요하다.
- OS별, 버전별 설치 방법이 상이하기 때문에 반드시 공식문서를 참고해야 한다.

### EC2

- Code Deploy에 의해 빌드 과정을 거친 프로젝트가 EC2 인스턴스로 전달되고, `.yml` (설정 파일)과 `.sh` (쉘 스크립트)에 의해 각 배포 결과를 로그로 저장하며 빌드 파일(`.jar` )을 실행한다.
- 해당 과정이 원활히 진행되기 위해서는 EC2 인스턴스에 접속하여 알맞은 Code Deploy Agent의 설치와 JDK 11 버전 설치가 필요하다.