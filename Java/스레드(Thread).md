# 목차
* [목차](#목차)
* [스레드(Thread)](#스레드thread)
    + [메인 스레드(Main thread)](#메인-스레드main-thread)
    + [멀티 스레드(Multi thread)](#멀티-스레드multi-thread)
    + [작업 스레드의 생성과 실행](#작업-스레드의-생성과-실행)
        + [첫 번째 방법](#1-runnable-인터페이스를-구현한-객체에서-run-을-구현하여-스레드를-생성하고-실행하는-방법)
        + [두 번째 방법](#2-thread-클래스를-상속-받은-하위-클래스에서-run-을-구현하는-스레드를-생성하고-실행하는-방법)
    + [익명 객체를 사용하여 스레드를 생성하고 실행하기](#익명-객체를-사용하여-스레드-생성하고-실행하기)
        + [Runnable 익명 구현 객체](#runnable-익명-구현-객체를-활용한-스레드-생성-및-실행)
        + [Thread 익명 하위 객체](#thread-익명-하위-객체를-활용한-스레드-생성-및-실행)
    + [스레드의 이름](#스레드의-이름)
        + [스레드의 이름 조회하기](#스레드의-이름-조회하기)
        + [스레드의 이름 설정하기](#스레드의-이름-설정하기)
        + [스레드의 인스턴스 주속값 얻기](#스레드의-인스턴스의-주소값-얻기)
    + [스레드의 동기화](#스레드의-동기화)
    + [임계 영역(Critical Section)과 락(Lock)](#임계-영역critical-section과-락lock)
        + [메서드 전체를 임계 영역으로 지정](#메서드-전체를-임계-영역으로-지정)
        + [특정한 영역을 임계 영역으로 지정](#특정한-영역을-임계-영역으로-지정)
    + [스레드의 상태와 실행 제어 메서드](#스레드의-상태와-실행-제어-메서드)
        + [`sleep(long milliSecond)`](#sleeplong-millisecond)
        + [`interrupt()`](#interrupt)
        + [`yield()`](#yield)
        + [`join()`](#join)
        + [`wait()`, `notify()`](#wait-notify)
    

# 스레드(Thread)

> 프로세스 내에서 실행되는 소스 코드의 실행 흐름
> 
- **프로세스는 실행 중인 애플리케이션을 의미한다.**
- **프로세스 내에서 실행되는 소스 코드의 실행 흐름**을 스레드라고 한다.
- 프로세스는 데이터, 컴퓨터 자원, 그리고 스레드로 구성되어 있다.
- **스레드는 데이터와 애플리케이션이 확보한 자원을 활용하여 소스 코드를 실행한다.**

## 메인 스레드(Main thread)

- 자바 애플리케이션을 실행하면 가장 먼저 실행되는 메서드
- 메인 스레드가 `main` 메서드를 실행시켜준다.
- `main` 메서드의 코드를 처으부터 끝까지 순차적으로 실행시키며, 코드의 끝을 만나거나 `return` 문을 만나면 실행을 종료한다.

## 멀티 스레드(Multi-Thread)

- 멀티 스레드 프로세스는 하나의 프로세스로 여러 개의 스레드를 가질 수 있다.
- 여러 스레드가 동시에 작업을 수행할 수 있으며, 이름 멀티 스레딩이라고 한다.
- 예를 들어, 카카오톡에서 사진을 업로드하면서 동시에 메세지를 주고 받을 수 있는 것도 멀티 스레드를 사용중이기 때문이다.

## 작업 스레드의 생성과 실행

- 메인 스레드 외에 별도의 작업 스레드를 활용한다는 것은, **작업 스레드가 수행할 코드를 작성하고, 작업 스레드를 생성하여 실행시키는 것을 의미한다.**
- 작업 스레드가 수행할 코드도 클래스 내부에 작성해주어야 하며, `run()` 이라는 메서드 내에 스레드가 처리할 작업을 작성하도록 규정되어져 있다.
- `run()` 메서드는 Runnable 인터페이스와 Thread클래스에 정의되어져 있다. 작업 스레드를 생성하고 실행하는 방법은 두 가지가 있다.
    1. Runnable 인터페이스를 구현한 객체에서 `run()` 을 구현하여 스레드를 생성하고 실행하는 방법
    2. Thread 클래스를 상속 받은 하위 클래스에서 `run()` 을 구현하여 스레드를 생성하고 실행하는 방법

### 1. Runnable 인터페이스를 구현한 객체에서 `run()` 을 구현하여 스레드를 생성하고 실행하는 방법

```java
public class ThreadExample {
    public static void main(String[] args) {

        // Runnable 인터페이스를 구현한 객체 생성
        Runnable task1 = new ThreadTask1();;

        // Runnable 구현 객체를 인자로 전달하면서 Thread 클래스를 인스턴스화하여 스레드 생성
        Thread thread1 = new Thread(task1);

        // 위의 두 줄을 아래와 같이 한 줄로 축약할 수도 있다.
        // Thread thread1 = new Thread(new ThreadTask1());

        // 작업 스레드를 실행시켜, run() 내부의 코드 처리하도록 한다.
        thread1.start();

				// 메인 스레드 반복문
        for (int i = 0; i < 100; i++) {
            System.out.print("@");
        }
    }
}

//Runnable 인터페이스를 구현하는 클래스
class ThreadTask1 implements Runnable {
    public void run() {
        // run() 메서드 바디에 스레드가 수행할 작업 내용 작성
        for (int i = 0; i < 100; i++){
            System.out.print("#");
        }
    }
}

// 결과 
// 매 실행 시마다 다르게 출력된다.
@@@@@@@@@@@######@@@@@############################
@#########@@@@@@@@@@@@@@@@############@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@##@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@###########################################
```

- 메인 스레드와 작업 스레드가 동시에 병렬로 실행되면서 `@` 와 `#` 이 섞여서 출력됐다.

### 2. Thread 클래스를 상속 받은 하위 클래스에서 `run()` 을 구현하는 스레드를 생성하고 실행하는 방법

- Runnable 인터페이스로 생성할 때와 달리 직접 인스턴스화하지 않아도 된다.

```java
package test1;

public class ThreadExample2 {
    public static void main(String[] args) {
        // Thread 클래스를 상속받은 클래스를 인스턴스화하여 스레드를 생성
        // Runnable로 생성할 때와 달리 직접 인스턴스화 하지 않는다.
        ThreadTask2 thread2 = new ThreadTask2();

        // 작업 스레드를 실행시켜, run() 내부의 코드를 처리
        thread2.start();

        // 반복문 추가
        for (int i = 0; i < 100; i++) {
            System.out.print("@");
        }
    }
}

// Thread 클래스를 상속받는 클래스 작성
class ThreadTask2 extends Thread {
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.print("#");
        }
    }
}

// 결과
// 결과 
// 매 실행 시마다 다르게 출력된다.
@@@@@@@@@@@######@@@@@############################
@#########@@@@@@@@@@@@@@@@############@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@##@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@###########################################
```

## 익명 객체를 사용하여 스레드 생성하고 실행하기

### Runnable 익명 구현 객체를 활용한 스레드 생성 및 실행

```java
public class ThreadExample1 {
    public static void main(String[] args) {
        // 익명 Runnable 구현 객체를 활용하여 스레드 생성
        Thread thread1 = new Thread(new Runnable() {
            public void run() {
                for (int i = 0; i < 100; i++) {
                    System.out.print("#");
                }
            }
        });

        thread1.start();

        for (int i = 0; i < 100; i++) {
            System.out.print("@");
        }
    }
}
```

### Thread 익명 하위 객체를 활용한 스레드 생성 및 실행

```java
package test2;

public class ThreadExample2 {
    public static void main(String[] args) {
        // 익명 Thread 하위 객체를 활용한 스레드 생성
        Thread thread2 = new Thread() {
            public void run() {
                for(int i = 0; i < 100; i++) {
                    System.out.print("#");
                }
            }
        };

        thread2.start();

        for (int i = 0; i < 100; i++) {
            System.out.print("@");
        }
    }
}
```

## 스레드의 이름

- 메인스레드는 “main”이라는 이름을 가지며, 그 외에 추가적으로 생성한 스레드는 기본적으로 “Thread-n”이라는 이름을 가진다.

### 스레드의 이름 조회하기

- 스레드의 이름은 `스레드의_참조값.getName()` 으로 조회할 수 있다.

```java
public class ThreadExample3 {
    public static void main(String[] args) {

        Thread thread3 = new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("Get Thread Name");
            }
        });

        thread3.start();

        System.out.println("thread3.getName() = " + thread3.getName());
    }
}

// 결과
Get Thread Name
thread3.getName() = Thread-0
```

### 스레드의 이름 설정하기

- 스레드의 이름은 `스레드의_참조값.setName()` 으로 설정할 수 있다.

```java
public class ThreadExample4 {
    public static void main(String[] args) {
        Thread thread4 = new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("Set And Get Thread Name");
            }
        });

        thread4.start();
        System.out.println("thread4.getName() = " + thread4.getName());

        thread4.setName("Code States");
        System.out.println("threadr.getName() = " + thread4.getName());
    }
}

// 결과
thread4.getName() = Thread-0
threadr.getName() = Code States
```

### 스레드의 인스턴스의 주소값 얻기

- 스레드의 이름을 조회하고 설정하는 메서드는 모두 Thread 클래스로부터 인스턴스화된 인스턴스의 메서드이므로, 호출할 때에 스레드 객체의 참조가 필요하다.
- 만약 실행 중인 스레드의 주소값을 사용해야 하는 상황이 발생한다면 Thread 클래스의 정적 메서드인 `currentThread()` 를 사용해야 한다.

```java
public class ThreadExample1 {
    public static void main(String[] args) {
        Thread thread1 = new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread());
            }
        });

        thread1.start();
        System.out.println(Thread.currentThread());
    }
}

// 결과
Thread[main,5,main]
Thread[Thread-0,5,main]
```

## 스레드의 동기화

- 프로세스는 스레드가 운영 체제로부터 자원을 할당 받아 소스 코드를 실행하여 데이터를 처리한다.
- 싱글 스레드 프로세스는 데이터에 단 하나의 스레드만 접근하기때문에 문제가 없지만, **멀티 스레드 프로세스는 두 스레드가 동일한 데이터를 공유하게 되어 문제가 발생할 수 있다.**

```java
public class ThreadExample3 {
    public static void main(String[] args) {
        Runnable threadTask3 = new ThreadTask3();
        Thread thread3_1 = new Thread(threadTask3);
        Thread thread3_2 = new Thread(threadTask3);

        thread3_1.setName("김찬빈");
        thread3_2.setName("김현경");

        thread3_1.start();
        thread3_2.start();
    }
}

class Account {

    // 잔액을 나타내는 변수
    private int balance = 1000;

    public int getBalance(){
        return balance;
    }

    // 인출 성공 시 true, 실패 시 false 반환
    public boolean withdraw(int money) {

        // 인출 가능 여부 판단 : 잔액이 인출하고자 하는 금액보다 같거나 많아야 한다.
        if (balance >= money) {
            // if문의 실행부에 진입하자마자 해당 스레드르 일시 정지 시키고,
            // 다른 스레드에게 제어권을 강제로 넘긴다.
            // 일부러 문제 상황을 발생시키기 위해 추가한 코드이다.
            try { Thread.sleep(1000); } catch (Exception error) {}

            // 잔액에서 인출금을 깎아 새로운 잔액을 기록한다.
            balance -= money;

            return true;
        }
        return false;
    }
}

class ThreadTask3 implements Runnable {
    Account account = new Account();

    public void run() {
        while (account.getBalance() > 0) {

            // 100 ~ 300원의 인출금을 랜덤으로 정한다.
            int money = (int)(Math.random() * 3 + 1) * 100;

            // withdraw를 실행시키는 동시에 인출 성공 여부를 변수에 할당한다.
            boolean denied = !account.withdraw(money);

            // 인출 결과 확인
            // 만약, withdraw가 false를 리턴하였다면, 즉 인출에 실패했다면,
            // 해당 내역에 -> DENIED를 출력한다.
            System.out.println(String.format("Withdraw %d$ By %s. Balance : %d %s.",
                    money, Thread.currentThread().getName(), account.getBalance(), denied ? "-> DENIED" : "")
            );
        }
    }
}

// 결과
// 두 스레드가 하나의 객체를 공유하면서 오류가 발생했다.
Withdraw 300$ By 김찬빈. Balance : 100 -> DENIED.
Withdraw 300$ By 김찬빈. Balance : 100 -> DENIED.
Withdraw 200$ By 김찬빈. Balance : 100 -> DENIED.
Withdraw 200$ By 김찬빈. Balance : 100 -> DENIED.
Withdraw 300$ By 김찬빈. Balance : 100 -> DENIED.
Withdraw 200$ By 김찬빈. Balance : 100 -> DENIED.
Withdraw 200$ By 김찬빈. Balance : 100 -> DENIED.
Withdraw 100$ By 김현경. Balance : -100 .
Withdraw 100$ By 김찬빈. Balance : -100 .
```

- 위 같은 오류가 발생하지 않게 하는 것을 스레드 동기화라고 한다.
- 스레드 동기화 적용을 위해서는 **임계영역과 락(Lock)**에 대한 이해가 필요하다.

## 임계 영역(Critical section)과 락(Lock)

> **임계 영역** : 오로지 하나의 스레드만 코드를 실행할 수 있는 코드 영역
> 

> **락** : 임계 영역을 포함하고 있는 객체 접근할 수 있는 권한
> 
- 임계 영역으로 설정된 객체가 다른 스레드에 의해 작업이 이루어지고 있지 않을 때, 임의의 스레드는 해당 객체에 대한 락을 획득하여 임계 영역 내의 코드를 수행한다.
- 임계 영역 내의 코드를 실행 중일 때에는 다른 스레드들은 락이 없으므로 이 객체의 임계 영역 내의 코드를 실행할 수 없다.
- 특정 코드 구간을 임계 영역으로 설정할 때에는 `synchronized` 라는 키워드를 사용한다. `synchronized` 는 두 가지 방법으로 사용할 수 있다.

### 메서드 전체를 임계 영역으로 지정

- 메서드의 반환 타입 좌측에 `synchronized` 를 작성하면 메서드 전체를 임계 영역으로 설정할 수 있다.
- 메서드 전체를 임계 영역으로 지정하면 메서드가 호출되었을 때, 메서드를 실행할 스레드는 메서드가 포함된 객체의 락을 얻는다.

```java
class Account {
	...
	public synchronized boolean withdraw(int money) {
	    if (balance >= money) {
	        try { Thread.sleep(1000); } catch (Exception error) {}
	        balance -= money;
	        return true;
	    }
	    return false;
	}
}
```

### 특정한 영역을 임계 영역으로 지정

- `synchronized` 와 함께 소괄호() 안에 해당 영역이 포함된 객체의 참조를 넣고 중괄호{}로 블럭을 열어 블럭 내에 코드를 작성해야 한다.

```java
class Account {
	...
	public boolean withdraw(int money) {
			synchronized (this) {
			    if (balance >= money) {
			        try { Thread.sleep(1000); } catch (Exception error) {}
			        balance -= money;
			        return true;
			    }
			    return false;
			}
	}
}
```

- 임계 영역을 지정해주고 다시 코드를 실행시켜보자

```java
public class ThreadExample3 {
    public static void main(String[] args) {
        Runnable threadTask3 = new ThreadTask3();
        Thread thread3_1 = new Thread(threadTask3);
        Thread thread3_2 = new Thread(threadTask3);

        thread3_1.setName("김찬빈");
        thread3_2.setName("김현경");

        thread3_1.start();
        thread3_2.start();
    }
}

class Account {

    // 잔액을 나타내는 변수
    private int balance = 1000;

    public int getBalance(){
        return balance;
    }

    // 인출 성공 시 true, 실패 시 false 반환
    public synchronized boolean withdraw(int money) {

        // 인출 가능 여부 판단 : 잔액이 인출하고자 하는 금액보다 같거나 많아야 한다.
        if (balance >= money) {
            // if문의 실행부에 진입하자마자 해당 스레드르 일시 정지 시키고,
            // 다른 스레드에게 제어권을 강제로 넘긴다.
            // 일부러 문제 상황을 발생시키기 위해 추가한 코드이다.
            try { Thread.sleep(1000); } catch (Exception error) {}

            // 잔액에서 인출금을 깎아 새로운 잔액을 기록한다.
            balance -= money;

            return true;
        }
        return false;
    }
}

class ThreadTask3 implements Runnable {
    Account account = new Account();

    public void run() {
        while (account.getBalance() > 0) {

            // 100 ~ 300원의 인출금을 랜덤으로 정한다.
            int money = (int)(Math.random() * 3 + 1) * 100;

            // withdraw를 실행시키는 동시에 인출 성공 여부를 변수에 할당한다.
            boolean denied = !account.withdraw(money);

            // 인출 결과 확인
            // 만약, withdraw가 false를 리턴하였다면, 즉 인출에 실패했다면,
            // 해당 내역에 -> DENIED를 출력한다.
            System.out.println(String.format("Withdraw %d$ By %s. Balance : %d %s.",
                    money, Thread.currentThread().getName(), account.getBalance(), denied ? "-> DENIED" : "")
            );
        }
    }
}

// 결과
Withdraw 200$ By 김찬빈. Balance : 800 .
Withdraw 100$ By 김현경. Balance : 700 .
Withdraw 200$ By 김찬빈. Balance : 500 .
Withdraw 300$ By 김현경. Balance : 200 .
Withdraw 300$ By 김찬빈. Balance : 200 -> DENIED.
Withdraw 100$ By 김현경. Balance : 100 .
Withdraw 200$ By 김찬빈. Balance : 100 -> DENIED.
Withdraw 300$ By 김현경. Balance : 100 -> DENIED.
Withdraw 200$ By 김현경. Balance : 0 -> DENIED.
Withdraw 100$ By 김찬빈. Balance : 0 .
```

올바르게 작동한다 👍

## 스레드의 상태와 실행 제어 메서드

스레드의 상태와, 스레드의 상태를 제어하는 메서드에 대해 araboza.

## 스레드 실행 제어 메서드

### `sleep(long milliSecond)`

> milliSecond 동안 스레드를 잠시 멈춘다.
> 

```java
static void sleep(long milliSecond)
```

- `sleep()` 은 스레드의 실행을 잠시 멈출 때 사용한다.(실행 상태에서 일시 정지(TIMED_WAITING) 상태로 전환)
- 인자로 얼마나 스레드를 머출 것인지 설정할 수 있지만, 지정한 시간보다 약간의 오차를 가진다.
- Thread 클래스의 메서드이다. `sleep()` 을 호출할 때는 `Thread.sleep(1000);` 과 같이 클래스를 통해 호출하는 것을 권장한다.
- `sleep()` 에 의해 일시 정지된 스레드는 두 가지 경우에 실행 대기 상태로 복귀한다.
    - 인자로 전달한 시간만큼 시간이 경과한 경우
    - `interrupt()` 를 호출한 경우
- `interrupt()` 를 호출하여 스레드를 실행 대기 상태로 복귀시키려고 할 경우에는 반드시 `try-catch` 문을 사용해서 예외 처리를 해주어야 한다. `interrupt()` 가 호출되면 예외가 발생하기 때문이다.
- 따라서 `sleep()` 을 사용할 때는 아래처럼 `try-catch` 문으로 `sleep()` 을 감싸줘야 한다.

```java
try { Thread.sleep(1000); } catch (Exception error) {}
```

### `interrupt()`

> 일시 중지 상태인 스레드를 실행 대기 상태로 복귀시킨다.
> 

```java
void interrupt()
```

- `interrupt()` 는 `sleep()` , `wait()` , `join()` 에 의해 일시 정지 상태에 있는 스레드들을 실행 대기 상태로 복귀시킨다.
- `sleep()` , `wait()` , `join()` 에 의해 일시 정지된 스레드들의 코드 흐름은 각각 `sleep()` , `wait()` , `join()` 에 멈춰져 있다.
- `멈춰있는 스레드.interrupt()` 를 호출하면 `sleep()` , `wait()` , `join()` 메서드에서 예외가 발생되면서 일시 정지가 풀리게 된다.

```java
public class ThreadExample5 {
    public static void main(String[] args) {
        Thread thread1 = new Thread() {
            // 스레드를 1초 동안 일시정지한다.
            public void run() {
                try {
                    while (true) Thread.sleep(1000);
                }
                catch (Exception e) {}
                System.out.println("Wake Up!!!");
            }
        };
        //스레드의 상태 확인
        System.out.println("thread1.getState() = " + thread1.getState());
        // 스레드를 실행 대기 상태로 만들어준다.
        thread1.start();
        // 스레드의 상태 확인
        System.out.println("thread1.getstate() = " + thread1.getState());

        // 스레드의 상태가 일시 정지 상태라면 스레드 상태를 출력하고 반복문을 중단한다.
        while(true) {
            if (thread1.getState() == Thread.State.TIMED_WAITING) {
                System.out.println("thread1.getState() = " + thread1.getState());
                break;
            }
        }

        // 일시 정지 상태인 스레드를 실행 대기 상태로 복귀시킨다.
        thread1.interrupt();

        // 스레드의 상태가 실행중인 상태라면 스레드의 상태를 출력하고 반복문을 중단한다.
        while (true) {
            if (thread1.getState() == Thread.State.RUNNABLE) {
                System.out.println("thread1.getState() = " + thread1.getState());
                break;
            }
        }

        // 스레드의 상태가 소멸된 상태라면 스레드의 상태를 출력하고 반복문을 중단한다.
        while (true) {
            if (thread1.getState() == Thread.State.TERMINATED) {
                System.out.println("thread1.getState() = " + thread1.getState());
                break;
            }
        }
    }
}

// 결과
thread1.getState() = NEW
thread1.getstate() = RUNNABLE
thread1.getState() = TIMED_WAITING
Wake Up!!!
thread1.getState() = RUNNABLE
thread1.getState() = TERMINATED
```

### `yield()`

> 다른 스레드에게 자신의 실행 시간을 양보한다.
> 

```java
static void yield()
```

- `yield()` 를 사용하면 실행 대기 상태로 바뀌며, 자신에게 남은 실행 시간을 실행 대기열 상 우선순위가 높은 다른 스레드에게 양보한다.
- 스레드를 활용할 때, 스레드에게 반복적인 작업을 시키는 경우가 많다. 그런데 특정 상황에 따라 반복문 순회가 불필요할 때가 있다.

```java
// example의 값이 false가 나와도 while문은 계속 반복된다.
public void run() {
	while (true) {
		if(example) {
				...
		}
	}
}
```

```java
// example의 값이 false가 나오면 while문의 반복은 중단된다.
// 스레드는 실행 대기 상태로 바뀌며, 자신에게 남은 실행 시간을 실행 대기열 상 우선순위가 높은 다른 스레드에게 양보한다.
public void run() {
	while (true) {
		if(example) {
				...
		}
		else Thread.yield();
	}
}
```

### `join()`

> 다른 스레드의 작업이 끝날 때까지 기다린다.
> 

```java
void join()
void join(long milliSecond)
```

- `join()` 은 특정 스레드가 작업하는 동안에 자신을 일시 중지 상태로 만드는 상태 제어 메서드이다.
- 인자로 밀리초 단위를 전달할 수 있다.
- 다음의 경우 실행 대기 상태로 복귀한다.
    - 전달한 인자만큼의 시간이 경과했을 경우
    - `interrupt()` 가 호출한 경우
    - `join()` 호출 시 지정했떤 다른 스레드가 작업을 마친 경우
- `join()` 은 `sleep()` 과 유사하다.
    - 스레드를 일시 중지 상태로 만든다.
    - `try-catch` 문으로 감싸서 사용해야 한다.
    - `interrupt()` 에 의해 실행 대기 상태로 복귀할 수 있다.
- `join()` 과 `sleep()` 의 차이점
    - `sleep()` 은 Thread 클래스이 `static` 메서드이다. (예 : `Thread.sleep(1000);`)
    - `join()` 은 특정 스레드에 대해 동작하는 인스턴스 메서드이다. (예 : `thread1.join();`)
    
    ```java
    public class ThreadExample6 {
        public static void main(String[] args) {
            SumThread sumThread = new SumThread();
    
            // to 값 10으로 선언
            sumThread.setTo(10);
            // 스레드 실행
            sumThread.start();
    
            // 메인 스레드가 sumThread의 작업이 끝날 때까지 기다림
            try { sumThread.join(); } catch (Exception e) {}
            System.out.println(String.format("1부터 %d까지의 합 : %d", sumThread.getTo(), sumThread.getSum()));
        }
    }
    
    class SumThread extends Thread {
        private long sum;
        private int to;
    
        public long getSum() {
            return sum;
        }
    
        public int getTo() {
            return to;
        }
    
        public void setTo(int to) {
            this.to = to;
        }
    
        public void run() {
            for (int i = 1; i <= to; i++) {
                sum += i;
            }
        }
    }
    
    // 결과
    55
    // join()을 하지 않았다면 0
    ```
    
    ### `wait()`, `notify()`
    
    > 스레드 간 협업에 사용된다.
    > 
    - 두 스레드가 교대로 작업을 처리해야 할 때 사용할 수 있는 상태 제어 메서드
    - `notify()` 를 호출한 스레드A의 객체는 실행 대기 상태가 되고, `wati()` 을 호출한 스레드B는 일시 정지 상태로 만든다.
    - 이후 스레드A가 작업을 완료하면 `notify()` 를 호출하여 스레드B를 다시 실행 대기 상태로 복귀시킨 후, `wait()` 을 호출하여 스레드A를 일시 정지 상태로 전환한다.
    - 이 과정이 반복되면서 두 스레드는 공유 객체에 대해 서로 효과적으로 협업할 수 있다.