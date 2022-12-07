# 목차
* [목차](#목차)
* [Automated Deployment](#automated-deployment)
    + [배포 자동화](#배포-자동화)
    + [배포 자동화 파이프라인](#배포-자동화-파이프라인)
    + [AWS 개발자 도구](#aws-개발자-도구)
    + [Question](#question)

# Automated Deployment

## 배포 자동화

> 배포 자동화란 한 번의 클릭 혹은 명령어 입력을 통해 전체 배포 과정을 자동으로 진행하는 것을 뜻한다.
> 

### 장점

- 수동적이고 반복적인 배포 과정을 자동화함으로써 시간이 절약된다.
- 배포 자동화를 통해 전체 배포 과정을 매번 일관되게 진행하는 구조를 설계하여 휴먼 에러(Human Error)를 방지할 수 있다. 휴먼 에러란 사람이 수동적으로 배포 과정을 진행하는 중에 생기는 실수들을 뜻한다.

## 배포 자동화 파이프라인

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/83c8054a-118b-4191-af29-e481cf2f4633/Untitled.png)

> 배포에서 파이프라인(Pipeline)이란 소스 코드의 관리부터 실제 서비스로의 배포 과정을 연결하는 구조를 뜻한다.
> 
- 파이프라인은 전체 배포 과정을 여러 단계(Stage)로 분리한다.
- 각 단계는 파이프라인 안에서 순차적으로 실행되며, 각 단계마다 주어진 작업(Actions)들을 수행한다.
- 파이프라인은 대표적을 세 가지 단계가 존재한다.
    1. Source 단계 : Source 단계에서는 원격 저장소에 관리되고 있는 소스 코드에 변경 사항이 일어날 경우, 이를 감지하고 다음 단계로 전달하는 작업을 수행한다.
    2. Build 단계 : Build 단계에서는 Source 단계에서 전달받은 코드를 컴파일, 빌드, 테스트하여 가공한다. 또한 Build 단계를 거쳐 생성된 결과물을 다음 단계로 전달하는 작업을 수행한다. 
    3. Deploy 단계 : Deploy 단계에서는 Build 단계로부터 전달받은 결과물을 실제 서비스에 반영하는 작업을 수행한다. 
- **파이프라인의 단계는 상황과 필요에 따라 더 세분화되거나 간소화 될 수 있다.**

## AWS 개발자 도구

- AWS에는 개발자 도구 섹션이 존재한다. 개발자 도구 섹션에서 제공하는 서비스를 활용하여 배포 자동화 파이프라인을 구축할 수 있다.
- AWS 섹션에서 제공하는 서비스에 대해 알아보자

### CodeCommit

- Source 단계를 구성할 때 CodeCommit 서비스를 이용한다.
- CodeCommit은 GitHub과 유사한 서비스를 제공하는 버전 관리 도구이다.
- GitHub과 비교할 때 CodeCommit 서비스는 보안과 관련된 기능에 강점을 가진다. 소스 코드의 유출이 크게 작용하는 기업에서는 매우 중요한 요소이다.
- 다만, 프리티어 한계 이상으로 사용할 시 사용 요금이 부과될 수도 있다.
- 사이드 프로젝트나 가볍게 작성한 소스 코드를 저장해야 할 경우에는 GitHub을 이용하는 것이 효과적이다.

### CodeBuild

- Build 단계에서는 CodeBuild 서비스를 이용한다.
- CodeBuild 서비스를 통해 유닛 테스트, 컴파일, 빌드와 같은 빌드 단게에서 필수적으로 실행되어야 할 작업들을 명령어를 통해 실행할 수 있다.

### CodeDeploy

- Deploy 단계를 구성할 때는 기본적으로 다양한 서비스를 이용할 수 있다.
- CodeDeploy 서비스를 이용하면 실행되고 있는 서버 애플리케이션에 실시간으로 변경 사항을 전달할 수 있다.
- 또한 S3 서비스를 통해 S3 버킷을 통해 업로드된 정적 웹 사이트에 변경 사항을 실시간으로 전달하고 반영할 수 있다.

### CodePipeline

- 각 단계를 연결하는 파이프라인을 구축할 때 CodePipeline 서비스를 이용한다.

## Question

- 개발자 도구 섹션에서 제공하는 다른 서비스들은 어떤 기능을 제공할까?
- CodeBuild 서비스는 사용자가 작성한 buildspec.yml 파일을 참조하여 작업을 수행한다. [AWS 공식 문서](https://docs.aws.amazon.com/ko_kr/codebuild/latest/userguide/build-spec-ref.html)를 참조하여 buildspec.yml에 대해 알아보자
- CodeDeploy 서비스는 사용자가 작성한 sppspec.yml 파일을 참조하여 작업을 수행한다. [AWS 공식 문서](https://docs.aws.amazon.com/ko_kr/codedeploy/latest/userguide/reference-appspec-file.html)를 참조하며 appspec.yml에 대해 알아보자