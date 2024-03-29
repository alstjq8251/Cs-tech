## 자료구조(Data Structure)란?
- 자료구조는 컴퓨터상 자료를 효율적으로 저장하기 위해 만들어진 논리적인 구조이다.
- 자료 구조의 현명한 선택을 통해 효율적인 알고리즘을 사용할 수 있게 하여 성능을 향상킨다.

### 자료구조의 분류
- 자료 구조의 분류는 선형 구조와 비선형 구조로 크게 나뉜다.

| 구조 | 설명 | 종류|
:--: | :--: | :--:| 
선형 구조 | 데이터를 연속적으로 연결한 자료 구조 | 리스트 , 스택 , 큐 , 데크|
비선형 구조 | 데이터를 비연속적으로 연결한 자료 구조 | 트리 , 그래프|

<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/219060778-55e78ab7-f40a-4f8d-a32b-09773487de2e.png">

### 선형 구조
1. `리스트(List)`

| 개념 | 설명|
:--: | :--: |
선형 리스트(Linear List) | 배열과 같이 연속되는 기억 장소에 저장되는 리스트<br> 선형 리스트의 대표적인 구조로는 배열(Array)등이 있다. <br> 가장 간편한 자료 구조이며, 접근 구조가 빠름 <br> 자료의 삽입 , 삭제 시 기존 자료의 이동이 필요|
연결 리스트(Linked List) | `노드`의 포인터 부분으로 서로 연결시킨 리스트 <br> 연결하는 방식에 따라 단순 연결 리스트 , 원형 연결 리스트 , 이중 연결 리스트 , 이중원형 연결 리스트로 구분한다. <br> 노드의 삽입 , 삭제가 선형 리스트와 달리 편리하다 <br> 연결을 위한 포인터가 추가되어 저장 공간이 추가로 필요하다 <br> `포인터`를 통해 찾는 시간이 추가되어 선형 리스트에 비해 느리다|

##### 연결리스트를 구성하는 노드? 와 포인터란?
- 연결리스트를 구성하는 노드는 데이터(Data) + 포인터(Pointer)로 되어있으며 데이터 저장 부분과 포인터 부분으로 구성된 저장 공간을 뜻한다.
  - `포인터`란 주로 C언어에서 쓰이며 현 위치에서 다음 위치를 알려주는 요소이자 해당 주소값을 가리키는 것이다.

<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/219064701-96c45f78-57b9-4710-b801-7b4873a785a7.png">

```
포인터는 메모리 공간의 주소를 가리키는 변수인데 java나 파이썬 등에선 포인터를 사용하여 주소를 할당하지 않고 JVM이 자동으로 할당해주게 된다.
트리 , 연결리스트에서 쓰이는 노드는 데이터 + 링크 라고 표현해 이해하는 것이 옳다.
```

좌 - ArrayList 우 - LinkedList 두 선형 리스트 간 물리 메모리 공간이 어떻게 점유되는지 차이를 보여주는 사진이다.
<div>
  <img width="500" height = "250" alt="image" src="https://user-images.githubusercontent.com/98382954/219066139-32492e10-e51a-450f-8e77-ad4c0ddf00d7.png">
  <img width="500" height = "250" alt="image" src="https://user-images.githubusercontent.com/98382954/219065642-38017540-9b42-47b2-b7d7-f27f08710032.png">
</div>

이러한 차이들로 각각의 상황에 자료구조를 선택해야 하는데 Java의 자료구조를 설명할 때 다시 기술할 것이다.

2. `스택(Stack)`
- 스택이란 한 방향으로만 자료를 넣고 꺼낼 수 있는 LIFO(Last-In- First-Out)형식의 자료 구조이다.

<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/219068595-44ba6a78-9a80-40fc-ab39-313a5512e785.png">

- 한 방향으로만 PUSH와 POP을 이용해 자료를 넣고 꺼낸다.
- TOP은 스택에서 가장 위에 있는 데이터로 , 스택 포인터(Stack Pointer)라고도 불린다.

가장 기본적인 스택 연산은 하단의 표와 같다.

| 연산 | 설명 |
:--: | :--: |
Push | 데이터를 차례대로 스텍에 넣는 연산|
POP | 스택에서 가장 위에 있는 데이터를 하나씩 꺼내는 연산

3. `큐(Queue)`
- 큐란 한쪽 끝에서는 삽입 작업이 이뤄지고 , 반대쪽 끝에서는 삭제 작업이 이뤄지는 FIFO(First-In First-Out)형식의 자료 구조이다.

<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/219069699-a5fad4d5-fd9a-4ff1-b649-ed25be8822c9.png">

- 한쪽 끝에서 ENQUEUE 연산을 통해 데이터를 넣고 한쪽에서는 DEQUEUE 연산을 이용해 데이터를 꺼낸다.
- 데이터가 꺼내는 쪽에서 가장 가까운 데이터를 Front라 하며 데이터를 넣는 쪽에서 가장 가까운 데이터를 Rear라 한다.

가장 기본적인 큐 연산은 하단의 표와 같다.

| 연산 | 설명 |
:--: | :--: |
ENQUEUE | 데이터를 차례대로 넣는 연산|
DEQUEUE | 처음 저장된 데이터부터 하나씩 꺼내는 연산

4. `데크(Deque)`
- 데크란 큐의 양쪽 끝에서 삽입과 삭제를 할 수 있는 자료 구조이다.

<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/219070886-f8b6dfd5-f4fc-4069-b7eb-3c87a7c413ff.png">

- 두 개의 포인터를 사용하여 , 양쪽의 삭제/삽입이 가능하다.
- 데크를 이용해 스택과 큐의 구현이 가능하다.
- 인덱싱이 가능한 고수준의 배열이다.

기본적인 데크의 연산은 하단의 표를 참고한다.

| 연산 | 설명 |
:--: | :--: |
PUSH | 데크에 차례데로 데이터를 넣는 연산|
POP | 데크에서 Front와 Rear에 있는 데이터를 하나씩 꺼내는 연산

### 비선형구조
1. `트리(Tree)`
- 트리는 데이터들을 계층화시킨 자료 구조이다.
- 인덱스를 조작하는 방법으로 가장 많이 사용하는 구조이다.
- 트리는 노드(Node)와 노드를 연결하는 링크(Link)로 구성된다.
- 배열과 달리 노드들이 포인터로 연결되어 노드의 상한선이 없다.

<img width="100%" alt="image" src="https://user-images.githubusercontent.com/98382954/219072918-e2a6ff89-351d-4e94-b17f-4a51a2056927.png">


트리에서 사용하는 용어는 하단의 표와 같다.
| 용어 | 설명|
:--: | :--: |
루트 노드(Root Node) | 트리에서 부모가 없는 최상위 노드, 트리의 시작점|
단말 노드(Leaf Node) | 자식이 없는 노드 , 트리의 가장 말단에 위치|
레벨(Level) | 루트 노드를 기준으로 특정 노드까지의 경로 길이|
조상 노드(Ancestor Node) | 특정 노드에서 루트에 이루는 경로상 모든 노드|
자식 노드(Child Node) | 특정 노드에 연결된 다음 레벨의 노드|
부모 노드(Parent Node) | 특정 노드에 연결된 이전 레벨의 노드|
형제 노드(Sibling) | 같은 부모를 가진 노드|
깊이(Depth) | 루트 노드에서 특정 노드에 도달하기 위한 간선의 수|
차수(Degree) | 특정 노드에 연결된 자식 노드의 수|

> 트리의 차수를 묻는 경우 특정 노드를 언급하지 않는다면 가장 큰 차수가 그 트리의 차수가 된다.

##### 이진 트리
`정의`
  - **차수(Degree)가 2 이하인 노드들로 구성된 트리**, 즉 자식이 둘 이하인 노드들로만 구성된 트리를 말한다.
  - 이진 트리의 레벨 i에서 최대 노드의 수는 2 (i-1)승 이다.
  - 이진 트리에서 Terminal Node 수가 n0, 차수가 2인 노드 수가 n2라 할때 n0= n2+1이 된다.

![image](https://github.com/alstjq8251/Cs-tech/assets/98382954/be845ba2-e18e-404b-8c17-d895f70cd02e)


#### 트리 순회 방법
  - 트리를 구성하는 각 노드들을 찾아가는 방법을 운행법(Traversal)이라 한다.
1. 전위(Preorder) 순행
  - Root -> Left -> Right 순으로 진행
2. 중위(Inorder) 순행
  - Left -> Root -> Right 순으로 진행  
3. 후위(Postorder) 순행
  - Left -> Right -> Root 순으로 진행

##### 트리 순회 방법 수식 표기법

1. 전위(Preorder) 순행 표기법(PreFix)
   - 연산자 -> Left -> Right, +AB
2. 중위(Inorder) 순행 표기법(Infix)
   - Left -> 연산자 -> Right , A+B
3. 후위(PostOrder) 순행 표기법(PostFix)
   - Left -> Right -> 연산자 , AB+



#### Reference
<https://boycoding.tistory.com/32><br>
<https://velog.io/@seulgea/DataStructure-LinkedList><br>
<https://colinch4.github.io/2020-11-28/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-ArrayList%EC%99%80-LinkedList/><br>
<https://aerocode.net/188><br>
<https://ko.wikipedia.org/wiki/%EC%9D%B4%EC%A7%84_%ED%8A%B8%EB%A6%AC>
