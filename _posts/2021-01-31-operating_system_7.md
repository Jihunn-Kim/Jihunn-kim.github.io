---
title: "운영체제 요약 - 7. 교착상태"
last_modified_at: 2021-04-07
show_date: true
classes: wide
excerpt: ""
categories:
  - operating_system
---

## 7.1. 시스템 모델(System Model)
시스템은 프로세스들에게 분배할 수 있는 유한한 수의 자원들로 구성된다. 
프로세스는 자원을 사용하기 전에 반드시 요청해야 하고(자원 사용 가능할 때까지 대기), 사용 후에는 반드시 방출해야 한다. 
자원 요청과 방출은 시스템 호출이며, 운영체제는 자원의 상태와 어느 프로세스에게 할당되었는지 기록한다. 

한 프로세스 집합 내의 모든 프로세스가 그 집합 내의 다른 프로세스에 의해서만 발생될 수 있는 사건을 기다린다면, 그 프로세스 집합은 교착상태에 있다고 한다. 
예를 들어, 세 개의 프로세스가 각각 한 개의 CD 드라이브를 점유한 채로 다른 CD 드라이브를 요청한다면 교착상태에 놓인다. 

## 7.2. 교착상태의 특징(Deadlock Characterization)

### 7.2.1. 필요 조건들(Necessary Conditions)
교착상태는 아래 네 조건이 동시에 성립될 때 발생할 수 있다. 
* 상호 배제(Mutual exclusion)
	- 한 번에 한 프로세스만이 자원을 사용할 수 있는 비공유 모드
* 점유하며 대기(Hold-and-wait)
	- 한 프로세스가 자원을 점유한 채 다른 프로세스의 자원을 얻으려고 대기
* 비선점(No preemption)
	- 자원을 점유한 프로세스외에 자원을 강제로 방출할 수 없음
* 순환 대기(Circular wait)
	- 대기하고 있는 프로세스 집합에서 P<sub>n-1<sub>이 P<sub>n<sub>의 자원을, P<sub>n<sub>가 P<sub>0<sub>을 기다림

### 7.2.2. 자원 할당 그래프(Resource-Allocation Graph)
프로세스 집합과 자원 집합을 노드와 방향간선으로 그래프를 생성할 수 있다. 
프로세스 P로 자원 R로 가는 방향 간선은 자원 요청 간선이며, 그 반대는 자원 할당 간선이다. 
자원 할당 그래프에 사이클이 없다면 교착상태가 아니고, 있다면 교착상태일 수도 아닐 수도 있다. 

<figure style="width: 350px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-31-operating_system_7/1.png' }}" alt=""> 
</figure> 

## 7.3. 교착상태 처리 방법(Methos for Handling Deadlocks)
교착상태를 처리하는 방법에는 세 가지 방법이 있다. 
1. 교착상태를 예방하거나 회피하는 프로토콜
2. 교착상태를 허용한 다음에 회복시키는 방법
3. 교착상태 문제를 무시하는 방법

대부분의 운영체제는 교착상태가 드물게 발생하기 때문에 3번을 사용하며, 따라서 교착상태는 응용 개발자가 프로그램을 통해 처리해야 한다. 

## 7.4. 교착상태 예방(Deadlock prevention)
교착상태 네 가지 필요조건 중 하나라도 발생하지 않도록 보장함으로써, 교착상태를 예방한다.

### 7.4.1. 상호 배제(Mutual exclusion)
일반적으로 상호 배제 조건을 없애는 것은 불가능하다. 
Mutex lock과 같은 어떤 자원들은 근본적으로 공유가 불가능하기 때문이다. 

### 7.4.2. 점유하며 대기(Hold-and-wait)
일반적으로 두 가지 방안이 있다. 
하나는 프로세스가 실행되기 전에 필요한 모든 자원을 요청하고 할당받는 것이고, 
다른 하나는 프로세스가 자원을 갖고 있지 않을 때만 자원을 요청할 수 있도록 허용하는 것이다. 

하지만 많은 자원들이 할당된 후 오랫동안 사용되지 않아 자원 이용도가 낮을 수 있다. 
또한 자주 쓰이는 자원들을 여러 개 필요로 하는 프로세스는 필요 자원 중 하나가 항상 다른 프로세스에게 할당되어 있기 때문에 무한정 대기해야 할수도 있다. 

### 7.4.3. 비선점(No preemption)
한 방안은 어떤 자원을 점유하고 있는 프로세스가 할당할 수 없는 다른 자원을 요청하면, 현재 점유하고 있는 자원들을 방출하는 것이다. 
대안으로, 추가 자원을 위해 대기하고 있는 프로세스한테서 현재 프로세스가 요청하는 자원을 가져올 수 있다.  

### 7.4.4. 순환 대기(Circular wait)
한 가지 방법은 모든 자원들에게 순서를 부여하여, 프로세스가 자원을 오름차순으로 요청하도록 하는 것이다. 
예를 들어, 테이프 드라이브=1, 디스크 드라이브=5 라 하면, 프로세스는 테이프 드라이브를 먼저 요청하고 디스크 드라이브를 요청해야 한다. 
하지만 순서나 계층 구조를 정하는 것만으로는 교착상태를 예방할 수 없고, 순서를 지키는 프로그램을 프로그래머가 작성해야 한다. 

## 7.5. 교착상태 회피(Deadlock Avoidance)
교착상태 예방은 장치의 이용률이 저하되고 시스템 처리율이 감소된다. 
대안인 교착상태 회피는 자원이 어떻게 요청될 지에 대한 추가 정보를 제공하도록 요구하는 것이다. 

가장 단순하고 유용한 모델은 프로세스가 자신이 필요로 하는 타입의 자원 최대 수를 선언하도록 하는 것이다. 
각 프로세스가 요청할 타입의 자원 최대 수를 미리 파악할 수 있다면, 교착상태에 들어가지 않도록 알고리즘을 만들 수 있다. 

이 알고리즘은 순환 대기 상황이 발생하지 않도록 자원 할당 상태를 검사한다. 
자원 할당 상태에는 가용 자원의 수, 할당된 자원의 수, 프로세스들의 최대 요구 수에 의해 정의된다. 

### 7.5.1. 안전상태(Safe State)
시스템 상태가 안전하다는 말은 어떤 순서로든 프로세스들이 요청하는 자원을 교착상태를 야기하지 않고 차례로 모두 할당해 줄 수 있다는 것을 뜻한다. 
즉, <P<sub>1</sub>, P<sub>2</sub>, ..., P<sub>n</sub>>의 process sequence에서 P<sub>i</sub>가 모든 P<sub>j</sub> (j < i)들이 반납한 자원을 가지고 수행 가능한 안전 순서(safe sequence)가 있다는 것이다. 
이와 같은 순서가 없다면 불안정 상태에 있다고 한다. 

안전상태의 시스템은 교착상태가 아니다. 불안정 상태라면 교착상태일 수도 아닐 수도 있다. 
프로세스가 자원을 요청할 때, 안전상태에서 안전상태로 변한다면 즉시 할당한다. 
그렇지 않다면 요청을 들어주든지 아니면 기다리게 할지 결정한다. 

### 7.5.2. 자원 할당 그래프 알고리즘(Resource-Allocation Graph Algorithm)
자원 타입마다 하나의 인스턴스만 있을 때만 이 방식을 쓸 수 있다. 

자원 할당 그래프에 프로세스가 미래에 자원을 요청할 예약 간선(프로세스 P -> 자원 R)을 추가하여 교착상태를 회피할 수 있다. 
프로세스가 자원을 요청하면 예약 간선은 요청 간선이 되며, 자원이 방출되면 할당 간선은 예약 간선이 된다. 

<figure style="width: 400px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-31-operating_system_7/2.png' }}" alt=""> 
</figure> 

프로세스에 자원을 할당하기 전에 예약 간선을 포함하여 사이클이 생기는 지 확인한다. 
사이클이 없다면 안전 상태이므로 할당하고, 아니라면 불안정 상태가 되므로 대기한다. 

### 7.5.3. 은행원 알고리즘(Banker's Algorithm)
자원 타입마다 여러 인스턴스가 있어도 사용할 수 있다. 

프로세스가 시작할 때 프로세스가 사용할 자원의 최대 개수를 미리 신고해야 한다. 
그리고 시스템이 자원을 할당하고 있는 상태를 나타내는 여러 자료구조를 사용한다.
* Available[i] = k
	- 자원 R<sub>i</sub>를 k개 추가적으로 더 사용할 수 있음
* Max[i, j] = k
	- 프로세스 P<sub>i</sub>가 R<sub>j</sub>를 최대 k개까지 요청할 수 있음
* Allocation[i, j] = k
	- 프로세스 P<sub>i</sub>가 R<sub>j</sub>를 k개 사용하고 있음
* Need[i, j] = k
	- 프로세스 P<sub>i</sub>가 R<sub>j</sub>를 k개까지 더 요청할 수 있음
	- Need[i, j] = Max[i, j] - Allocation[i, j]

#### 7.5.3.1. 안전성 알고리즘(Safety Algorithm)
시스템이 안전한지 아닌지 알아낼 수 있는 알고리즘
1. 끝나지 않은 한 프로세스의 Need가 가용 자원보다 적은 것을 찾고, 없다면 3번으로 간다.
2. 프로세스를 끝내고 할당된 자원을 회수한다. 1번으로 돌아간다. 
3. 모든 프로세스가 끝났다면 이 시스템은 안전 상태이다.

#### 7.5.3.2. 자원 요청 알고리즘(Resource-Request Algorithm)
자원 요청을 안전하게 들어줄 수 있는지 검사하는 알고리즘
1. 요청이 Need를 넘어서면 초기에 신고한 최대 요청 수를 넘어서므로 오류로 처리
2. 요청이 가용 자원보다 크면 대기
3. 요청값을 사용하여 Available, Allocation, Need를 수정함
4. 바뀐 상태가 안전하다면 자원을 할당하고, 아니라면 원상태로 복구한뒤 프로세스를 대기

## 7.6. 교착상태 탐지(Deadlock Detection)
교착상태 탐지를 위해 시스템 상태를 검사하는 알고리즘과 교착상태 회복 알고리즘이 필요하다. 

### 7.6.1. 각 자원 타입이 한개씩 있는 경우(Single Instance of Each Resource Type)
자원 할당 그래프를 변형하여 교착상태를 탐지한다. 

<figure style="width: 500px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-31-operating_system_7/3.png' }}" alt=""> 
</figure> 

자원 할당 그래프에서 리소스 노드를 삭제하고, P<sub>i</sub> -> R<sub>q</sub> -> P<sub>j</sub> 의 형태를 P<sub>i</sub> -> P<sub>j</sub>로 바꾼다. 
그리고 사이클을 탐지하여 교착상태인지 탐지한다. 

### 7.6.2. 각 타입의 자원을 여러 개 가진 경우(Several Instance of a Resource Type)
자원 상태를 나타내는 여러 자료구조를 사용한다.
* Available[i] = k
	- 가용 자원 R<sub>i</sub>이 k개 남아있음
* Allocation[i, j] = k
	- 프로세스 P<sub>i</sub>가 R<sub>j</sub>를 k개 사용하고 있음
* Request[i, j] = k
	- 프로세스 P<sub>i</sub>가 R<sub>j</sub>를 k개 요청하고 있음

탐지 알고리즘은 은행원 알고리즘과 유사하다. 
1. 종료되지 않은 프로세스 중 가용 자원보다 적은 요청을 한 프로세스를 찾고, 없다면 3번으로 간다
2. 자원을 반환하고 프로세스를 종료한다. 1번을 반복한다. 
3. 종료되지 않은 프로세스가 존재한다면 시스템은 교착상태이다. 

### 7.6.3. 탐지 알고리즘 사용
교착상태가 일어나는 시점은 어떤 프로세스가 자원을 요청하고 그것이 즉시 만족되지 못하는 시점이다. 

한 극단적인 방법은 프로세스의 요청이 만족되지 않을 때마다 탐지 알고리즘을 돌리는 방법이다. 
한 대안은 지정된 시간 간격, 예를 들면 한 시간에 한번 또는 CPU 이용률이 40% 이하로 떨어질 때에 탐지 알고리즘을 호출하는 것이다. 

## 7.7. 교착상태로부터 회복(Recovery from Deadlock)
교착상태가 발생하면 운영자(operator)에게 통지하는 방법과 시스템이 자동적으로 회복하는 방법이 있다. 

### 7.7.1. 프로세스 종료
프로세스를 중지시킴으로써 프로세스에게 할당되었던 모든 자원을 회수한다. 
교착상태 프로세스를 모두 중지시키거나, 교착상태가 제거될 때까지 한 프로세스씩 중지시킬 수 있다. 
하지만 프로세스가 강제 중지되면 부정확한 파일 갱신, 출력 등이 생길 수 있다. 

### 7.7.2. 자원 선점(Resource Preemption)
교착상태가 사라질때 까지 다른 프로세스의 자원을 계속 선점한다. 
다음 세 가지 상황을 고려해야 한다. 
1. 어느 프로세스와 자원을 가져올 것인가?
	- 프로세스가 점유하고 있는 자원의 수, 실행 시간 등을 고려한다.
2. 자원이 선점될 프로세스를 어떻게 할 것인가?
	- 프로세스를 완전 종료시키거나 교착상태가 사라질 때까지 자원을 선점한다.
3. 기아 상태가 발생하지 않는 것을 어떻게 보장할 것인가?
	- 동일한 프로세스의 자원이 선점되지 않도록, 선점된 횟수를 고려한다. 


