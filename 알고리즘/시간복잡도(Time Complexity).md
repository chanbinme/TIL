# 목차
* [목차](#목차)
* [시간복잡도(Time Complexity)](#시간복잡도time-complexity)
    + [Big-O 표기법](#big-o-표기법)
        + [O(1)](#o1-constant-complexity)
        + [O(n)](#onlinear-complexity)
        + [O(log n)](#olog-nlogarithmic-complexity)
        + [O(n^2)](#on2quadratic-complexity)
        + [O(2^n)](#o2nexponential-complexity)
    + [Big-O 활용해보기](#big-o-활용해보기)   
        + [데이터 크기에 따른 시간 복잡도](#대략적인-데이터-크기에-따른-시간-복잡도)
        + [빠른 순으로 시간복잡도 정렬](#빠른-순으로-시간-복잡도-정렬)
        + [자료구조와 알고리즘 종류별 시간복잡도](#자료구조와-알고리즘-종류별-시간복잡도)
        + [실행시간 예측하기](#실행시간-예측하기) 

# 시간복잡도(Time Complexity)

![https://blog.chulgil.me/content/images/2019/02/Screen-Shot-2019-02-07-at-2.31.54-PM-1.png](https://blog.chulgil.me/content/images/2019/02/Screen-Shot-2019-02-07-at-2.31.54-PM-1.png)

> 입력값의 변화에 따라 연산을 실행할 때, 연산 횟수에 비해 시간이 얼마만큼 걸리는가?
> 

알고리즘을 구현한다는 것은 바꾸어 말해 **입력값이 커짐에 따라 증가하는 시간의 비율을 최소화한 알고리즘**을 구성했다는 이야기이다. 그리고 이 시간 복잡도는 빅-오 표기법을 사용해 나타낸다.

## Big-O 표기법

> 입력값의 변화에 따라 연산을 실행할 때, 연산 횟수에 비해 시간이 얼마만큼 걸리는가?를 표기하는 방법
> 

시간 복잡도를 표기하는 방법은 다음과 같다.

- Big-O(빅-오) : 최악의 경우를 나타내는 경우
- Big-Ω(빅-오메가) : 최선을 나타내는 경우
- Big-θ(빅-세타) : 중간(평균)을 나타내는 경우

이 중에서 빅오 표기법이 가장 많이 사용된다. 빅오 표기법은 최악의 경우를 고려하므로, 프로그램이 실행되는 과정에서 소요되는 최악의 시간까지 고려할 수 있기 때문이다. **“최소한 특정 시간 이상이 걸린다”** 혹은 **“이 정도 시간이 걸린다”**를 고려하는 것보다 **“이 정도 시간까지 걸릴 수 있다”**를 고려해야 그에 맞는 대응이 가능하다.

Big-O 표기법에대해 아라보쟈

## O(1) (constant complexity)

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/791cbc1e-d446-4523-a77e-d115f7b2400c/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220927T120240Z&X-Amz-Expires=86400&X-Amz-Signature=b88f524ff2d3011e0e60f4c6ec0172a99806935cd5f8e4e5699b7580d5caf53f&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- 입력값이 증가하더라도 시간이 늘어나지 않는다.
- 다시 말해 입력값의 크기와 관계없이, 즉시 출력값을 얻어낼 수 있다는 의미이다.

```java
// O(1)의 시간 복잡도를 가진 알고리즘

public int 0_1_algorithm(int[] arr, int index) {
	return arr[index];
}

int[] arr = new int[]{1, 2, 3, 4, 5};
int index = 1;
int results = 0_1_algorithm(arr, index);
System.out.println(results); // 2
```

위 알고리즘에선 입력값의 크기가 아무리 커져도 즉시 출력값을 얻어낼 수 있다. 예를 들어 arr의 길이가 100만이라도, 즉시 해당 index에 접근해 값을 반환할 수 있다.

## O(n)(linear complexity)

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0bbe1285-34f5-46de-9223-1cd1a32c3d96/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220927T120253Z&X-Amz-Expires=86400&X-Amz-Signature=20e0290d583bf695d1dd12bfc8115835f01d893694a95a7c68893f1bc78c9197&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- 입력값이 증가함에 따라 시간 또한 같은 비율로 증가하는 것을 의미한다.
- 입력값이 1이라면 1초의 시간이 걸리고, 입력값을 100배로 증가시켰을 때 1초의 100배인 100초가 걸리는 알고리즘이 있다면, 그 알고리즘은 O(n)의 시간 복잡도를 가진다고 할 수 있다.

```java
// O(n)의 시간 복잡도를 가지는 알고리즘

public void O_n_algorithm(int n) {
	for(int i = 0; i < n; i++) {
	// do something for 1 second
	}
}

public void another_O_n_algorithm(int n) {
	for(int i = 0; i < n * 2; i++) {
		// do something for 1 second
	}
}
```

`anothier_O_n_algorithm` 의 알고리즘을 보고 O(2n)이라고 표현하겠다고 생각할 수 있다. 하지만 이 알고리즘 또한 Big-O 표기법으로는 O(n)으로 표기한다. 입력값이 커지면 커질수록 계수(n 앞에 있는 수)의 의미(영향력)가 점점 퇴색되기 때문에, 같은 비율로 증가하고 있다면 2배가 아닌 5배, 10배로 증가하더라도 O(n)으로 표기한다.

## O(log n)(logarithmic complexity)

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/704bb359-868c-49c8-a9ef-8b8e5c5b0b97/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220927T120304Z&X-Amz-Expires=86400&X-Amz-Signature=434656cfa509763f23e78846e6281291f2754ff711fabf8e0b19a269765d4441&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- Big-O표기법 중 O(1) 다음으로 빠른 시간 복잡도를 가진다.
- BST(Binary Search Tree)도 O(log n)의 시간 복잡도를 가진 대표적인 알고리즘(탐색기법)이다.
- 매번 숫자를 제시할 때마다 경우의 수가 절반이 줄어들기 때문에 최악의 경우에도 7번이면 원하는 숫자를 찾아낼 수 있다.

## O($n^2$)(quadratic complexity)

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/d72548d1-3c34-4465-aacc-a7c5f74ca834/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220927T120317Z&X-Amz-Expires=86400&X-Amz-Signature=9c6b95fe38c205cca4bde7377a213571b8f5623c67becee5d76f805278711e4b&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- 입력값이 증감함에 따라 시간이 n의 제곱수의 비율로 증가하는 것을 의미한다.
- 예를 들어 입력값이 1일 경우 1초가 걸리던 알고리즘에 5라는 값을 주었더니 25초가 걸리게 된다면, 이 알고리즘의 시간 복잡도는 O(n^2)라고 표현할 수 있다.

```java
// O(n^2)의 시간 복잡도를 가진 알고리즘
public void O_quadratic_algorithm(int n) {
	for(int i = 0; i < n; i++) {
		for(int j = 0; j < n; j++) {
			// do something for 1 second
		}
	}
}

public void another_O_quadratic_algorithm(int n) {
	for(int i = 0 ; i < n; i++) {
		for(int j = 0; j < n; j++) {
			for(int k = 0; k < k; k++) {
				// do something for 1 second
			}
		}
	}
}
```

2n, 5n을 모두 O(n)이라고 표현하는 것처럼, n^3과 n^5도 모두 O(n^2)로 표기한다. n이 커지면 커질수록 지수가 주는 영향력이 점점 퇴색되기 때문이다.

## O($2^n$)(exponential complexity)

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f3b801aa-01e3-4ff8-a570-1296a3a28afa/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220927T120345Z&X-Amz-Expires=86400&X-Amz-Signature=1feca8ade19a714ef22dc6c457e125d5a301b8b7f296628cae9344f79274daac&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- Big-O 표기법 중 가장 느린 시간 복잡도를 가진다.
- 구현한 알고리즘의 시간 복잡도가 O($2^n$)이라면 다른 접근 방식을 고민해 보는 것이 좋다.

```java
// O(2^n)의 시간 복잡도를 가지는 알고리즘

public int fibonacci(int n) {
	if(n <= 1) {
		return 1;
	}
	return fibonacci(n - 1) + fibonacci(n - 2);
}
```

재귀로 구현하는 피보나치 수열은 O($2^n$)의 시간 복잡도를 가진 대표적인 알고리즘이다.

브라우저 개발자 창에서 n을 40으로 두어도 수초가 걸리는 것을 확인할 수 있으며, n이 100 이상이면 평생 결과를 반환받지 못할 수도 있다.

## Big-O 활용해보기

- 입력 데이터가 클 때는 O(n) 혹은 O(log n)의 시간 복잡도를 만족할 수 있도록 예측해서 문제를 풀어야 한다.
- 데이터가 작을 때는 시간 복잡도가 크더라도 문제를 풀어내는 것에 집중하자

### 대략적인 데이터 크기에 따른 시간 복잡도

| 데이터 크기 제한 | 예상되는 시간 복잡도 |
| --- | --- |
| n ≤ 1,000,000 | O(n) or O(log n) |
| n ≤ 10,000 | O(n2) |
| n ≤ 500 | O(n3) |

### 빠른 순으로 시간 복잡도 정렬

| O(1) | O(log n) | O(n) | O(⁍) | O(⁍) |
| --- | --- | --- | --- | --- |

### 자료구조와 알고리즘 종류별 시간복잡도

| Data Structures | Average Case |  |  | Worst Case |  |  |
| --- | --- | --- | --- | --- | --- | --- |
|  | Search | Insert | Delete | Search | Insert | Delete |
| Array | O(n) | N/A | N/A | O(n) | N/A | N/A |
| Sorted Array | O(log n) | O(n) | O(n) | O(log n) | O(n) | O(n) |
| Linked List | O(n) | O(1) | O(1) | O(n) | O(1) | O(1) |
| Doubly Linked List | O(n) | O(1) | O(1) | O(n) | O(1) | O(1) |
| Stack | O(n) | O(1) | O(1) | O(n) | O(1) | O(1) |
| Hash table | O(1) | O(1) | O(1) | O(n) | O(n) | O(n) |
| Binary Search Tree | O(log n) | O(log n) | O(log n) | O(n) | O(n) | O(n) |
| B-Tree | O(log n) | O(log n) | O(log n) | O(log n) | O(log n) | O(log n) |
| Red-Black tree | O(log n) | O(log n) | O(log n) | O(log n) | O(log n) | O(log n) |
| AVL Tree | O(log n) | O(log n) | O(log n) | O(log n) | O(log n) | O(log n) |

| Sorting Algorithms | 공간 복잡도 | 시간 복잡도 |  |  |
| --- | --- | --- | --- | --- |
|  | 최악 | 최선 | 평균  | 최악 |
| Bubble Sort | O(1) | O(n) | O(n2) | O(n2) |
| Heapsort | O(1) | O(n log n) | O(n log n) | O(n log n) |
| Insertion Sort | O(1) | O(n) | O(n2) | O(n2) |
| Mergesort | O(n) | O(n log n) | O(n log n) | O(n log n) |
| Quicksort | O(log n) | O(n log n) | O(n log n) | O(n2) |
| Selection Sort | O(1) | O(n2) | O(n2) | O(n2) |
| Shell Sort | O(1) | O(n) | O(n log n2) | O(n log n2) |
| Smooth Sort | O(1) | O(n) | O(n log n) | O(n log n) |

### 실행시간 예측하기

![1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc2d1fp%2FbtqHMuFnpau%2F4KznlVXaOpunKaKruaC8p1%2Fimg.png)

### **질문**

- 가장 빠른/느린 시간 복잡도는 무엇일까요?
    - 가장 빠른 시간 복잡도는 O(1)이다. 일명 답정너
    - 가장 느린 시간 복잡도는 O($2^n)$해당 시간 복잡도가 걸린다면 알고리즘을 다시 짜는 것이 좋다.
- 효율적인 알고리즘을 구성했다 함은 무엇을 의미할까요?
    - 입력값이 증감함에 따라 증가하는 시간을 최소화한 알고리즘
- 시간 복잡도는 A와 B의 비례함이 어느 정도인지를 나타냅니다. A와 B는 무엇일까요?
    - A는 연산횟수 B는 연산하는 소요 시간