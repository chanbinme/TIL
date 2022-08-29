# 목차
* [목차](#목차)
- [`init`](#init)
- [`clone`](#clone)
- [`add`](#add)
- [`commit`](#commit)
- [`remote add`](#remote-add)
- [`remote -v`](#remote-add)
- [`pull`](#pull)
- [`push`](#push)
- [`restore`](#restore)

# Git 기본 명령어

## init

`init` : 기존 디렉토리를 Git Repository로 변환하거나 새로운 Repository를 생성하는데 사용
```java
git init
```

## clone

`clone` : 저장소의 디렉토리를 복제/다운로드하는데 사용
```java
git clone <다운로드 할 디렉토의 URL>
```

## add

`add` : 파일을 staging area에 추가하기 위해 사용하는 명령어. add된 파일은 commit 할 수 있는 상태가 된다. 
```java
git add <add할 파일 이름>
```

## commit

`commit` : 변경된 내용을 Local Directory에 저장한다.
```java
git commit -m 'message'
```

## remote add

`remote add` : Remote repository를 연결하고 관리하는 명령어
```java
git remote add <원격 저장소 이름> <연결할 repository의 URL>
```

## remote -v

`remote -v` : 설정된 원격 저장소를 보여주는 명령어
```java
git remote -v
```

## pull

`pull` : Remote Repository의 파일을 가져와서 Local 브랜치와 merge해주는 명령어
```java
git pull <원격 저장소 이름> <브랜치 이름>
```

## push

`push` : Remote Repository에 변경된 파일을 업로드해주는 명령어
```java
push <저장소 이름> <브랜치 이름>
```

## restore

`restore` : Working tree의 변경된 파일을 복원해주는 명령어
```java
git restore <파일 이름>
```

## reset

`reset --soft HEAD^` : commit을 취소하고 해당 파일들을 스테이징 영역에 보존해주는 명령어
```java
git reset --soft HEAD^
```

`reset HEAD^` : commit을 취소하고 해당 파일들을 Unstaging해주는 명령어
```java
git reset HEAD^

`reset --hard HEAD^` : commit을 취소하고 해당 파일들의 변경점을 삭제하는 명령어
```java
git reset --hard HEAD^
```