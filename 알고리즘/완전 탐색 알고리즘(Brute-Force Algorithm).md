# 목차
* [목차](#목차)
* [완전 탐색 알고리즘(Brute-Force Algorithm)](#완전-탐색-알고리즘brute-force-algorithm)
    + [Brute Force Algorithm이 사용되는 경우](#brute-force-algorithm이-사용되는-경우)
    + [Brute Force Algorithm의 한계](#brute-force-algorithm의-한계)
    + [어디서 사용할까?](#어디서-사용할까)

# 완전 탐색 알고리즘(Brute-Force Algorithm)

> 무차별 대입 방법을 나타내는 알고리즘이며, 순수한 컴퓨팅 성능에 의존하여 모든 가능성을 시도하여 문제를 해결하는 방법이다.
> 
- 암호학에서는 **Brute Force Attack**이라고 불리며 **특정한 암호를 풀기 위해서 모든 값을 대입하는 방법**을 말한다.
- 공간복잡도와 시간복잡도의 요소를 고려하지 않고 최악의 시나리오를 취하더라도 솔루션을 찾으려고 하는 방법을 의미한다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/1f71f115-1e0e-40df-96ce-c0662de326c9/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220927T121253Z&X-Amz-Expires=86400&X-Amz-Signature=f80ed45e5bc9b771b5bcbf7582174f12cc486d48f313a721f1cdf1463f68ff54&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

## Brute Force Algorithm이 사용되는 경우

- 프로세스 속도를 높이는데 **사용할 수 있는 다른 알고리즘이 없을 때**
- 문제를 해결하는 여러 솔루션이 있고 **각 솔루션을 확인해야 할 때**

간단히 말하자면 문제가 있을 때 더 적절한 해결 방법을 찾지 못했을 때 시도하는 방법이 Brute Force Algorithm이다. 

그러나 데이터의 범위가 커질수록 상당히 비효율적이다. 프로젝트의 규모가 커진다면 **더 효율적인 알고리즘을 사용**해야 한다.

## Brute Force Algorithm의 한계

문제의 복잡도에 매우 민감하다. 문제가 복잡해질수록 기하급수적으로 많은 자원을 필요로 하는 비효율적인 알고리즘이 될 수 있다. 여기서 자원은 시간이 될 수도 있고 컴퓨팅 자원이 될 수도 있다. 

## 어디서 사용할까?

왠지 단순무식한 알고리즘이라 잘 사용되지 않을 거라고 생각했지만 많은 곳에서 사용하고 있다. 반복문을 통해서 범위를 줄이지 않고 하나하나 비교하는 것도 Brute Force Algorithm이다. 

### Brute Force 활용 알고리즘

- 버블 정렬 알고리즘 - Bubble Sort
- Tree 자료 구조의 완전탐색 알고리즘 - Exhausive Search (BFS, DFS)
- 동적 프로그래밍 - DP(Dynamic Programming)

### 추가로 공부하면 좋을 키워드

- Brute Force vs Dynamic Programming
- Closet-Pair Problems by Brute Force
- Convex-Hull Problems by Brute Force