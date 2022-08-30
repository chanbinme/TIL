# 목차
- [목차](#목차)
- [Git](#git)
- [Github](#github)
- [Git Repository](#git-repository)
- [Working Tree(Working Directory)](#working-treeworking-directory)
- [Staging Area](#staging-areaindex)
- [Fork](#fork)
- [clone](#clone)
- [commit](#commit)
- [push](#push)
- [pull](#pull)

# Git & Github 기본 개념

## Git
```java
Git이란 개발자의 코드를 효율적으로 관리하기 위해서 개발된 '분산형 버전 관리 시스템'이다.
쉽게 말해 파일을 관리해주는 프로그램이다. 
```

- Linux OS를 만든 리누스 토르발즈가 만든 프로그램이다. 로컬에서 버전을 관리해준다.
- 버전관리, 백업, 그리고 협업과 관련된 기능이 있다.
- 파일의 변경 사항을 추적하며, 사용자가 각 파일의 **버전을 관리**할 수 있게 도와준다.
- 파일을 **백업**할 수 있게 해준다.
- **협업**자들과 함께 파일을 공유하고, 각자의 작업물을 취합할 수 있게 해준다.
- 이전 버전을 선택해 손쉽게 버전을 되돌릴 수 있다.
- 특정 버전에서 두 버전으로 분기한 후, 분기한 두 버전을 각기 다르게 수정할 수 있다.(EX. Window용, Mac용)

### 버전 관리를 사용하는 이유
1. 변경되는 이력들을 저장해둘 수 있다.
2. 이전 버전으로 되돌아갈 수 있다.
3. 누가 어떤 파일을 추가, 수정, 삭제했는지 확인 가능하다.
4. Git으로 관리되는 파일은 Github, GitLab, Bitbucket 등의 여러 가지 원격 저장소를 이용해서 백업과 협업을 할 수 있다.

## Github
```java
Github는 Git Repository를 관리할 수 있는 클라우드 기반 서비스이다. 쉽게 말해 내 컴퓨터에서 Git으로 관리하는 프로젝트를 올려둘 수 있는 사이트이다.
```

- 원격 저장소 기능르 제공해주는 서비스이다.
- Github에서 Code Review 등을 통해 협업이 가능하다. 개발자들의 SNS라고 볼 수 있다.
- 수많은 오픈 소스 프로젝트들이 Github으로부터 호스팅되고 있어서, 누구든 자유롭게 기여할 수 있다. 이 작업을 contribute(기여하다)라고 한다.

## Git Repository
```java
Git Repository는 Git으로 관리되는 폴더를 말한다.
```

- Git Repository는 두 종류의 저장소를 제공한다.
    - Local Repository(로컬 저장소) : PC에 파일이 저장되는 개인 전용 저장소. 변경된 파일을 푸쉬(push)하여 원격 저장소에 업로드한다.
    - Remote Repository(원격 저장소) : 파일이 전용 서버에서 관리되며 여러 사람이 함께 공유하기 위한 저장소

## Working Tree(Working Directory)
```java
실제로 작업하고 있는 폴더, 디렉터리
```

## Staging Area(index)
```java
곧 커밋한 파일들에 대한 정보를 저장하는 공간이다.
```

## Fork
```java
Fork는 원격 저장소를 내 원격 저장소로 가지고 오는 작업을 말한다.
```
- 프로젝트에 contribute하기 위해서는 먼저 프로젝트 Fork해야 한다.

## clone
```java
Clone은 원격 저장소에 있는 코드를 내 컴퓨터로 가져오는 작업을 말한다.
```
- 코드를 수정하기 위해서는 프로젝트를 내 컴퓨터로 clone해야 한다.

## commit
```java
commit은 마무리된 작업에 작업이력을 기록해서 저장소로 보내는 행위를 말한다. 
staging area에 tracked된 파일들을 복사하여 Local Repository에 저장한다.
```

## Push
```java
Push는 내 컴퓨터에서 변경된 소스코드를 기록해 놓은 commit을 원격 저장소로 업로드하는 작업을 말한다.
```
- Push 후 Github의 Pull request 기능을 통해, 내가 제안한 코드 변경사항에 대해 반영 여부를 요청할 수 있다.

## Pull
```java
Pull은 원격 저장소에서 변경된 소스코드를 내 컴퓨터로 가져오는 작업을 말한다.
```