# 목차
* [목차](#목차)
* [트리(Tree)](#트리-tree)
    + [이진 트리(Binary Tree)](#이진-트리-binary-tree)
    + [이진 탐색 트리(Binary Search Tree)](#이진-탐색-트리bst-binary-search-tree)
* [그래프(Graph)](#binary-search-tree)
    + [무방향 그래프(Undirection Graph)](#무방향-그래프-undirected-graph)
    + [가중치 그래프(Weight Graph)](#가중치-그래프-weighted-graph)
* [BFS와 DFS](#bfs와-dfs)
    + [BFS](#bfs)
    + [DFS](#dfs-depth-first-search-깊이-우선-탐색)

# 트리 (Tree)

- 계층적인 구조를 표현할 때 사용하는 비선형 자료구조
    - 조직도
    - 디렉토리와 서브디렉터리
    - 가계도
- 트리는 노드(node)들과 노드들을 연결하는 링크(link)들로 구성되어 있다.
- 노드의 개수가 n개라면 항상 n - 1개의 링크를 가진다.
- 노드간의 경로는 하나밖에 존재하지 않는다.(유일하다)

### 용어

- 루트 노드(root node) : 가장 위에 있는 노드. 부모가 없다
- 단말 노드(leaf node) : 자식이 없는 노드를 말한다.
- 부모 노드(Parente node) : 상하관계에서 위에 있는 노드를 말한다. 모든 노드들은 유일한 부모 노드들을 가진다.
- 자식 노드(child node) : 상하관계에서 아래에 있는 노드를 말한다.
- 형제 노드(sibling node) : 동일한 부모를 가진 자식 노드들을 말한다.
- 조상 노드 (ancestor node) : 부모의 부모 노드들을 말한다. (부모-자식 관계의 확장)
- 자손 노드 (descendent node) : 자식의 자식 노드들을 말한다. (부모-자식 관계의 확장)
- 서브 트리 (sub tree) : 전체 트리 안에서 부모-자식 관계로 이루어진 트리를 말한다. (트리는 여러 서브 트리로 이루어졌다고 볼 수 있다.)
- 레벨 (level) : 각 계층을 나타낸다. (루트는 레벨1로 그 아래는 레벨2, 3, 4, 5 …)
- 높이 (height) : 가장 높은 레벨을 말한다. (레벨5까지 있는 트리의 높이는 5이다.)

# 이진 트리 (Binary Tree)

- 자식 노드를 최대 2개만 가지는 트리를 말한다.
- 각각의 자식 노드는 부모의 왼쪽 자식인지 오른쪽 자식인지가 지정된다.
- 같은 값을 가진 트리여도 자식의 위치에 따라 다른  이진트리가 된다.
    - 부모(엄마) - 왼쪽 자식(아들1) - 오른쪽 자식(아들2) ≠ 부모(엄마)-왼쪽 자식(아들2) - 오른쪽 자식(아들2)
    - 부모(엄마) - 왼쪽 자식(아들) ≠ 부모(엄마) - 오른쪽 자식(아들)
- 사용 예
    - 수식
    - 허프먼 코드

### 용어

- Full Binary Tree : 모든 레벨의 노드가 최대치로 꽉 차 있는 트리
    - 높이가 h라면 (2의 h승 - 1)개의 노드를 가진다.
- Complete Binary Tree : 마지막 레벨을 제외한 모든 레벨이 꽉 차 있는 트리. 단, 마지막 레벨의 가장 오른쪽 자식부터 비어있어야 한다. (heap의 특성과 같다.)
    - 노드가 N개인 full 혹은 complete 이진 트리의 높이는 O(logN)이다.

### 표현

- 연결구조(Linkded Structure)로 표현한다. (Linkded List를 쓰면 되는듯?)
- 각 노드에 하나의 데이터 필드와 왼쪽 자식(left), 오른쪽 자식(right), 그리고 부모 노드(p)의 주소를 저장한다.(단, 부모노드의 주소는 반드시 필요한 경우가 아니라면 보통 생략한다.)
- 루트의 주소값은 별도로 저장한다. (루트의 주소값을 잃어버리면 해당 트리 전체가 사라지는거다 ㄷㄷ)

### 순회

- 순회 : 이진 트리의 모든 노드를 방문하는 일
- 중순위(inorder) 순회 : 왼쪽 서브트리를 inorder → 루트노드 → 오른쪽 서브트리 inorder
    
    ```java
    InorderTreeWalk(node n) {
    	if(n != null) {
    		InorderTreeWalk(n.left());
    		return n;
    		InorderTreeWalk(n.right());
    	}
    }
    ```
    
- 선순위(preorder) 순회 : 루트노드 → 왼쪽 서브트리 preorder → 오른쪽 서브트리 preorder
    
    ```java
    PreorderTreeWalk(node n) {
    	if(n != null) {
    		return n;
    		PreorderTreeWalk(n.left());
    		PreorderTreeWalk(n.right());
    	}
    }
    ```
    
- 후순위(posterorder) 순회 : 왼쪽 서브트리 postorder → 오른쪽 서브트리 postorder → 루트노드
    
    ```java
    PostorderTreeWalk(node n) {
    	if(n != null) {
    		PostorderTreeWalk(n.left());
    		PostorderTreeWalk(n.right());
    		return n;
    	}
    }
    ```
    
- 레벨오더(level-order) 순회 : 레벨 순으로 방문, 동일 레벨에서는 왼쪽에서 오른쪽 순서로 순회한다. 큐(queue)를 이용하여 구현한다.
    
    ```java
    LevelorderTreeWalk(node n) {
    	// 큐에 루트를 넣고
    	// 큐가 빌 때까지 반복
    			// 큐.push(큐.poll());
    
    사실 아직 이해 못함
    ```
    

# 이진 탐색 트리(BST; Binary Search Tree)

- 이진 트리이면서 각 노드에 하나의 키를 저장하고 빠르게 연산할 수 있는 자료구조
- 왼쪽 자식 노드(서브 트리)의 값은 부모 노드보다 작고 오른쪽 자식 노드(서브 트리)의 값은 부모 노드보다 크거나 같아야 한다.
    - 부모(5) - 왼쪽 자식(3) - 오른쪽 자식(7) (O)
    - 부모(5) - 왼쪽 자식(6) - 오른쪽 자식(7) (X)
    - 부모(5) - 왼쪽 자식(3) - 오른쪽 자식(2) (X)
- 다음과 같은 연산들을 지원한다.
    - INSERT : 새로운 키의 삽입
    - SEARCh : 키 탐색
    - DELETE : 키의 삭제
- 일반적으로 SEARCH, INSERT, DELETE 연산이 트리의 높이(height)에 비례하는 시간 복잡도를 가진다. O(h)

## Search

```java
// 재귀 함수 사용
BinarySearchTree(RootNode x, Key k) {
	if( x == null || k == x.key) {
		return x;
	}
	
	if ( k < x.key ) {
		return BinarySearchTree(x.left, k)
	}
	else return BinarySearchtree)x.right, k)
}
```

```java
// 반복문 사용
BinarySearchTree(RootNode x, Key k) {
	while(x != null && k != x.key) {
		if(k < x.key) x = x.left;
		else x = x.right;
	}
	return x;
}
```

- 이진 탐색 트리에서 최소값 찾기
    
    ```java
    // 재귀 함수 사용
    BinarySearchTreeMin(RootNode x, Key k) {
    	if(x.left == null) return x;
    
    	x = x.left;
    	BinarySearchTreeMin(x, k);
    }
    ```
    
    ```java
    // 반복문 사용
    BinarySearchTreeMin(RootNode x, Key k) {
    	while (x.left != null) {
    	x = x.left;
    	}
    	return x;
    }
    ```
    
- Succesor 찾기 : succesoor란 노드 x의 successor란 x.key보다 크면서 가장 작은 키를 가진 노드를 말한다.
    - 노드 x의 오른쪽 서브트리가 존재할 경우, 오른쪽 서브트리의 최소값
    - 오른쪽 부트리가 없는 경우, 자신이 왼쪽 자식이 될 때까지 부모자식을 따랑 올라감.
    - 루트에 갈 때까지 자신이 왼쪽 자식이 될 부모를 만나지 못하면 Successor는 존재하지 않음 (즉, x가 최대값)
        
        ```java
        BinarySearchTreeMin(Node x) {
        	if(x.right != null) {
        		return BinarySearchTreeMin(x.right);
        	}
        	else {
        		y = x.p;
        		while(y != null && x == y.right)) {
        			x = y;
        			y = y.p;
        		}
        	return y;
        }
        ```
        
    - Predecessor 찾기  : Succesoor의 반대
    
    ## Insert
    
    ```java
    Insert(BinarySearchTree T, int z) {
    	y = null;
    	x = T.root;
    	// x가 null이되면 x의 부모 노드는 y
    	while (x != null) {
    		y = x;
    		if(z < x.key)x = x.left;
    		else x = x.right;
    	}
    	// z의 부모가 y를 가리키게함
    	z.p = y;
    	if( y == null) T.root = z;
    	else if(z < y.key) y.left = z;
    	else y.right = z;
    ```
    
    ## Delete
    
    - 자식노드가 없는 경우
    - 삭제하려는 노드의 자식노드가 1개인 경우 : 삭제하려는 노드의 부모 노드의 자식 노드를 삭제하려는 노드의 자식 노드로 교체
    - 삭제하려는 노드의 자식노드가 2개인 경우 : 삭제하려는 노드의 데이터를 지우고 새로운 데이터로 교체.(삭제하려는 노드의 Successor나 presuccessor로 교체)
    
    ```java
    Delete(BinarySearchTree T, node z) {
    	if (z.left() == null || z.righ() == null) y = z;
    	else y = Successor(z);
    	
    	if (y.left != null) x = y.left();
    	else x = y.right();
    
    	if (x != null) x.p() = y.p();
    
    	if (y.p() == null) T.root() = x;
    	else if(y == y.p().left()) {
    		y.p().left() = x;
    	} else y.p().right() = x;
    
    	if (y != z) z.key() = y.key();
    	
    	return y; 
    ```
    

**질문**

- 이진 트리와 BST 이외에 어떠한 트리가 있을까요?
- 균형 이진 탐색 트리란 무엇일까요?

# 그래프 (Graph)

- 객체(object)들 간의 이진관계를 표현
- V : 노드(node) 혹은 정점(vertex)
- E : 노드쌍을 연결하는 엣지(edge) 혹은 링크(link)
- n = 정점의 개수
- m = 에지의 개수
- 연결된 (connected) 그래프 : 모든 노드들이 연결된 그래프
- 연결요소(connected component) : 그래프 안에 연결되어 있는 요소들의 개수 (서브트리랑 비슷)

## 무방향 그래프 (Undirected Graph)

- 객체들 간의 이진관계를 표현하는 무방향 자료구조(ex. (1, 2) == (2, 1))
- 노드와 노드를 연결하는 경로(path)가 존재할 때 서로 연결되어 있다고 한다.
- 인접행렬 : 인접행렬은 대칭
- 인접리스트 : 정점 집합을 표현하는 하나의 배열과 각 정점마다 인접한 정점들의 연결 리스트

## 방향 그래프 (Directed Graph)

- 에지가 방향을 가짐 (u, v) == u로부터 v로의 방향을 갖는다는 의미 (ex. (1, 2) ≠ (2, 1))
- 인접행렬 : 비대칭
- 인접리스트 :  m개의 노드를 가짐

## 가중치 그래프 (Weighted Graph)

- 에지마다 가중치(weight)를 지정
- 인접행렬 : 에지의 존재를 나타내는 값으로 1대신 에지의 가중치를 저장
    - 에지가 없을 때 혹은 대각선 :
        - 특별히 정해진 규칙은 없으며, 그래프와 가중치가 의마하는 바에 따라서
        - 예 : 가중치가 거리 혹은 비용을 의미하는 경우라면 에지가 없으면 **∞,** 대각선은 0.
        - 예 : 만약 가중치가 용량을 의미한다면 에지가 없을 때 0, 대각선은 **∞**
        

# BFS와 DFS

순회(traversal) : 그래프의 모든 노드들을 방문하는 일

### 대표적인 순회 방법 2가지

- BFS (Breadth-First Search, 너비 우선 순회)
- DFS (Depth-First Search, 깊이 우선 순회)

## BFS

- 최단 경로를 알고싶을 때 사용한다.


- $L_0$ = s, 여기서 s는 출발 노드
- $L_1$ = L0의 모든 이웃 노드들
- $L_2$ = L1의 이웃들 중 L0에 속하지 않는 노드들
- …
- $L_i$= $L_{i-1}$ 의 이웃들 중 $L_{i-2}$에 속하지 않는 노드들

### 큐를 이용하여 표현

1. start 노드를 체크(체크는 이미 방문된 노드라는 표시)
2. start 노드를 큐에 넣음
3. 큐가 빌 때까지 
4. 큐의 맨 앞 요소 뽑음. 
5. 맨 앞 요소의 체크 되어있지 않은 이웃 요소들을 체크하고 큐에 넣음

```java
BFS(Queue G, node s) {
	Queue queue = new Queue();
	queue.add(s);
	
	while(!queue.isEmpty()) {
		u = queue.poll();
		for(u의 인접한 v를 다 훑어본다) {
				if(v가 체크 되어있지 않으면) {
					 v를 체크한다.
						queue.add(v);
				}
			}
		}
}
```

### BFS와 최단경로

- Start에서 $L_i$에 속한 노드까지의 최단 경로의 길이는 i이다. (여기서 경로의 길이는 경로에 속한 엣지의 개수를 의미한다.)
- BFS를 하면서 각 노드에 대해서 최단 경로의 길이를 구할 수 있다.
- 입력 : 방향 혹은 무방향 그래프 G=(V, E), 그리고 출발노드 sV
- 출력 : 모든 노드 v에 대해서
    - d[v] = s로부터 v까지의 최단 경로의 길이(엣지의 개수)
    - [𝝿](https://kr.piliapp.com/symbol/pi/)[v] = s로부터 v까지의 최단경로상에서 v의 직전 노드(predecessor)

## DFS (Depth-First Search, 깊이 우선 탐색)

- 깊이를 우선적으로 탐색
- 한 정점에서 시작해서 다음 경로로 넘어가기 전에 해당 경로를 완벽하게 탐색할 때 사용한다.
- BFS보다 탐색 시간은 오래 걸리더라도 모든 노드를 완전히 탐색할 수 있다.

질문

- DFS와 BFS의 장단점은 또 무엇이 있을까요?
    - DFS는 최단 경로를 확인 할 수 있다. 모든 경로를 확인할 때는 오랜 시간이 걸린다.
    - BFS는 DFS보다 느리지만 모든 경로를 확인할 수 있다.
- 그래프가 굉장히 크다면 어떤 탐색 기법을 고려해야 할까요?
    - BFS
- 반대로, 그래프의 규모가 작고, depth가 얕다면 어떤 탐색 기법을 고려해야 할까요?
    - DFS