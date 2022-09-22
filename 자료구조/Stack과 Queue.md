# 목차
* [목차](#목차)
* [자료구조가 뭘까?](#자료구조가-뭘까)
* [Stack](#stack)
    + [Stack의 특징](#stack의-특징)
    + [Stack은 어디에서 사용할까?](#stack은-어디에서-사용할까)
    + [Stack 구현해보기](#stack-구현해보기)
* [Queue](#queue)
    + [Queue의 특징](#queue의-특징)
    + [Queue는 어디에 사용될까?](#queue는-어디에-사용될까)

# 자료구조가 뭘까?

> 자료구조란 여러 데이터의 묶음을 저장하고, 사용하는 방법을 정의하는 것
> 

데이터(data)는 무엇일까? 데이터는 **문자, 숫자, 소리, 그림, 영상 등 실생활을 구성하고 있는 모든 값**을 말한다. 

데이터는 그 자체만으로 어떤 정보를 가지기 힘들다. 예를 들어 나이라는 데이터만 알고 있다면, 사람의 나이인지, 강아지의 나이인지, 나무의 나이인지 알 수 없다. 이처럼 **데이터는 분석하고 정리하여 활용해야만 의미를 가질 수 있다.** 

그뿐만 아니라 데이터를 사용하려는 목적에 따라 형태를 구분하고, 분류하여 사용해야한다. **필요에 따라 데이터의 특징을 잘 파악(분석)하여 정리하고, 활용해야 한다는 것이다.**


이 중에서 가장 자주 등장하는 네 가지의 자료구조를 알아보자

- Stack
- Queue
- Tree
- Graph

**대부분의 자료구조는 특정한 상황에 놓인 문제를 해결하는 데에 특화되어 있다.** 그래서 많은 자료구조를 알아두면, **어떠한 상황이 닥쳤을 때 적합한 자료구조를 빠르고 정확하게 적용하여 문제를 해결할 수 있다.**

# Stack

Stack은 쌓다, 쌓이다, 포개지다와 같은 뜻을 갖고 있다. 마치 접시를 쌓아 놓은 형태와 비슷한 **Stack은 데이터(data)를 순서대로 쌓는 자료구조**이다. 

- Stack의 특징은 입력과 출력이 하나의 방향으로 이루어지는 제한적 접근에 있다.
- 이런 Stack의 자료구조의 정책을 LIFO(Last In First Out) 혹은 FILO(First In Las Out)이라고 부른다.
- Stack에 데이터를 넣는 것을 ‘PUSH’, 데이터를 꺼내는 것을 ‘POP’이라고 한다.

## Stack의 특징

### 1. LIFO(Last In First Out)

- 먼저 들어간 데이터가 가장 먼저 나온다.(후입선출)

```java
public class Test3 {
    public static void main(String[] args) {
        Stack<Integer> stack = new Stack<>(); // Integer형 스택 선언

        stack.push(1);
        stack.push(2);
        stack.push(3);
        stack.push(4);

        System.out.println(stack);
//        ---------------------
//        들어가는 순서
//        1 <- 2 <- 3 <- 4
//        ---------------------
        while (!stack.empty()) {
            System.out.println(stack.pop());
        }
//        ---------------------
//        나가는 순서
//        4 <- 3 <- 2 <- 1
//        ---------------------
    }
}

// 결과
[1, 2, 3, 4]
4
3
2
1
```

### 2. 데이터는 하나식 넣고 뺄 수 있다.

Stack 자료구조는 데이터가 아무리 많이 있어도 하나씩 데이터를 넣고, 뺀다. 한꺼번에 여러 개를 넣거나 뺄 수 없다.

### 3. 하나의 입출력 방향을 가지고 있다.

Stack 자료구조는 데이터의 입출력 방향이 같다. 만약, 입출력 방향이 여러 개라면 Stack 자료구조라고 볼 수 없다.

## Stack은 어디에서 사용할까?

대표적으로 브라우저의 뒤로 가기, 앞으로 가기 기능을 구현할 때 Stack이 활용된다.

브라우저에서 자료구조 Stack이 사용될 때에는 다음과 같은 순서를 거친다.

1. 새로운 페이지로 접속할 때, 현재 페이지를 prev Stack에 보관한다.
2. 뒤로 가기 버튼을 눌러 이전 페이지로 돌아갈 때에는, 현재 페이지를 Next Stack에 보관하고 Prev Stack에 가장 나중에 보관된 페이지를 현재 페이지로 가져온다.
3. 앞으로 가기 버튼을 눌러 앞서 방문한 페이지로 이동을 원할 때에는, Net Stack의 가장 마지막 보관된 페이지를 가져온다.
4. 마지막으로 현재 페이지를 Prev Stack에 보관한다.

## Stack 구현해보기

Java에서는 기본적으로 Stack, Queue를 제공한다. 하지만 자료구조로써 Stack의 특성만 이해한다면, ArrayList를 활용해서 Stack과 Queue로 사용할 수 있다. 

**자료구조는 자료(데이터)를 다루는 구조 그 자체를 뜻하며, 구현하는 방식에는 제약이 없다.**

```java
package test4;

import java.util.ArrayList;

public class Test4 {
    public static void main(String[] args) {
        // 미리 정의된 ArrayStack 객체를 사용
        ArrayListStack stack = new ArrayListStack();

        stack.push(1); // [1]
        stack.push(2); // [1, 2]
        stack.push(3); // [1, 2, 3]
        stack.push(4); // [1, 2, 3, 4]
        stack.push(5); // [1, 2, 3, 4, 5]

        System.out.println(stack.show()); // [1, 2, 3, 4, 5]

        stack.pop(); // [1, 2, 3, 4]
        stack.pop(); // [1, 2, 3]

        System.out.println(stack.show()); // [1, 2, 3]

        System.out.println(stack.size()); // 3
    }
}

class ArrayListStack {
    ArrayList al = new ArrayList();
		// a를 리스트 맨 뒤에 추가하고 리스트를 리턴
    public ArrayList push(int a) {
        al.add(a);
        return al;
    }
		// 리스트를 문자열로 변환하여 리턴
    public String show() {
        return al.toString();
    }
		// 맨 마지막 요소를 삭제하고 삭제한 데이터 리턴
    public ArrayList pop() {
				return  al.remove(al.size() - 1);     
    }
		// 리스트 데이터 크기 리턴
    public int size() {
        return al.size();
    }
}
```

 # Queue

큐(Queue)는 줄을 서서 기다리다, 대기 행렬이라는 뜻을 가지고 있다. 

- Queue는 Stack과 반대되는 개념으로, 먼저 들어간 데이터(data)가 먼저 나오는 FIFO(First In First Out)을 특징으로 가지고 있다.
- 입력과 출력의 방향이 고정되 있으며, 두 곳으로 접근이 가능하다.
- Queue에서 데이터를 넣는 것을 ‘enqueue’, 데이터를 꺼내는 것을 ‘dequeue’라고 한다.

## Queue의 특징

### 1. FIFO(First In First Out)

- 먼저 들어간 데이터가 제일 처음에 나오는 선입선출의 구조를 가지고 있다.

```java
public class Test5 {
    public static void main(String[] args) {
        Queue<Integer> queue = new LinkedList<>(); // int형 queue 선언

        queue.add(1);   // queue에 값 추가
        queue.add(2);
        queue.add(3);
        queue.add(4);

        System.out.println(queue);
        // 출력 방향 <--------------< 입력 방향 (선입선출)
        // 1 <- 2 <- 3 <- 4
        // <---------------------------<

        queue.poll();   // queue에 첫 번째 값을 반환하고 제거
        queue.poll();
        queue.poll();
        queue.poll();

        System.out.println(queue);

        // 빈 queue에 값에 poll을 사용하면 어떻게 될까?
        System.out.println(queue.poll());
    }
}

// 결과
[1, 2, 3, 4]
[]
null
```

### 2. 데이터는 하나씩 넣고 뺄 수 있다.

Queue 자료구조는 Stack과 마찬가지로 데이터가 아무리 많이 있어도 하나씩 데이터를 넣고, 뺀다. 한꺼번에 여러개를 넣거나 뺄 수 없다.

### 3. 두 개의 입출력 방향을 가지고 있다.

데이터의 입력, 출력 방향이 다르다. 만약 입출력 방향이 같다면 Queue 자료구조라고 볼 수 없다.

## Queue는 어디에 사용될까?

컴퓨터 장치들 사이에서 데이터(data)를 주고받을 때, 각 장치 사이에 존재하는 **속도의 차이나 시간 차이를 극복**하기 위해 **임시 기억 장치의 자료구조로 Queue**를 사용한다. 이것을 통틀어 **버퍼(buffer)**라고 한다. 

Queue는 컴퓨터와 프린터의 데이터 통신으로 알 수 있다..

1. 일반적으로 프린터는 속도가 느리다.
2. CPU는 프린터와 비교하여, 데이터를 처리하는 속도가 빠르다.
3. 따라서, **CPU는 빠른 속도로 인쇄에 필요한 데이터(data)를 만든 다음, 인쇄 작업 Queue에 저장하고 다른 작업을 수행**한다.
4. **프린터는 인쇄 작업 Queue에서 데이터(data)를 받아 일정한 속도로 인쇄**한다. 