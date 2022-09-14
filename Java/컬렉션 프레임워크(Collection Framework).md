# 목차
* [목차](#목차)
* [컬렉션 프레임워크(Collection Framework)](#컬렉션-프레임워크collection-framework)
    + [Collection 인터페이스](#collection-인터페이스)
        + [Collection 인터페이스 메서드](#collection-인터페이스-메서드)
    + [List<E>](#liste)
        + [List 인터페이스 메서드](#L)
        + [ArrayList](#arraylist)
        + [LinkedList](#linkedlist)
        + [ArrayList와 LinkedList 차이](#arraylist와-linkedlist-차이)
    + [Iterator](#iterator)
        + [Iterator 인터페이스 메서드](#iterator-인터페이스-메서드)
    + [Set](#sete)
        + [Set 인터페이스 메서드](#set-인터페이스-메서드)
        + [HashSet](#hashset)
            + [HashSet은 중복된 값인지 어떻게 판단할까?](#hashset은-중복된-값인지-어떻게-판단할까)
        + [TreeSet](#treeset)
    + [Map<K,V>](#mapkv)
        + [Map 인터페이스 메서드](#map-인터페이스-메서드)
        + [HashMap](#hashmap)
        + [Map.Entry 인터페이스 메서드](#mapentry-인터페이스-메서드)

# 컬렉션 프레임워크(Collection Framework)

> 특정 자료 구조에 데이터를 추가하고, 삭제하고, 수정하고, 검색하는 등의 동작을 수행하는 편리한 메서드들을 정의해놓은 것
> 
- 컬렉션 프레임워크는 주요 인터페이스로 List, Set, Map을 제공한다.
    - List
        - 데이터의 순서가 유지되며, 중복 저장이 가능한 컬렉션을 구현하는 데에 사용된다.
        - ArryaList, Vector Stack, LinkedList 등이 List 인터페이스를 구현한다.
    - Set
        - 데이터의 순서가 유지되지 않으며, 중복 저장이 불가능한 컬렉션을 구현하는 데에 사용된다.
        - HashSet, TreeSet 등이 Set인터페이스를 구현한다.
    - Map
        - 키(key)와 값(value)의 쌍으로 데이터를 저장하는 컬렉션을 구현하는 데에 사용된다.
        - 데이터의 순서가 유지되지 않으며, 키는 값을 식별하기 위해 사용되므로 중복 저장이 불가능하고, 값은 중복 저장이 가능하다.
        - HashMap, HashTable, TreeMap, Properties 등이 있다.
- List와 Set은 서로 공통점이 많아서 Collection이라는 인터페이스로 묶인다.

![Untitled](https://blog.kakaocdn.net/dn/5VduD/btroxjoZlhf/tWoOBb98qm7wJKWjDZ7Zr1/img.jpg)

## Collection 인터페이스

> List와 Set의 공통점이 추출되어 추상회 된 인터페이스
> 

### Collection 인터페이스 메서드

| 기능 | 리턴 타입 | 메소드 | 설명 |
| --- | --- | --- | --- |
| 객체 추가 | boolean | add(Object o) /addAll(Collection c) | 주어진 객체 및 컬렉션의 객체들을 컬렉션에 추가합니다. |
| 객체 검색 | boolean | contains(Object o) / containsAll(Collection c) | 주어진 객체 및 컬렉션이 저장되어 있는지 여부를 리턴합니다. |
|  | Iterator | iterator() | 컬렉션의 iterator를 리턴합니다. |
|  | boolean | equals(Object o) | 컬렉션이 동일한지 여부를 확인합니다. |
|  | boolean | isEmpty() | 컬렉션이 비어있는지 여부를 확인합니다. |
|  | int | size() | 저장되어 있는 전체 객체 수를 리턴합니다. |
| 객체 삭제 | void | clear() | 컬렉션에 저장된 모든 객체를 삭제합니다. |
|  | boolean | remove(Object o) / removeAll(Collection c) | 주어진 객체 및 컬렉션을 삭제하고 성공 여부를 리턴합니다. |
|  | boolean | retainAll(Collection c) | 주어진 컬렉션을 제외한 모든 객체를 컬렉션에서 삭제하고, 컬렉션에 변화가 있는지의 여부를 리턴합니다. |
| 객체 변환 | Object[] | toArray() | 컬렉션에 저장된 객체를 객체배열(Object [])로 반환합니다. |
|  | Object[] | toArray(Object[] a) | 주어진 배열에 컬렉션의 객체를 저장해서 반환합니다. |

## List<E>

- List 인터페이스는 배열과 같이 객체를 일렬로 늘어놓은 구조를 가지고 있다.
- 객체를 인덱스로 관리하기 때문에 객체를 저장하면 자동으로 인덱스가 부여되고, 인덱스로 객체를 검색, 추가, 삭제할 수 있는 여러 기능을 제공한다.
- List 인터페이스를 구현한 클래스로는 ArrayList, Vector, LinkdedList, Stack 등이 있다.

### List 인터페이스 메서드

| 기능 | 리턴 타입 | 메서드 | 설명 |
| --- | --- | --- | --- |
| 객체 추가 | void | add(int index, Object element) | 주어진 인덱스에 객체를 추가 |
|  | boolean | addAll(int index, Collection c) | 주어진 인덱스에 컬렉션을 추가 |
|  | Object | set(int index, Object element) | 주어진 위치에 객체를 저장 |
| 객체 검색 | Object | get(int index) | 주어진 인덱스에 저장된 객체를 반환 |
|  | int | indexOf(Object o) / lastIndexOf(Object o) | 순방향 / 역방향으로 탐색하여 주어진 객체의 위치를 반환 |
|  | ListIterator | listIterator() / listIterator(int index) | List의 객체를 탐색할 수 있는ListIterator 반환 / 주어진 index부터 탐색할 수 있는 ListIterator 반환 |
|  | List | subList(int fromIndex, int toIndex) | fromIndex부터 toIndex에 있는 객체를 반환 |
| 객체 삭제 | Object | remove(int index) | 주어진 인덱스에 저장된 객체를 삭제하고 삭제된 객체를 반환 |
|  | boolean | remove(Object o) | 주어진 객체를 삭제 |
| 객체 정렬 | void | sort(Comparator c) | 주어진 비교자(comparator)로 List를 정렬 |

## ArrayList

- List 인터페이스를 구현한 클래스로, 컬렉션 프레임워크에서 가장 많이 사용된다.
- 기능적으로 Vector와 동일하지만 기존의 Vector를 개선한 것이므로, Vector보다는 주로 ArrayList를 사용한다.
- 객체 인덱스로 관리된다는 점이 배열과 유사하지만 ArrayList는 저장 용량을 초과하여 객체들이 추가되면, 자동으로 저장용량이 늘어난다.
- 리스트 계열 자료구조의 특성을 이어받아 데이터가 연속적으로 존재한다. 즉, 데이터의 순서를 유지한다.

```java
ArrayList<타입 매개변수> 객체명 = new ArrayList<타입 매개변수>(초기 저장 용량);

ArrayList<String> container = new ArrayList<String>(30);
// String 타입의 객체를 저장하는 ArrayList 생성
// 초기 용량을 30으로 지정. 초기 용량을 전달하지 않으면 기본적으로 10으로 지정된다.
```

- 특정 인덱스의 객체를 제거하면, 바로 뒤 인덱스부터 마지막 인덱스까지 모두 앞으로 1씩 당겨진다.
- 번번한 객체 삭제와 삽입이 일어나는 곳에서는 ArraylList보다는 LinkedList를 사용하는 것이 좋다.

## LinkedList

- 데이터를 효율적으로 추가, 삭제, 변경하기 위해 사용한다.
- LinkedList는 모든 데이터가 불연속적으로 존재하며, 이 데이터는 서로 연결(link)되어 있다.
- 데이터를 삭제할 때, 삭제하고자 하는 요소와 이전 요소가 삭제하고자 하는 요소의 다음요소를 참조하도록 변경하면 된다. 배열처럼 데이터를 이동하기 위해 복사할 필요가 업식 때문에 처리 속도가 훨씬 빠르다.

```java
ArrayList<String> list = new LinkedList<>();
```

## ArrayList와 LinkedList 차이

### ArrayList

- 강점
    - 데이터를 순차적으로 추가하거나 삭제하는 경우
    - 데이터를 읽어들이는 경우 인덱스를 통해 데이터에 접근할 수 있으므로 검색이 빠르다.
- 약점
    - 중간에 데이터를 추가하거나, 중간에 위치하는 데이터를 삭제하는 경우, 해당 데이터 뒤에 위치한 값들을 뒤로 밀어주거나 앞으로 당겨주어야 한다.

### LinkedList

- 강점
    - 중간에 위치하는 데이터를 추가하거나 삭제하는 경우, Prev와 Next의 주소값만 변경하면 되므로, ArrayList보다 비교적 빠르게 처리한다.
- 약점
    - 처음부터 데이터를 읽어드리므로 ArrayList에 비해 비교적 검색 속도가 느리다.

## Iterator

> 컬렉션에 저장된 요소들을 순차적으로 읽어오는 역할
> 
- Collection 인터페이스에 정의된 `iterator()`를 호출하면, Iterator 타입의 인스턴스가 반환된다.
- Collection 인터페이스를 상속받은 List와 Set 인터페이스를 구현한 클래스들은 `iterator()` 메서드를 사용할 수 있다.

### Iterator 인터페이스 메서드

| 서드 | 설명 |
| --- | --- |
| hasNext() | 읽어올 객체가 남아 있으면 true를 리턴하고, 없으면 false를 리턴합니다. |
| next() | 컬렉션에서 하나의 객체를 읽어옵니다. 이 때, next()를 호출하기 전에 hasNext()를 통해 읽어올 다음 요소가 있는지 먼저 확인해야 합니다. |
| remove() | next()를 통해 읽어온 객체를 삭제합니다. next()를 호출한 다음에 remove()를 호출해야 합니다. |
- `next()` 를 사용하기 전에 먼저 가져올 객체가 있는지 `hasNext()` 를 통해 확인하는 것이 좋다.

```java
ArrayList<String> list = ...;
Iterator<String> iterator = list.iterator();

while(iterator.hasNext()) {  // 읽어올 다음 객체가 있다면
	String str = iterator.next();  // next()를 통해 다음 객체를 읽어온다.
	if(str.equals("str과 같은 단어")) { // 조건에 부합하면
		iterator.remove();             // 해당 객체를 컬렉션에서 제거한다.
}
```

- Iterator를 사용하지 않아도, for-each문을 이용해 전체 객체를 반복할 수 있다.

```java
ArrayList<String> list = ...;
for(String str : list) {
	...
}
```

## Set<E>

- 요소의 중복을 허용하지 않고, 저장 순서를 유지하지 않는 컬렉션
- 대표적인 Set을 구현한 클래스에는 HashSet, TreeSet이 있다.

### Set 인터페이스 메서드

| 기능 | 리턴 타입 | 메서드 | 설명 |
| --- | --- | --- | --- |
| 객체 추가 | boolean | add(Object o) | 주어진 객체를 추가하고, 성공하면 true를, 중복 객체면 false를 반환합니다. |
| 객체 검색 | boolean | contains(Object o) | 주어진 객체가 Set에 존재하는지 확인합니다. |
|  | boolean | isEmpty() | Set이 비어있는지 확인합니다. |
|  | Iterator | Iterator() | 저장된 객체를 하나씩 읽어오는 반복자를 리턴합니다. |
|  | int | size() | 저장되어 있는 전체 객체의 수를 리턴합니다. |
| 객체 삭제 | void | clear() | Set에 저장되어져 있는 모든 객체를 삭제합니다. |
|  | boolean | remove(Object o) | 주어진 객체를 삭제합니다. |

## HashSet

- Set 인터페이스를 구현한 대표적인  컬렉션 클래스
- 중복된 값을 허용하지 않으며, 저장 순서를 유지하지 않는다.

### HashSet은 중복된 값인지 어떻게 판단할까?

1. `add(Object o)` 를 통해 객체를 저장하고자 한다.
2. 저장하려는 객체의 해시코드를 `hashCode()` 메서드로 얻어낸다.
3. Set이 저장하고 있는 모든 객체들의 해시코드를 `hashCode()` 메서드로 얻어낸다.
4. 저장하려는 객체의 해시코드와 Set에 저장되어져 있던 객체들의 해시코드를 비교한다.
    1. 이 때, 동일한 해시코드를 가진 객체가 존재하면 5번으로 넘어간다.
    2. 동일한 해시코드를 가진 객체가 존재하지 않으면, Set에 객체가 추가되며 `add(Object o)` 메서드가 `true`를 리턴한다.
5. `equals()` 메서드를 통해 객체를 비교한다.
    1. `true` 가 리턴된다면 중복 객체로 간주되어 Set에 추가되지 않고, `add(Object o)`가 `false` 를 리턴한다.
    2. `false` 가 리턴되면 Set에 객체가 추가되며, `add(Object o)` 메서드가 `true` 를 리턴한다.

## TreeSet

- 이진 탐색 트리 형태로 데이터를 저장한다.
- 데이터의 중복 저장을 허용하지 않고 저장 순서를 유지하지 않는다.
- 이진 탐색 트리(Binary Search Tree)란 하나의 부모 노드가 최대 두 개의 자식 노드와 연결되는 이진 트리(Binary tree)의 일종으로, 정렬과 검색에 특화한 자료 구조이다.
- 모든 왼쪽 자식의 값이 루트나 부모보다 작고, 모든 오른쪽 자식의 값이 루트나 부모보다 큰 값을 가진다.
- TreeSet의 기본 정렬 방식은 오름차순이다.

## Map<K,V>

- Map 인터페이스는 키(key)와 값(value)으로 구성된 객체를 저장하는 구조를 가지고 있다.
- 여기서  객체를 Entry객체라고 하는데, 이 Entry객체는 키와 값을 각각 Key 객체와 Value 객체로 저장한다.
- 키는 중복 저장될 수 없지만, 값은 중복 저장이 가능하다. 만약 기존에 저장된 키와 동일한 키로 값을 저장하면, 기존의 값이 새로운 값으로 대치된다.
- Map 인터페이스를 구현한 클래스에는 HashMap, HashTable, TreeMap, SortedMap 등이 있다.

![Untitled](https://img1.daumcdn.net/thumb/R800x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcfpMTT%2FbtqEvxLt6qb%2FMXYNWUvXCKfRvNWjDMZoq0%2Fimg.png)

### Map 인터페이스 메서드

| 기능 | 리턴 타입 | 메서드 | 설명 |
| --- | --- | --- | --- |
| 객체 추가 | Object | put(Object key, Object value) | 주어진 키로 값을 저장합니다. 해당 키가 새로운 키일 경우 null을 리턴하지만, 동일한 키가 있을 경우에는 기존의 값을 대체하고 대체되기 이전의 값을 리턴합니다. |
| 객체 검색 | boolean | containsKey(Object key) | 주어진 키가 있으면 true, 없으면 false를 리턴합니다. |
|  | boolean | containsValue(Object value) | 주어진 값이 있으면 true, 없으면 false를 리턴합니다. |
|  | Set | entrySet() | 키와 값의 쌍으로 구성된 모든 Map.Entry 객체를 Set에 담아서 리턴합니다. |
|  | Object | get(Object key) | 주어진 키에 해당하는 값을 리턴합니다. |
|  | boolean | isEmpty() | 컬렉션이 비어 있는지 확인합니다. |
|  | Set | keySet() | 모든 키를 Set 객체에 담아서 리턴합니다. |
|  | int | size() | 저장된 Entry 객체의 총 갯수를 리턴합니다. |
|  | Collection | values() | 저장된 모든 값을 Collection에 담아서 리턴합니다. |
| 객체 삭제 | void | clear() | 모든 Map.Entry(키와 값)을 삭제합니다. |
|  | Object | remove(Object key) | 주어진 키와 일치하는 Map.Entry를 삭제하고 값을 리턴합니다. |

## HashMap

- HashMap은 해시 함수를 통해 키와 값이 저장되는 위치를 결정하므로, 사용자는 그 위치를 알 수 없고, 삽입되는 순서와 위치 또한 관계가 없다.
- 해싱(Hashing)을 사용하기 때문에 많은 양의 데이터를 검색하는 데 있어서 뛰어난 성능을 보인다.
- HashMap의 개별 요소가 되는 Entry 객체는 Entry 인터페이스를 구현한다.

```java
HashMap<String, Integer> hashMap = new HashMap<>();
```

- Map은 키와 값을 쌍으로 저장하기 때문에 `iterator()` 를 직접 호출 할 수 없다. 대신 `keySet()` 이나 `etnrySet()` 메서드를 이용해 Set형태로 반환한 컬렉션에 `iterator()` 를 호출하여 반복자를 만든 후, 반복자를 통해 순회할 수 있다.

```java
public class HashMapExample {
	public static void main(String[] args) {
		
		// HashMap 생성
		HashMap<String, Integer> chicken = new HashMap<>();

		// Entry 객체 저장
		chicken.put("뿌링클", 20000);
		chicken.put("고추 바사삭", 18000);
		chicken.put("허니 콤보", 19000);

		// keySet으로 itertor() 호출하는 방법
		// key를 요소로 가지는 Set을 생성
		Set<String> keySet = chicken.keySet();

		// keySet을 순회하면서 value를 읽어온다.
		Iterator<String> keyIterator = keySet.iterator();
		while(keyIterator.hasNext()) {
			String key = keyIterator.next();
			Integer value = chicken.get(key);

		// entrySet으로 iterator() 호출하는 방법
		// Entry 객체를 요소로 가지는 Set을 생성 
		Set<Map.Entry<String, Integer>> etnrySet = chicken.entrySet();

		// entrySet을 순회하면서 value를 읽어온다.
		Iterator<Map.Entry<String, Integer>> entryIterator = entrySet.iterator();
		while(entryIterator.hasNext()) {
			Map.Entry<String, Integer> entry = entryIterator.next();
			String key = entry.getKey();  // Chicken.Entry 인터페이스의 메서드
			Integer value = entry.getValue();  // Chicken.Entry 인터페이스의 메서드
		}
	}
}
```

### Map.Entry 인터페이스 메서드

| 리턴 타입 | 메서드 | 설명 |
| --- | --- | --- |
| boolean | equals(Object o) | 동일한 Entry 객체인지 비교합니다. |
| Object | getKey() | Entry 객체의 Key 객체를 반환합니다. |
| Object | getValue() | Entry 객체의 Value 객체를 반환합니다. |
| int | hashCode() | Entry 객체의 해시코드를 반환합니다. |
| Object | setValue(Object value) | Entry 객체의 Value 객체를 인자로 전달한 value 객체로 바꿉니다. |
