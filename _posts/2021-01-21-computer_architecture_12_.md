---
title: "컴퓨터 구조 요약 - 12. 메모리 구조"
last_modified_at: 2021-04-03
show_date: true
classes: wide
excerpt: ""
categories:
  - computer_architecture
---

## 12.1. 메모리 계층(Memory Hierarchy)
기억 장치는 CPU에 의해 실행될 프로그램이 저장되는 곳으로 주기억 장치와 보조 기억 장치 두 가지로 분류된다.  
주기억 장치는 CPU와 직접 통신이 가능하며, CPU가 현재 사용하는 정보가 저장되어 있다.  
보조 기억 장치에는 이를 제외한 모든 정보가 저장되어 있다. 

메모리 계층 시스템은 보조 기억 장치, 주 기억 장치, 캐시 메모리 등으로 구성된다. 
캐시 메모리로 갈수록 접근 속도는 빨라지지만 비싸고 용량이 작아진다. 
<a href="https://stackoverflow.com/questions/3699582/what-is-the-difference-between-l1-cache-and-l2-cache" target="_blank"><b> 캐시 메모리는 L1, L2, L3로 나뉜다. </b></a> 
숫자가 작을수록 더 빠르고, 컴퓨터 구조마다 다르지만 CPU와 가까이 있어 <a href="https://softwareengineering.stackexchange.com/questions/234253/why-is-cpu-cache-memory-so-fast" target="_blank"><b>데이터 버스를 거치지 않고 프로세서에게 데이터가 전달될 수 있다. </b></a>

---

## 12.2 주기억 장치(Main Memory)
주기억 장치는 컴퓨터가 동작하는 동안 데이터와 프로그램을 저장하고 있는 메모리이다. 

RAM이 주로 사용되며 일부는 ROM으로 구성된다. 
RAM은 읽고 쓰기가 가능하며, 변경될 가능성이 있는 프로그램이나 데이터를 저장한다. 

ROM은 읽기만 가능하며, 불변 상수, 표 등을 저장한다. 
특히 부트스트랩 로더(bootstrap loader)라고 불리는 초기화 프로그램이 저장되어 있다. 
이는 전원이 켜졌을 때, 운영체제의 일부분을 디스크에서 주기억 장치로 적재하고 제어를 운영체제로 옮겨준다. 

컴퓨터는 전원이 켜지는 순간 프로그램 카운터의 값을 부트스트랩 로더의 시작 주소로 세팅하는 기능을 제공한다. 

### 12.2.1. RAM and ROM Chips
RAM과 ROM은 공통적으로 칩 선택 입력과 메모리 주소 입력이 있다. 
여러 개의 RAM칩과 ROM 사용될 때, 특정 칩을 선택하는데 필요하다. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-21-computer_architecture_12/1.png' }}" alt=""> 
</figure> 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-21-computer_architecture_12/2.png' }}" alt=""> 
</figure> 

### 12.2.2. 메모리 주소 맵(Memory Address Map)

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-21-computer_architecture_12/3.png' }}" alt=""> 
</figure> 

### 12.2.3. CPU로의 메모리 연결(Memory Connection to CPU)

<figure style="width: 700px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-21-computer_architecture_12/4.png' }}" alt=""> 
</figure> 

## 12.3. 보조 기억 장치(Auxiliary Memory)
가장 많이 사용되는 보조 기억 장치는 자기 디스크이다. 
기억 장치의 주요한 특성은 접근 시간, 전송룔 등이 있다. 

접근 시간에는 read-write 헤드가 지정된 기억 장소에 도달하는데 필요한 seek time 과 
데이터를 전송하는 데 요구되는 transfer time 이 있다. 
전송룔은 장치가 분당 전송할 수 있는 문자나 워드의 수를 나타낸다. 

### 12.3.1. 자기 디스크(Magnetic Disks)
자기 디스크에는 자화 물질이 입혀진 여러 원형 금속판(디스크)이 하나의 축을 중심으로 모여있다. 
각 디스크마다 read-write 헤드가 있으며, 모든 디스크들은 빠른 속도로 함께 회전한다. 

중심원에서 같은 거리에서 떨어진 비트들을 트랙이라 부른다. 
그리고 트랙은 섹터라 불리는 영역으로 나뉜다. 

<figure style="width: 400px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-21-computer_architecture_12/5.png' }}" alt=""> 
</figure> 

사용할 디스크 번호, 트랙, 섹터가 지정되면 read-write 헤드가 지정된 트랙에 위치한 후, 지정된 섹터가 올 때까지 기다린다. 
섹터가 도달하면 데이터 전송을 시작한다. 

## 12.4. 어소시어티브 메모리(Associative Memory)
데이터 처리에는 탐색이 필요하다. 
특정 주소의 메모리 내용을 읽어, 같은 항목을 찾을 때까지 찾을 항목과 읽은 정보를 비교한다. 

만약 데이터의 내용으로 탐색을 할 수 있다면 탐색 시간을 줄일 수 있다. 
내용으로 접근 되는 메모리 장치를 어소시어티브 메모리 또는 내용 지정 메모리(content addressable memory, CAM)라 부른다. 
이는 저장 능력 뿐 아니라 내용을 비교하기 위한 논리 회로를 갖고 있기 때문에 RAM보다 값이 비싸다. 

### 12.4.1. 하드웨어 구성(Hardware Organization)
어소시어티브 메모리에는 비교할 인자와, 인자의 비트 중 비교할 비트를 선택하는 마스크 비트가 들어온다. 
이후 어소시어티브 메모리 내용과 인자를 비교하여, 일치한다면 매치(match) 레지스터의 값을 1로 세트한다. 

<figure style="width: 500px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-21-computer_architecture_12/6.png' }}" alt=""> 
</figure> 

어소시어티브 메모리가 m개의 words를 가진다면 매치 레지스터 또한 m개의 비트를 가지며, 
n번째 매치 레지스터 값이 1이라면 n번째 어소시어티브 메모리와 비교 내용이 일치한다는 뜻이다. 
따라서 해당 내용을 읽어온다. 

<figure style="width: 400px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-21-computer_architecture_12/7.png' }}" alt=""> 
</figure> 

### 12.4.2. 쓰기 동작
어소시어티브 메모리에 쓰기 동작을 수행하려면 tag레지스터가 추가적으로 필요하다. 
이는 활성화된 워드와 그렇지 않은 워드를 구별하기 위한 레지스터이다. 

새로 내용을 삽입하거나 기존 내용을 삭제하려면 tag레지스터의 비트를 0으로 클리어하면 된다. 
tag레지스터를 사용할 때, 읽기 동작은 tag 레지스터의 비트 값이 1인 워드만 비교한 후 해야한다.

## 12.5. 캐시 메모리(Cache Memory)
많은 프로그램들은 주어신 시간 동안 메모리 중 국한된 영역만 참조하는 경향이 있다. 
순차적인 프로그램 루프, 서브루틴와 공통 배열 참조와 같은 반복 절차가 이에 해당한다. 

자주 참조되는 프로그램와 데이터를 속도가 빠른 캐시 메모리에 저장한다면 평균 메모리 접근 시간을 줄일 수 있다. 
따라서, CPU는 메모리에 접근할 필요가 있을 때 캐시를 먼저 조사한다. 

캐시에서 발견되면 이를 읽어들이고, 아니라면 주기억 장치에서 읽는다. 
캐시에서 발견될 확률을 히트율(hit ratio)라 하며 보통 0.9 이상의 값을 갖는다. 

캐시 구조는 다음과 같은 방법이 있다. 
1. 어소시어티브 매핑
2. 직접 매핑
3. 세트-어소시어티브 매핑

### 12.5.1. 어소시어티브 매핑(Associative Mapping)
가장 빠른 캐시 구조는 어소시어티브 메모리를 사용하는 것이다. 
어소시어티브 메모리에 주소와 데이터를 저장하고, CPU가 주소를 보내면 매치되는 데이터를 출력한다. 

<figure style="width: 400px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-21-computer_architecture_12/8.png' }}" alt=""> 
</figure> 

### 12.5.2. 직접 매핑(Direct Mapping)
직접 매핑은 CPU주소에 tag필드와 index필드를 두어 RAM과 캐시에 접근하는 방식이다. 
먼저 index에 적힌 주소 비트로 캐시에 접근하고, 없으면 index 비트 앞에 tag 비트를 덧붙여 RAM 주소를 만들어낸다. 

<figure style="width: 500px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-21-computer_architecture_12/9.png' }}" alt=""> 
</figure> 

캐시에 8워드 블록씩 저장하며, index 필드를 block, word 필드로 나누는 방법도 있다. 

<figure style="width: 500px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-21-computer_architecture_12/10.png' }}" alt=""> 
</figure> 

### 12.5.3. 세트-어소시어티브 매핑(Set-Associative Mapping)
이 방법은 캐시에 같은 index 주소 밑에 두 개 이상의 메모리 워드를 저장할 수 있는 방법이다. 
같은 인덱스여도 tag가 다르다면 캐시에 저장할 수 있다. 
하지만 더 많은 비교 논리 회로가 필요하다. 

<figure style="width: 500px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-21-computer_architecture_12/11.png' }}" alt=""> 
</figure> 

### 12.5.4. 캐시에 기록(Writing into Cache)
write-through 혹은 write-back 방식으로 주기억 장치의 내용을 갱신한다. 

write-through 방식은 캐시메모리와 주기억 장치를 같이 갱신하는 방법이다. 
따라서, 주기억 장치는 항상 캐시와 같은 유효한 데이터를 가지고 있다. 

write-back 방식은 캐시의 내용만 갱신하며, 플래그를 추가로 사용하여 이를 표시한다. 
플래그가 표시된 워드가 캐시로부터 제거될 때 주기억 장치에 복사된다. 
이 방법을 쓰는 이유는 워드가 캐시에 있는 동안 여러번 갱신될 수 있기 때문이다. 

### 12.5.5. 캐시의 초기값 설정(Cache Initialization)
캐시는 초기에 유효하지 않는 데이터를 포함하고 있다. 
따라서 캐시의 워드가 유효한 데이터를 포함하는지 가르키는 유효 비트를 사용한다. 
초기에는 모든 유효 비트가 0이고, 워드가 주기억 장치로부터 전송되면 1로 세트된다. 

## 12.6. 가상 메모리(Virtual Memory)
가상 메모리란 사용자가 보조 기억 장치를 사용하여 프로그램을 개발할 수 있도록 하는 개념이다. 

### 12.6.1. 주소 공간과 메모리 공간(Address Space and Memory Space)
프로그래머에 의하여 쓰여진 주소를 가상 주소라 하며, 이들의 집합을 주소 공간이라 한다. 
한편 주기억 장치의 주소를 물리적 주소라 하며, 이들의 집합을 메모리 공간이라 한다. 
가상 주소는 주소 매핑을 통하여 주기억 장치의 실제 주소로 전환되어야 한다. 

### 12.6.2. 페이지를 사용하는 주소 매핑(Address Mapping Using Pages)
주소 매핑은 주소 공간과 메모리 공간의 정보가 고정된 크기의 그룹으로 나뉘어지면 간단하게 구성할 수 있다. 
같은 크기로 나누어진 메모리 공간을 블럭이라 하며, 나누어진 주소 공간을 페이지라 한다. 

가상 주소를 페이지 번호와 페이지내의 주소로 나누어 주소 매핑을 하는 방법이 있다. 
여기에는 페이지 번호에 해당하는 페이지가 몇 번째 블록인지 나타내는 테이블이 필요하다. 

이는 메모리 페이지 테이블이라 불리며, 메모리 테이블 퍼버 레지스터에 저장된다. 
이를 통해 블록 번호를 매핑한 후, 페이지내 주소를 통해 주기억 장치를 읽는다. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-21-computer_architecture_12/12.png' }}" alt=""> 
</figure> 

테이블의 현존(presence) 비트는 페이지가 보조 기억 장치로부터 주기억 장치에 전송되었는지의 여부를 가르킨다. 
현존 비트가 0이면 해당 페이지가 메모리내에 없으므로, 페이지를 보조 기억 장치로부터 주기억 장치에 옮긴다. 

### 12.6.3. 어소시어티브 메모리 페이지 테이블(Associative Memory Page Table)
메모리 페이지 테이블에 1024개의 페이지와 32개의 블럭이 있다면, 테이블의 32개의 현존 비트만 1이고 나머지 992개는 쓰이지 않는다. 
테이블을 효율적으로 구성하기 위한 방법은 워드의 수를 주기억 장치의 블럭의 수와 같게 하는 것이다. 

이는 어소시어티브 메모리를 통해 구현될 수 있다. 
페이지 번호와 블럭 번호를 매칭시키는 테이블을 어소시어티브 메모리에 저장시킨다. 
만약 매치가 일어나지 않으면 페이지를 보조 기억 장치로부터 기억 장치로 옮긴다. 

<figure style="width: 400px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-21-computer_architecture_12/13.png' }}" alt=""> 
</figure> 

### 12.6.7. 페이지 교체(Page Replacement)
가상 메모리는 하드웨어와 소프트웨어의 결합체이다. 
메모리 관리 소프트웨어 시스템은 메모리 공간의 효율적 이용을 위하여 소프트웨어 동작을 조정한다. 
1. 새로운 페이지를 위해 주기억 장치에서 어떤 페이지를 삭제할지 정함
2. 언제 새 페이지를 보조 기억 장치로부터 주기억 장치로 옮길지 결정
3. 페이지가 주기억 장치 어디에 저장될지 결정

프로그램 동작 중, 참조하려는 페이지가 주기억 장치에 없는 페이지 결함(page fault)상태가 생긴다면, 페이지가 주기억 장치로 옮겨질 때까지 실행이 중단된다. 
보조 기억 장치로부터 주기억 장치로 페이지를 전송하는 것은 입출력 동작이기 때문에, I/O프로세서가 이 일을 맡는다. 

주기억 장치가 가득 찬 상태라면 제거할 페이지를 선택해야 한다. 
이를 위한 교체 알고리즘에는 first-in, first-out(FIFO)와 least recently used(LRU) 등이 있다. 

FIFO 알고리즘은 주기억 장치 안에 가장 오래 있었던 페이지를 제거한다.  
LRU는 주기억 장치에 있는 각 페이지마다 카운터를 설치한다. 
이 카운터는 일정 시간 마다 1씩 증가하며, 해당 페이지가 참조될 때 0으로 초기화 된다. 
최근에 가장 적게 쓰인 페이지가 가장 높은 카운터 값을 가지므로 이를 제거한다. 

## 12.7. 메모리 관리 하드웨어(Memory Management Hardware)
많은 프로그램이 메모리내에 존재하는 멀티프로그래밍에서는 프로그램과 데이터를 메모리 사이에서 이동시키고, 
사용되는 메모리의 양을 조절하고, 프로그램이 다른 프로그램을 바꾸지 못하도록 하는 것이 필요하다. 

이러한 역활을 하는 메모리 관리 시스템은 여러 프로그램을 관리하기 위한 하드웨어와 소프트웨어의 집합체이다. 
메모리 관리 소프트웨어는 OS의 일부이므로 여기서는 하드웨어를 논한다. 

메모리 관리 장치의 기본적인 역활은 다음과 같다. 
1. 가상 메모리 참조를 물리 메모리 주소로 변환하는 동적 저장 장소 재배치(세그먼트)
	- 고정적인 크기를 가진 페이지 시스템과 비슷하다.
	- 크기가 가변적이며 프로그램과 데이터를 논리 부분으로 나누어 관리한다.
2. 서로 다른 사용자가 하나의 프로그램을 같이 사용하기 위한 편의 제공
	- 컴파일러같은 프로그램을 각 사용자들이 메모리에 각각 복사하기보다는 하나만 복사하여 이를 공용하도록 함.
3. 사용자간 허락되지 않은 접근 방지 및 OS의 기능을 변경하지 못하도록 하는 정보의 보호

세그먼트가 사용되면 논리 주소는 세그먼트 주소, 페이지 주소, 워드 주소 필드로 구성된다. 
각 세그먼트의 크기는 실행되는 프로그램에 따라 변화하므로, 세그먼트를 다시 같은 크기의 페이지로 나누어 관리한다. 

<figure style="width: 700px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-21-computer_architecture_12/14.png' }}" alt=""> 
</figure> 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-21-computer_architecture_12/15.png' }}" alt=""> 
</figure> 

하지만 논리 주소에서 물리 주소로 변환하는 것은 시간이 많이 소요된다. 
세그먼트 테이블에서 페이지를 찾고, 페이지 테이블에서 블록을 찾고, 워드로 메모리 물리 주소를 찾는 세번의 참조가 필요하다. 

이를 해결하기 위해 세그먼트, 페이지, 블록을 어소시어티브 메모리에 저장하여 주소를 변환한다. 
어소시어티브 메모리에서 찾지 못할 경우, 느린 변환을 사용한 후 어소시어티브 메모리에 저장한다. 

### 12.7.1. 메모리 보호(Memory Protection)
세그먼트 테이블에 있는 페이지의 베이스 주소 외에 길이와 보호 필드를 추가하여 메모리를 보호할 수 있다. 

<figure style="width: 400px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-21-computer_architecture_12/16.png' }}" alt=""> 
</figure> 

길이 필드는 각 세그먼트에 지정되는 최대의 페이지수를 나타낸다. 
보호 필드는 특정한 세그먼트에 허락할 수 있는 접근의 권한을 지정한다. 

접근 권한의 종류는 다음과 같다. 
1. Full read and write
2. Read only(write protection)
	- 유틸리티, 라이브러리같은 공통 프로그램에 유용
3. Execute only(program protection) 
	- 프로그램 복사 방지
4. System only(operating system protection) 

