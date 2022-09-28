# 목차
* [목차](#목차)
* [순열(permutation)과 조합(Combination)](#순열permutation과-조합combination)
    + [시작하기 전 알아둬야 할 용어](#시작하기-전-알아둬야-할-용어)
	+ [문제: 카드 뽑기](#문제--카드-뽑기)
        + [case 1](#case-1-순서를-생각하며-3장을-선택할-때의-모든-경우의-수)
		+ [case 2](#case-2-순서를-생각하지-않고-3장을-선택할-때의-모든-경우의-수)
        

# 순열(permutation)과 조합(Combination)

## 시작하기 전 알아둬야 할 용어

- 순열 :  요소 n개 중에 m개를 선택하여 순서에 상관 있게 뽑는 경우의 수
- 조합 : 순서에 상관없이 요소 n개 중에 m개를 뽑는 경우의 수
- `factorial, 팩토리얼` : n에서부터 1씩 감소하여 1까지의 모든 정수의 곱이다. (n보다 작거나 같은 모든 양의 정수의 곱이다.) 팩토리얼에서, 0과 1은 모두 1이다.

## 문제 : 카드 뽑기

> A, B, C, D, E로 이뤄진 5장의 카드가 있다. 이 5장의 카드 중 3장을 선택하여 나열하려고 한다. 이때, 다음의 조건을 각각 만족하는 경우를 찾아야 한다.
> 
> 1. **순서를 생각하며 3장을 선택합니다.**
> 2. **순서를 생각하지 않고 3장을 선택합니다.**

각 조건을 만족시키며 카드를 나열하는 방법은 각각 몇 가지일까?

## case 1. 순서를 생각하며 3장을 선택할 때의 모든 경우의 수

해당 조건을 만족하려면, 다음과 같은 방법으로 경우의 수를 구한다.

- 첫 번째 나열하는 카드를 선택하는 방법에는 다섯가지가 있다.
- 첫 번째 카드를 나열하고 난 다음, 두 번째 카드를 선택하는 방법에는 네 가지가 있다.
- 두 번째 카드를 나열하고 난 다음, 세 번째 카드를 선택하는 방법에는 세 가지가 있다.

따라서 `5 X 4 X 3 = 60` 가지의 방법이 있다.

이렇게 n개 중에서 일부만을 선택하여 나열하는 것을 순열이라고 한다. 

> 순열은 순서를 지키며 나열해야 한다.
> 

[A, B, D]와 [A, D, B] 모두 A, B, D를 사용했지만 나열하는 순서가 다르므로 서로 다른 경우로 파악해야 된다는 뜻이다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b6b2fca1-b46e-4044-9d2b-f9680088ba4f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220928T132855Z&X-Amz-Expires=86400&X-Amz-Signature=5a84038aa3893085a876b1d4564acc0869aab14ae8f431ec73b20a9caea3d211&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- 5장에서 3장을 선택하는 모든 순열의 수 = 5P3 = `(5 X 4 X 3 X 2 X 1) / (2 X 1) = 60`
- 일반식 : `nPr = n! / (n - r)!`
- `5!` = `5 X ( 5 - 1) X (5 - 2) X (5 - 3) X (5 - 4) = 5 X 4 X 3 X 2 X 1 = 120`

## 예제

```java
// 반복문 코드

public static ArrayList<String[]> permutationLoop() {
	// 순열 요소가 인자로 주어질 경우, 인자 그대로 사용하면 되지만, 인자가 주어지지 않고
	// 문제 안에 포함되어 있을 경우 이런 식으로 직접 적어서 사용한다.

	String[] lookup = new String[]{"A", "B", "C", "E"};
	ArrayList<String[]> result = new ArrayList<>();
	
	// 반복문 1개 당 1개의 요소를 뽑는다.
	for (int i = 0; i < lookup.length; i++) {
		for (int j = 0; j < lookup.length; j++) {
			for (int k = 0; k < lookup.length; k++) {
				// 중복된 요소 제거
				if (i == j || j == k || k == i) continue;
					String[] input = new String[]{lookup[i], lookup[j], lookup[k]};
					result.add(input);
				}
			}
		}
	}
	return result;
}

// 결과
[
  [ 'A', 'B', 'C' ], [ 'A', 'B', 'D' ], [ 'A', 'B', 'E' ],
  [ 'A', 'C', 'B' ], [ 'A', 'C', 'D' ], [ 'A', 'C', 'E' ],
  [ 'A', 'D', 'B' ], [ 'A', 'D', 'C' ], [ 'A', 'D', 'E' ],
  [ 'A', 'E', 'B' ], [ 'A', 'E', 'C' ], [ 'A', 'E', 'D' ],
  [ 'B', 'A', 'C' ], [ 'B', 'A', 'D' ], [ 'B', 'A', 'E' ],
  [ 'B', 'C', 'A' ], [ 'B', 'C', 'D' ], [ 'B', 'C', 'E' ],
  [ 'B', 'D', 'A' ], [ 'B', 'D', 'C' ], [ 'B', 'D', 'E' ],
  [ 'B', 'E', 'A' ], [ 'B', 'E', 'C' ], [ 'B', 'E', 'D' ],
  [ 'C', 'A', 'B' ], [ 'C', 'A', 'D' ], [ 'C', 'A', 'E' ],
  [ 'C', 'B', 'A' ], [ 'C', 'B', 'D' ], [ 'C', 'B', 'E' ],
  [ 'C', 'D', 'A' ], [ 'C', 'D', 'B' ], [ 'C', 'D', 'E' ],
  [ 'C', 'E', 'A' ], [ 'C', 'E', 'B' ], [ 'C', 'E', 'D' ],
  [ 'D', 'A', 'B' ], [ 'D', 'A', 'C' ], [ 'D', 'A', 'E' ],
  [ 'D', 'B', 'A' ], [ 'D', 'B', 'C' ], [ 'D', 'B', 'E' ],
  [ 'D', 'C', 'A' ], [ 'D', 'C', 'B' ], [ 'D', 'C', 'E' ],
  [ 'D', 'E', 'A' ], [ 'D', 'E', 'B' ], [ 'D', 'E', 'C' ],
  [ 'E', 'A', 'B' ], [ 'E', 'A', 'C' ], [ 'E', 'A', 'D' ],
  [ 'E', 'B', 'A' ], [ 'E', 'B', 'C' ], [ 'E', 'B', 'D' ],
  [ 'E', 'C', 'A' ], [ 'E', 'C', 'B' ], [ 'E', 'C', 'D' ],
  [ 'E', 'D', 'A' ], [ 'E', 'D', 'B' ], [ 'E', 'D', 'C' ]
]
```

## case 2. 순서를 생각하지 않고 3장을 선택할 때의 모든 경우의 수

다음과 같은 방법으로 경우의 수를 구해야 한다.

- 순열로 구할 수 있는 경우를 찾는다.
- 순열로 구할 수 있는 경우에서 중복된 경우의 수를 나눈다.

조합은 순열과 달리 순서를 고려하지 않는다. 

예를 들어 순열에서는 [A, B, C][A, C, B][B, A, C][B, C, A][C, A, B][C, B, A]를 모두 다른 경우로 취급하지만, 조합에서는 하나의 경우로 취급한다. 다시 말해 순열에서처럼 순서를 생각하여 선택하면, 중복된 경우가 6배 발생한다. 

3장의 카드를 순열 공식에 적용한 결과는 `3! / (3-3)!` = `(3 X 2X 1) / 1` = `6` 이다. 순서를 생각하느라 중복된 부분이 발생한 순열의 모든 가짓수를, 중복된 6가지로 나누어 주면 조합의 모든 경우의 수를 얻을 수 있다.

따라서 `(5 X 4 X 3 X 2 X 1) / ((3 X 2 X 1) X (2 X 1)) = 10` 이다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b58b9df8-4df3-4893-a6b5-99fa0e9692f9/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220928T133021Z&X-Amz-Expires=86400&X-Amz-Signature=a0e25d0632150a4354fef5dcbbcaa5c7693378b57fed098411614e05240c89b2&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- 5장에서 3장을 무작위로 선택하는 조합에서 모든 경우의 수 = 5C3 = `5! / (3! * 2!)` = `10`
- 일반식: nCr = `n! / (r! * (n - r)!)`

## 예제

```java
// 반복문 코드

public static ArrayList<String[]> combinationLoop() {
	String[] lookup = new String[]{"A", "B", "C", "D", "E"};
	ArrayListh<String[]> result = new ArrayList<>();
	// 한 번 조합한 요소는 다시 조합하지 않는다.
	for(int i = 0; i < lookup.length; i++) {
	    for(int j = i + 1; j < lookup.length; j++) {
	      for(int k = j + 1; k < lookup.length; k++) {
	        String[] input = new String[]{lookup[i], lookup[j], lookup[k]};
					result.add(input);
	      }
	    }
	  }
	
	  return result;
	}

// 결과
[
  [ 'A', 'B', 'C' ], [ 'A', 'B', 'D' ], [ 'A', 'B', 'E' ],
  [ 'A', 'C', 'D' ], [ 'A', 'C', 'E' ], [ 'A', 'D', 'E' ],
  [ 'B', 'C', 'D' ], [ 'B', 'C', 'E' ], [ 'B', 'D', 'E' ],
	[ 'C', 'D', 'E' ]
]
```

# 한계

반복문으로 순열과 조합을 만들어낼 수는 있다. 하지만, 분명한 한계점이 존재한다.

- 개수가 늘어나면 반복문의 수도 늘어난다.
    - 만약, 11개의 요소 중 10개를 뽑아야 한다면, 10중 반복문을 구현해야 한다. 이는 굉장히 비효율적일 뿐더러 보기 좋은 코드에 부합하지 않는다.
- 뽑아야 되는 개수가 n개처럼 변수로 들어왔을 때 대응이 어렵다.
    - 요소 개수를 변수로 받는 건 `요소.length` 를 사용하여 대응할 수 있지만, 뽑아야 되는 개수도 변수로 받게 된다면 몇 개의 반복문을 설정해야 하는지, 설정은 어떻게 하는지 굉장히 까다로워진다.

<aside>
💡 그렇기 때문에 순열과 조합은 재귀를 사용하여 풀어야 하는 경우가 많다.

</aside>

