# ArrayIndexOutOfBoundsException 원인과 해결법
## 원인

> 정해진 배열의 크기보다 크거나 음수 index에 대한 요청이 있으면 ArrayIndexOutOfBoundsException이 발생한다.
> 

- 배열의 index는 1부터 시작하는 것이 아닌 0부터 시작한다. 배열의 크기를 n이라고 한다면 index는 1부터 n까지가 아닌 0부터 n-1까지인 것이다. 보통 이 부분에서 해당 예외가 많이 발생하는 것 같다.
- ArrayIndexOutOfBoundsException은 자바 컴파일러에서 검사하지 않고 실행을 시켜야 알 수 있다.

```java
int[] arr = new int[5] // arr의 범위는 arr[0] ~ arr[4]로 총 5개의 인덱스를 생성

arr[5] = 5; // arr[5]는 존재하지 않기 때문에 예외 발생!

// 결과
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: Index 5 out of bounds for length 5
	at ArrTest.main(ArrTest.java:7) // Index 5는 배열의 length의 범위를 벗어났다는 뜻
```

## 해결 방법

- 배열 범위에 맞게 값을 넣는 법을 알아보자

### 1. 반복문

```java
for(int i = 0; i < arr.length; i++) { ... } // 배열의 범위만큼만 반복
```

```java
int i = 0;
while(i < arr.length) { // 배열의 범위만큼만 반복
	...
	i++;
}
```

### 2. foreach문

```java
for(int i : arr) { ... } // 배열의 범위만큼만 반복
```

### 3. try-catch문

```java
try { // 예외가 발생할 것 같은 코드 작성
	int[] arr = new int[3];
	for (int i = 0; i < 3; i++) {
		arr[i] = i;
	}
// catch([발생할 것 같은 예외] [예외 변수 이름]) { 예외 발생 시 실행할 코드 }
} catch (ArrayIndexOutOfBoundsException e) { // ArrayIndexOutOfBoundsException가 발생하면 ArrayIndexOutOfBoundsException 출력해줘
            System.out.println(e);
} 
```