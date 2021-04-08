---
title: "운영체제 요약 - 6. 프로세스 동기화"
last_modified_at: 2021-04-06
show_date: true
classes: wide
excerpt: ""
categories:
  - operating_system
---

## 6.1. 배경(Background)
두 개의 프로세스가 동시에 공유 데이터를 조작한다면 의도치 않은 결과가 나올 수 있다. 
실행 결과가 접근이 발생한 순서에 따라 바뀌는 상황을 경쟁 상황(race condition)이라고 한다. 
이를 해결하기 위해, 한 순간에 한 프로세스만이 공유 데이터를 조작하도록 프로세스들을 동기화해야 한다. 

## 6.2. 임계구역 문제(The Critical-Section Problem)
각 프로세스는 임계구역이라 불리는 코드 부분을 포함하고 있을 수 있다. 
이 코드는 공유 데이터를 조작한다. 

프로세스가 임계구역에 진입하려면 진입허가를 요청해야 한다. 
요청을 하는 코드 부분을 진입 구역(entry section), 임계구역 뒤에는 퇴출 구역(exit section)이라 한다. 

임계구역 문제에 대한 해결안은 다음의 세 가지 요구조건을 충족해야 한다. 
* 상호 배제(mutual exclusion)
	- 한 프로세스가 임계구역에서 실행된다면, 다른 프로세스들은 임계구역에서 실행될 수 없다.
* 진행(progress)
	- 임계구역이 실행되지 않고 있다면, 다음에 어떤 프로세스들이 임계구역에 진입할지 정해야 한다.
* 한정된 대기(bounded waiting)
	- 한 프로세스가 무한히 임계구역에 들어가지 못하면 안된다. 

## 6.3. 피터슨의 해결안(Peterson's Solution)
단지 두 개의 프로세스만 있다고 가정된다. 
임계구역 진입 순번 turn, 임계구역 진입 준비 완료 flag 공유 변수를 진입 구역과 퇴출 구역에서 조작한다. 

<figure style="width: 500px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-30-operating_system_6/1.png' }}" alt=""> 
</figure> 

## 6.4. 동기화 하드웨어(Synchronization Hardware)
단일 처리기 환경에서 임계구역 문제는 공유 데이터가 변경되는 동안 인터럽트 발생을 막음으로써 해결할 수 있다. 

다중 처리기 환경에서는 하드웨어 명령어를 통해 임계구역 문제를 해결할 수 있다. 
많은 기계들은 한 워드(word)의 내용을 검사하고 변경하거나, 두 워드의 내용을 원자적으로 교환할 수 있는, 즉 인터럽트 되지 않는 하나의 단위로서, 특별한 하드웨어 명령어를 제공한다. 

test_and_set은 특정 기계 명령어의 추상화된 형태이다. 
만약 두 개의 CPU에서 명령어가 동시에 실행된다면, 임의의 순서로 ``순차적으로`` 실행될 것이다. 

<figure style="width: 450px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-30-operating_system_6/2.png' }}" alt=""> 
</figure> 

<figure style="width: 550px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-30-operating_system_6/3.png' }}" alt=""> 
</figure> 

## 6.5. Mutex Locks
하드웨어 기반 해결책은 응용 프로그래머는 사용할 수가 없다. 
따라서 mutext lock 이라는 소프트웨어 도구를 사용한다. 

프로세스는 임계구역에 들어가기 전에 반드시 lock을 획득해야 하고 임계구역을 빠져 나올 때 lock을 반환해야 한다. 
lock 획득과 반환 함수는 원자적으로 수행되어야 하므로 하드웨어 기법을 통해 구현된다. 

한 프로세스가 임계구역에 있는 동안 다른 프로세스들은 lock 획득 함수를 계속 실행하게 된다. 
이를 바쁜 대기(busy waiting) 혹은 spinlock 이라 한다. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-30-operating_system_6/4.png' }}" alt=""> 
</figure> 

프로세스들이 짧은 시간 동안만 임계구역에 있는다면 이런 방식도 유용하지만, 
오랜 시간 다른 작업을 못할 수도 있다. 

## 6.6. 세마포(Semaphores)
세마포는 정수 변수로서, 원자적 연산인 wait와 signal 함수로만 접근 가능하다. 

<figure style="width: 250px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-30-operating_system_6/5.png' }}" alt=""> 
</figure> 

### 6.6.1. 세마포 사용법(Semaphore Usage)
자원을 사용하려는 프로세스는 wait 함수를 통해 세마포 값이 0이 아니라면 이를 감소시킨다. 
프로세스가 자원을 방출할 때는 signal 함수를 사용하여 세마포 값을 증가시킨다. 

세마포는 동기화 문제에도 사용될 수 있다. 
프로세스 2의 S2가 프로세스 1의 S1이 끝난 뒤에 수행되게 하는 방법은 다음과 같다. 

```console
synch = 0;

P1 의 명령어
S1;
signal(synch);

P2의 명령어
wait(synch);
S2;
```

### 6.6.2. 구현(Implementation)
바쁜 대기를 해결하기 위해서 프로세스는 대기 상태로 전환될 수 있다. 
세마포 값이 양수가 아니라면 프로세스를 세마포에 연관된 대기 큐에 넣고, 제어를 CPU 스케줄러에게 넘겨 다른 프로세스를 실행시킨다. 
이후 다른 프로세스가 임계구역에서 나오면서 대기 큐에 있던 프로세스를 깨워 실행을 재개시킨다. 

<figure style="width: 500px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-30-operating_system_6/6.png' }}" alt=""> 
</figure> 

### 6.6.3. 교착 상태와 기아(Deadlock and Starvation)
한 집합 내의 모든 프로세스들이 그 집합 내의 다른 프로세스만이 유발할 수 있는 사건을 기다릴 때, 프로세스들이 교착 상태에 있다고 한다. 

<figure style="width: 300px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-30-operating_system_6/7.png' }}" alt=""> 
</figure> 

기아 상태는 프로세스들이 무한정 대기하는 것이다. 
세마포와 연관된 큐가 프로세스들을 후입 선출(last-in, first-out, LIFO)순서로 꺼내온다면 발생할 수 있다. 

### 6.6.4. 우선순위 역전(Priority Inversion)
낮은 우선순위 프로세스가 사용하던 자원이 높은 우선순위 프로세스에 의해 선점되는 경우, 낮은 우선순위 프로세스는 기다려야 하는 시간이 늘어난다. 
이를 우선순위 역전문제라 하며, 우선순위 상속 프로토콜(priority-inheritance protocol)로 해결한다. 

높은 우선순위 프로세스가 필요로 하는 자원을 사용하는 모든 프로세스들에 잠시 높은 우선순위를 주는 것이다. 
자원의 사용이 끝날 때까지 높은 우선순위를 상속받다가, 사용이 끝나면 원래 우선순위로 돌아간다. 

## 6.7. 고전적인 동기화 문제들
소비자-생산자의 유한 버퍼 문제, 데이터베이스 읽기-쓰기 문제, 식사에 젓가락(자원) 두개가 필요한 식사하는 철학자들 문제가 있다. 
새롭게 제안된 동기화 방법들을 검증하는 데 사용된다. 

## 6.8. 모니터(Monitors)
세마포를 이용할 때 프로그래머의 실수로 인해 다양한 오류가 쉽게 발생할 수 있다. 
이를 해결하기 위해 고급 언어 구조물(constructs)들이 개발되었으며, 그중 하나가 모니터이다. 

<figure style="width: 550px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-30-operating_system_6/8.png' }}" alt=""> 
</figure> 

### 6.8.1. 모니터 사용법(Monitor Usage)
**모니터 형**은 모니터 내부에서 상호 배제가 보장되는, 프로그래머가 정의한 연산자 집합을 포함하는 추상화 된 데이터 형(abstract data type, ADT)이다. 
또한 한 인스턴스의 상태를 나타내는 공유 변수들과 이에 대한 초기화 함수를 포함하고 있다. 
모니터 구조물은 모니터 안에서는 오직 하나의 프로세스만이 활성화 되도록 보장해 준다. 

모니터는 **condition형** 변수를 통해 부가적인 동기화 기법을 제공한다. 
condition 변수에는 wait, signal 함수밖에 없다. 
그리고 여러 프로세스가 하나의 condition 변수에 연관될 수 있다. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-30-operating_system_6/9.png' }}" alt=""> 
</figure> 

모니터에는 여러 큐가 존재할 수 있다. 
사용 중인 모니터에 접근 대기하는 프로세스들은 entry queue에서 대기한다. 
해당 condition을 기다리는 프로세스들은 condition queue에서 대기한다. 
모니터 내부 작업중 잠시 프로세스가 대기하는 next queue도 있다.

### 6.8.3. 세마포를 이용한 모니터의 구현
각 모니터에는 mutex라는 세마포가 있으며 초기 값은 1이다. 
프로세스는 모니터에 들어가기 전 wait(mutex)를 실행하고 나올 때 signal(mutext)를 해야 한다. 

signaling 프로세스는 실행 재개된 프로세스가 모니터를 떠나거나 wait() 할 때까지 기다려야 하므로 next라는 0으로 초기화된 세마포를 추가로 사용한다.
또한 정수 변수 next_count도 일시 중단된 프로세스 개수를 세기 위해 사용된다. 
```console
모니터 상호 배제를 보장한 실행

wait(mutex)
...
...
if (next_count > 0)
	signal(next)
else
	signal(mutex)
```

Condition 변수 및 wait, signal 함수를 세마포로 구현하는 방법은 다음과 같다. 
조건 변수 x에 초기 값 0인 x_sem, x_count 를 도입한다. 
```console
// x.wait()

x_count++
if (next_count > 0)
	signal(next)
else
	signal(mutex)
wait(x_sem)
x_count--
```

```console
// x.signal()

if(x_count > 0) {
	next_count++
	signal(x_sem)
	wait(next)
	next_count--
}
```

### 6.8.4. 모니터 내에서 프로세스 수행 재개
Condition 변수 x에 여러 프로세스들이 일시중단 되어 있고 어떤 프로세스가 x.signal()을 수행했을 때, 어느 프로세스가 수행 재개될지 정해야 한다. 
간단한 방법은 가장 오래 기다렸던 프로세스를 실행시키는 것이다. 

다른 방법은 우선순위를 사용하는 것이다. x.wait(c) 처럼 프로세스의 우선순위를 c로 나타내면서 대기한다. 
이후 x.signal()은 가장 작은 우선순위를 가진 프로세스를 깨울 수 있다. 

모니터와 세마포 모두 아래와 같은 상황에서 문제가 발생할 수 있다. 
* 프로세스가 허락을 받지 않고 자원에 접근
* 프로세스가 자원을 방출하지 않음
* 프로세스가 허락 없이 자원을 방출
* 프로세스가 자원 방출없이 자원을 다시 요구

프로세스들이 올바른 순서를 지키도록 보장하기 위해서는 14장에서 설명될 기법을 사용해야 한다.

## 6.9. 대체 방안들

### 6.9.1. 트랜잭션 메모리(Transactional Memory)
메모리 트랜잭션은 원자적인 메모리 읽기와 쓰기 연산의 연속적 순서이다. 
한 트랜잭션의 모든 연산이 완수되면 메모리 트랜잭션은 확정(commit)된다. 
그렇지 않다면 그 시점까지 수행된 모든 연산들은 취소되고 트랜잭션 시작 이전 상태로 되돌려진다(roll-back). 

트랜잭션 메모리는 소프트웨어 또는 하드웨어로 구현될 수 있다. 
소프트웨어 트랜잭션 메모리(STM)는 컴파일러가 명령문이 동시에 실행되거나 locking이 필요한 곳에 검사 코드를 삽입함으로써 동작한다. 
하드웨어 트랜잭션 메모리(HTM)는 기존의 캐시 계충 구조와 캐시 일관성 프로토콜을 변경해서 구현한다. 

### 6.9.2. OpenMP
{% raw %} #paragma omp ciritical {% endraw %}로 임계구역을 설정하여 한 번에 하나의 스레드만 실행할 수 있게 한다. 

### 6.9.3. 함수형 프로그래밍 언어(Functional Programming Languages)
함수형 프로그래밍 언어에서는 변수가 정의되어 값을 배정받으면 그 값은 변경될 수 없기 때문에 경쟁 조건이나 교착상태를 신경쓸 필요가 없다. 
<a href="https://en.wikipedia.org/wiki/Constant_(computer_programming)"><b> In functional programming, data are typically constant by default </b></a>
