# 목차
* [목차](#목차)
* [이진 탐색 알고리즘(Binary Search Algorithm)](#이진-탐색-알고리즘binary-search-algorithm)
    + [동작 과정](#동작-과정)
    + [언제 사용할까?](#언제-사용할까)
    + [Binary Search Algorithm의 한계](#binary-search-algorithm의-한계)
    + [어디서 사용할까?](#어디서-사용하고-있을까)
    + [Binary Search Tree(BST)와 다른 점](#binary-search-treebst와-다른-점)

# 이진 탐색 알고리즘(Binary Search Algorithm)

탐색 알고리즘은 크게 3가지가 있다.

- 선형 탐색 알고리즘 (Linear Search Algorithm)
- 이진 탐색 알고리즘 (Binary Search Algorithm)
- 해시 탐색 알고리즘 (Hash Search Algorithm)

이 중 Binary Search Algorithm에 대해 아라보쟈

> Binary Search Algorithm은 데이터가 정렬된 상태에서 절반씩 범위를 나눠 분할 정복기법으로 특정한 값을 찾아내는 알고리즘이다.
> 

## 동작 과정

1. 정렬된 배열의 가장 중간 인덱스를 지정한다.
2. 찾으려고 하는 값이 지정한 중간 인덱스의 값이라면 탐색을 종료한다. 아니라면 3단계로 간다.
3. 찾으려고 하는 값이 중간 인덱스의 값보다 큰 값인지, 작은 값인지 확인한다.
4. 값이 있는 부분과 값이 없는 부분으로 분리한다.
5. 값이 있는 부분에서 다시 1단계부터 반복한다. 

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/e99a55a5-b78c-4a19-a601-0f96cbfba051/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220927T121536Z&X-Amz-Expires=86400&X-Amz-Signature=de10a294b3fd0d82f88a00ccbbdeab23b8569dd78cc408733a9c6ea98428821a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

## 언제 사용할까?

- 정렬된 배열에서 요솟값을 더 효율적으로 검색할 때 사용한다.
- 데이터의 양이 많으면 많을수록 효율이 높아서 정렬된 데이터의 양이 많을 때 사용한다.

시간복잡도는 O(log n)으로 빠른 편이지만 항상 효율이 좋은 것은 아니다. 데이터양이 적고, 앞쪽에 위치한 데이터를 탐색할 때는 Linear Search Algorithm이 빠른 구간이 존재한다.

## Binary Search Algorithm의 한계

- 배열에만 구현할 수 있다.
- 정렬되어 있어야만 구현할 수 있다.
    - 규모가 작은 배열이라도 정렬이 되어 있지 않다면 정렬을 한 후 Binary Search Algorithm을 사용해도 효율이 높지 않다.

## 어디서 사용하고 있을까?

주로 대규모의 데이터 검색에 사용된다.

- 사전 검색
    - 사전에서 단어를 찾을 때 사용한다.
- 도서관 도서 검색
    - 도서관에서 사용하는 도서 코드로 도서를 검색할 때 사용한다.
- 대규모 시스템에 대한 리소스 사항 파악
    - 시스템 부하 테스트에서 예측된 부하를 처리하는데 필요한 CPU 양에 대해서 파악할 때 사용한다.
- 반도체 테스트 프로그램에서 디지털, 아날로그 레벨을 측정하는데 사용한다.

## Binary Search Tree(BST)와 다른 점

다르다.

마지막 단어로 알 수 있듯이 Tree는 자료구조를 나타내고 Algorithm은 해결 방법을 나타낸다.

Binary Search Tree는 하나의 노드가 두 개의 하위 트리를 가지는 이진 트리로 이루어진 구조이다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4506db62-9e34-4e77-af95-e279f1e01475/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220927T122646Z&X-Amz-Expires=86400&X-Amz-Signature=b118418761356da9cbe673645c0c95e8d2097e3263cdc347056a07f4ea47bec0&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

### 더 공부하면 좋은 키워드들

- Linear Search Algorithm
- Hash Search Algorithm
- Divide-and-conquer Algorithm
- Binary Tree vs Binary Search Tree
- Binary Search Algorithm vs Binary Search Tree