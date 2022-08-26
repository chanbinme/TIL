# 목차
* [목차](#목차)
* [CLI](#cli)
    + [프로그래밍에서 CLI가 중요한 이유](#프로그래밍에서-cli가-중요한-이유)
* [CLI 요소](#cli-요소)
    + [terminal](#terminal터미널)
    + [shell](#shell셸)
    + [kernel](#kernal커널)
* [CLI 기본 명령어](#cli-기본-명령어)
    + [pwd](#pwd--현재위치-확인)
    + [mkdir](#mkdir--새로운-폴더-생성하기)
    + [ls](#ls--특정-폴더에-포함된-파일이나-폴더-확인하기)
    + [cd](#cd--폴더에-진입하기)
    + [touch](#touch--파일-생성하기)
    + [>](#실행-결과를-파일로-저장하기)
    + [cat](#cat--파일의-내용을-터미널에-출력하기)
    + [rm](#rm--폴더나-파일-삭제하기)
    + [mv](#mv--폴더나-파일의-이름을-변경-또는-폴더나-파일의-위치-옮기기)
    + [cp](#cp--폴더나-파일을-복사하기)
* [Read, Write, Execute 권한](#read-write-execute-권한)
    + [폴더인지 파일인지 확인하기](#폴더인지-파일인지-확인하기)
    + [chmod](#chmod--권한을-변경하는-명령어)

# CLI
```
명령줄 인터페이스(CLI, Command-Line Interface)는 오직 문자열로만 이루어진 컴퓨터를 제어하는 환경을 의미한다
```
* 이 환경에서는 문자열을 출력하거나 문자열을 입력하는 것만이 가능하다.
* 그래픽 사용자 인터페이스(GUI, Graphic User Interface)는 사용자가 편리하게 사용할 수 있도록 기능을 그래픽으로 나타낸 환경을 의미한다.
* GUI가 할 수 있는 모든 작업은 CLI로 할 수 있다.

## 프로그래밍에서 CLI가 중요한 이유
* CLI는 화면에 나타날 내용에 그래픽 작업을 거치지 않기 때문에 컴퓨터 자원을 적게 사용하며, 더 빠르게 작동한다.
* 명령어로 입력하기 때문에 단순하고 명확하게 사용 가능하다.
* 아마존 웹 서비스(AWS, Amazon Web Service)를 CLI를 통해 작동시킬 수 있다.

## CLI 요소

### Terminal(터미널)
* 터미널은 CLI를 사용하는 물리적/가상적 기계이다.
* 터미널도 여러가지 프로그램이 있다. 맥OS의 경우 기본 프로그램은 터미널이지만, 개발자들은 다양한 기능을 제공하는 [iTem2](https://iterm2.com/downloads.html)를 더 선호한다.

### Shell(셸)
* 셸은 터미널에서 실행 가능한 대화형 프로그램이다.
* 셸은 cLI로 운영체제 커널에 명령을 내릴 수 있는 인터렉티브 프로그램이다.
* 셸은 REPL이라고도 이야기한다. REPL은 Read-Eval-Print Loop의 줄임말로 셸이 동작하는 방식에 대해서 잘 드러낸다.
```
    1. 사용자가 명령어를 입력한다.
    2. 명령어을 셸이 읽어들인다. (Read)
    3. 명령어를 해석해서 실행한다. (Eval)
    4. 실행된 결과를 출력한다. (Print)
    5. 셸은 이 과정을 반복한다. (Loop)
```

### kernal(커널)
* 소프트웨어와 하드웨어간의 커뮤니케이션을 관리하는 프로그램이다.
* 운영체제에서 가장 중요한 구성요소로서 입출력을 관리하고 소프트웨어로부터의 요청을 컴퓨터에 있는 하드웨어(CPU, 메모리, 저장장치 등)가 처리 할 수 있도록 요청을 반환하는 역할을 한다. 
* 하드웨어를 관리하고 필요한 프로세스를 나눠주는 등 시스템 자원을 제어하고, 컴퓨터 부팅시 부트로더에 의해 로드되어 항상 메모리에 상주한다.

![커널과 쉘](https://i.imgur.com/nz3UXlY.png)
**사용자(명령) -> 쉘(해석) -> 커널(명령 수행 후 결과 전송) -> 쉘(해석) -> 사용자(결과 확인)**

## CLI 기본 명령어
### `pwd` : 현재위치 확인
* 현재 폴더가 위치한 경로를 확인하기 위해 사용
* `pwd`는 print working directory의 약자로, 여기서 말하는 디렉토리(directory)는 폴더라고도 한다.
* 명령어 `pwd`를 입력하고 `Enter`를 눌면, 컴퓨터는 현재 작업 중인 폴더의 위치를 출력한다. 
```
pwd
```
![pwd](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/e89ed3d7-b604-44dc-aad0-7114127d4591/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-08-26_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.23.05.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220826%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220826T140440Z&X-Amz-Expires=86400&X-Amz-Signature=36d4a574a1a6ef0294a9f9e54395dbf8115ba16767f999af4e4537362a44abc9&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-08-26%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB%252010.23.05.png%22&x-id=GetObject)

### `mkdir` : 새로운 폴더 생성하기
* CLI에서 폴더를 생성하기 위해 사용하는 명령어
* `mkdir`의 약자는 make directories의 약자로, 폴더를 만드는 명령을 컴퓨터에 전달한다.
```
mkdir [만들려는 폴더 이름]
```

### `ls` : 특정 폴더에 포함된 파일이나 폴더 확인하기
* `ls`는 list의 약자로, 특정 폴더에 포함된 파일이나 하위 폴더의 리스트를 출력하는 명령어
```
ls
```
![ls](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/934b9d2a-2e9a-44dc-b04d-37ea3e199ddd/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-08-26_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.31.46.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220826%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220826T140909Z&X-Amz-Expires=86400&X-Amz-Signature=cf37429758ea6e42e840f260353009180a1df31e9632da43ab34470d19e7ec4c&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-08-26%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB%252010.31.46.png%22&x-id=GetObject)
* 명령어 `ls`에는 자주 사용하는 옵션 `l`과 `a`가 있다.
* CLI에서 특정 명령어의 옵션을 사용하는 경우에는 `-` 를 이용해 옵션을 입력했다고 컴퓨터에 전달한다.
* 옵션을 뜻하는 대쉬(dash, `-` ) **뒤에 오는 옵션의 순서는 기능에 영향을 미치지 않는다.**
    + `ls -l` : 폴더나 파일의 포맷을 전부 출력
    + `ls -a` : 숨어있는 폴더나 파일을 포함한 모든 항목을 터미널에 출력. a는 all의 약자
    + `ls -al` 또는 `ls -la`
- `ls -l` 을 사용하면, 가장 왼쪽에 출력되는 두 글자 `d` 와 `-` 를 확인할 수 있다.
    - `d` 는 디렉토리를, `-` 는 파일을 나타낸다.
    - 디렉토리는 `cd` 를 통해 진입할 수 있지만, 파일은 진입할 수 없다.
![ls -l](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b5d3cdca-4c2c-469f-9eb1-37a27e3d637a/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-08-26_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.43.59.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220826%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220826T141220Z&X-Amz-Expires=86400&X-Amz-Signature=b678fc6e2f34505620b925a9ff6c7cb56d142fe888bd887d325f9e266226396b&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-08-26%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB%252010.43.59.png%22&x-id=GetObject)

### `cd` : 폴더에 진입하기
- `cd`는 change directory의 약자로, 프롬프트로 상호작용하는 폴더를 다른 폴더로 변경한다.
- `cd` 뒤에 경로를 입력하면, 해당 경로로 한 번에 이동할 수 있다.
```
cd [이동하려고 하는 경로]

cd ~/helloworld/hello
```

### `touch` : 파일 생성하기
* 파일을 만들어주는 명령어
```
touch [만드려는 파일 이름 및 확장자]

touch hi.txt
```

### `>` : 실행 결과를 파일로 저장하기
* `>`를 사용하면 실행 결과를 파일로 저장할 수 있다.
- `ls > ls.txt`를 입력하면 ls.txt에 `ls` 명령어의 실행 결과가 저장된다. 
- `echo`는 `echo`뒤의 내용을 화면에 출력해주는 역할이다. 즉, `echo hello`라고 입력하면 단순히 화면에 `hello`를 출력해준다.
```
echo gksmfcksqls@gmail.com > hi.txt
```
<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/c3bcb1f4-75ed-4841-8d5a-11500c8b5ddf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-08-26_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.55.46.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220826%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220826T142322Z&X-Amz-Expires=86400&X-Amz-Signature=07b956ba337517ec647570e0e9d1bd511bbde90d2149bf2bf60ca689435c4e4e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-08-26%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB%252010.55.46.png%22&x-id=GetObject" width="60%">

### `cat` : 파일의 내용을 터미널에 출력하기
- 파일의 내용을 확인할 수 있는 명령어
- `cat` 을 이용하여 터미널에 출력하면, 파일의 모든 내용을 출력한다. 원격 서버에 접속한 상태에서, 너무 큰 파일을 출력하는 일은 굉장히 비효율적이다. 이와 관련하여, `cat` 대신에 사용할 수 있는 명령어들이 존재한다.

```css
cat [내용을 확인하려는 파일 이름]

cat hi.txt
```

### `rm` : 폴더나 파일 삭제하기
* `rm`은 remove의 약자로, 폴더나 파일을 삭제할 때 사용한다.
* **`rm` 으로 삭제한 폴더나 파일은, 휴지통을 거치지 않고 삭제된다.**
```css
rm [삭제하려는 파일 이름]

rm bye.txt
```
- 명령어 `rm` 은 단일 파일을 삭제할 수 있다. 만약 폴더를 삭제하려면 옵션을 이용해야 한다.

```css
rm -rf [삭제하려는 폴더 이름]

rm -rf bye
```
- 옵션 `r` 과 `f` 를 사용하면 폴더를 삭제할 수 있다.
    - `rm -r` : 폴더를 지울 때 사용. recursive를 뜻한다.
    - `rm -f` : 질문을 받지 않고 지울 때 사용. force를 뜻한다.

### `mv` : 폴더나 파일의 이름을 변경, 또는 폴더나 파일의 위치 옮기기
- `mv` 는 move의 약자로, 폴더나 파일을 이동할 때 사용한다.
```css
mv [폴더나 파일의 이름] [도착 폴더의 경로]
mv bye.txt bye/
```
- `mv` 는 폴더나 파일의 이름을 변경할 수 있다.
```css
mv [변경할 폴더나 파일의 이름] [변경하고자 하는 파일의 이름]
mv bye.txt helloworld.txt
```

### `cp` : 폴더나 파일을 복사하기

- `cp` 는 copy의 약자로, 폴더나 파일을 복사할 때 사용한다.
```css
cp [원본 파일 이름] [복사할 파일 이름]

cp helloworld.txt hiComputer.txt
```

- 폴더를 복사할 때는 `rm` 과 똑같이 옵션 `r` 과 `f` 를 사용한다.
```css
cp -rf [원본 폴더 이름] [복사할 폴더 이름]

cp -rf bye hi
```

## Read, Write, Execute 권한

### 폴더인지 파일인지 확인하기

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ef57f393-f78f-4d85-bfc2-19c911c7fa3a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220826%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220826T143827Z&X-Amz-Expires=86400&X-Amz-Signature=eee10eeba2fdc78f669b7f12169c32121c9528ee413915399aab3d7586b6e225&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- 표현의 첫 시작인 `-`와 `d` 는 각각 not directory와 directory를 나타냅니다. 즉, 폴더이면`d`로, 파일이면 `-`로 나타낸다.
- 이어지는 `r`, `w`, `x` 는 각각 read permission, wrtie permission, execute permission으로 읽기 권한, 쓰기 권한, 실행 권한을 나타낸다.
- 3번에 걸쳐 나타나는 이유는 사용자와 그룹, 나머지에 대한 권한을 표시하기 때문이다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4b6f8591-1688-43a4-8506-d44a455c844c/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220826%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220826T143705Z&X-Amz-Expires=86400&X-Amz-Signature=37f51a906b6d7756408cd35eb58cddc63e9cde818fc5e86e2d15041629fcbf5a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

### user, group, and other

- user : user는 파일의 소유자이다. 기본적으로 파일을 만든 사람이 소유자가 된다. 따라서 user를 소유자(owner)라고 하기도 한다.
- group : group에는 여러 user가 포함될 수 있다. 그룹에 속한 모든 user는 파일에 대한 동일한 group 액세스 권한을 갖는다. 많은 사람이 파일에 액세스해야 하는 프로젝트가 있다고 가정한다. 각 user에게 일일이 권한을 할당하는 대신에 모든 user를 group에 추가하고, 파일에 group 권한을 할당할 수 있다.
- other : 파일에 대한 액세스 권한이 있는 다른 user이다. 파일을 만들지 않은 다른 모든 user를 의미한다. 따라서 other 권한을 설정하면, 해당 권한을 global 권한 설정이라고 볼 수도 있다.

### `chmod` : 권한을 변경하는 명령어

- 폴더나 파일의 읽기, 쓰기, 실행 권한을 변경할 수 있다.
- OS에 로그인한 사용자와, 폴더나 파일의 소유자가 같을 경우에 명령어 `chmod` 로 폴더나 파일의 권한을 변경할 수 있다.
- 만약 OS에 로그인한 사용자와, 폴더나 파일의 소유자가 다를 경우에는 관리자 권한을 획득하는 명령어 `sudo`를 이용해 폴더와 파일의 권한을 변경할 수 있다.
- 명령어 `chmod`로 권한을 변경하는 방식은 두 가지가 있다.
    1. Symbolic method : 더하기(`+`), 빼기(`-`), 할당(`=`)과 액세서(accessor) 유형을 표기해서 변경하는 방법
        
        
        | Access class | Operator | Access Type |
        | --- | --- | --- |
        | u (user) | + (add access) | r (read) |
        | g (group) | - (remove access) | w (write) |
        | o (other) | = (set exact access) | x (execute) |
        | a (all: u, g, o) |  |  |
        
        ```java
        chmod g-r filename // removes read permission from group
        chmod g+r filename // adds read permission to group
        chmod g-w filename // removes write permission from group
        chmod g+w filename // adds write permission to group
        chmod g-x filename // removes execute permission from group
        chmod g+x filename // adds execute permission to group
        chmod o-r filename // removes read permission from other
        chmod o+r filename // adds read permission to other
        chmod o-w filename // removes write permission from other
        chmod o+w filename // adds write permission to other
        chmod o-x filename // removes execute permission from other
        chmod o+x filename // adds execute permission to other
        chmod u+x filename // adds execute permission to user
        ```
        
    2. Absolute form : 두 번째는 rwx를 3 bit로 해석하여, 숫자 3자리로 권한을 표기해서 변경하는 방법
        - Absoulute form은 숫자 7까지 나타내는 3 bits의 합으로 표기한다.
        
        | Permission | Nubmer |
        | --- | --- |
        | Read (r) | 4 |
        | Write (w) | 2 |
        | Excute (x) | 1 |
        
        | # | Sum | rwx | Permisiion |
        | --- | --- | --- | --- |
        | 7 | 4(r) + 2(w) + 1(x) | rwx | read, write, execute |
        | 6 | 4(r) + 2(w) + 0(-) | rw- | read, write |
        | 5 | 4(r) + 0(-) + 1(x) | r-x | read and execute |
        | 4 | 4(r) + 0(-) + 0(-) | r— | read only |
        | 3 | 0(-) + 2(w) + 1(x) | -wx | write, execute |
        | 2 | 0(-) + 2(w) + 0(-) | -w- | write only |
        | 1 | 0(-) + 0(-) + 1(x) | —x | execute only |
        | 0 | 0(-) + 0(-) + 0(-) | —- | none |
        
        ```java
        chmod 744 hello.java 
        ```