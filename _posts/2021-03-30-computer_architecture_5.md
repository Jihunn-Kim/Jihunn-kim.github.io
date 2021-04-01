---
title: "컴퓨터 구조 요약 - 5. 기본 컴퓨터의 구조와 설계"
last_modified_at: 2021-03-31
show_date: true
classes: wide
excerpt: ""
categories:
  - computer_architecture
---

## 5.1. 명령어 코드(Instruction Code)
컴퓨터의 구조는 레지스터, 타이밍과 제어 구조, 명령어 집합에 의해서 정의된다. 
이 장에서는 규모가 작은 기본 컴퓨터를 다루며, 각 연산이 어떻게 동작하는지를 살펴본다. 

사용자는 연산과 피연산자(operand), 처리되는 순서를 기술한 명령어의 집합인 프로그램을 통해 처리 과정을 제어할 수 있다. 
명령어는 일련의 마이크로 연산을 기술한 이진 코드로서 데이터와 함께 메모리에 저장된다. 

명령어 코드는 컴퓨터에게 어떤 동작을 수행할 것을 알리는 비트들의 집합이다. 
이는 여러 개의 부분으로 구성되며, 구성 형식은 컴퓨터 구조 설계자에 의해 결정된다. 

기본적인 부분은 연산 코드 부분이다. 가감산, 시프트 등의 동작을 정의한 비트들의 집합이다. 
제어 장치는 메모리로부터 연산 코드를 받고 해석하여, 연산의 하드웨어 실행에 필요한 마이크로 연산을 실행하는 일련의 제어 신호를 발생시킨다. 
 
### 5.1.1. 저장 프로그램 구조(Stored Program Organization)
컴퓨터의 가장 간단한 구성은 한 개의 프로세서 레지스터(processor register)를 가짐으로써 두 개의 부분으로 구성된 명령어 코드를 사용하는 것이다. 
프로세서 레지스터는 누산기(accumulator, AC)라고도 불린다. 

Opcode는 실행할 연산을, Address는 피연산자가 저장된 메모리내의 주소를 나타낸다. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-30-computer_architecture_5/1.png' }}" alt=""> 
</figure> 

### 5.1.2. 간접 주소(Indirect Address)
명령어 코드의 주소 부분이 피연산자의 주소를 가르키는 경우, 직접 주소(direct address)를 가지고 있다고 한다. 
반면, 피연산자의 주소를 가지고 있는 메모리의 주소를 가르키는 경우를 간접 주소(indirect address)라고 한다.

피연산자의 주소를 유효 주소(effective address)라고 한다. 

## 5.2. 컴퓨터 레지스터(Computer Registers)
컴퓨터 명령어는 한 번에 하나씩 순차적으로 수행된다. 
따라서 다음에 수행될 명령어의 주소를 알아낼 수 있는 레지스터가 필요하다. 

명령어 주소를 저장하는 레지스터를 포함한 레지스터들이 아래 표에 있다.

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-30-computer_architecture_5/2.png' }}" alt=""> 
</figure> 

### 5.2.1. 공통 버스 시스템(Common Bus System)
컴퓨터에는 레지스터들 사이나 레지스터와 메모리 사이에 정보 전송을 위한 경로가 필요하다. 
이와 같은 경로를 버스 시스템으로 구성하며, 버스 위에 놓이게 될 출력을 선택하기 위해 이진 변수인 선택 입력 S<sub>2</sub>, S<sub>1</sub>, S<sub>0</sub>이 사용된다. 

<figure style="width: 700px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-30-computer_architecture_5/3.png' }}" alt=""> 
</figure> 

## 5.3. 컴퓨터 명령어(Computer Instructions)
기본 컴퓨터 구조는 세 가지 명령어 코드 형식을 가진다.  

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-30-computer_architecture_5/4.png' }}" alt=""> 
</figure> 

### 5.3.1. 명령어 집합의 완전성(Instruction Set Completeness)
컴퓨터는 데이터 처리 작업을 위해 충분한 명령어들을 갖고 있어야 한다. 
1. 산술, 논리, 시프트 명령어
2. 메모리와 프로세서 레지스터 사이에 정보를 이동시키는 명령어
3. 상태 조건을 검사하는 명령과 프로그램 제어 명령어
4. 입력과 출력 명령어

## 5.4. 타이밍과 제어(Timing and Control)
기본 컴퓨터의 레지스터는 주 클럭 발생기에 의해 제어된다. 
그리고 레지스터의 상태 변경을 위해 제어 장치에서 생성된 제어 신호가 인에이블(enable)시켜 주어야 한다.

제어 장치는 하드와이어(hardwired) 제어 방식(5장 내용)과 마이크로 프로그램 제어 방식(7장 내용)의 두 종류가 있다.  
하드와이어 방식은 플립플롭, 디코더 등의 디지털 회로로 구현되며, 최적화되어 더 빠른 연산을 수행할 수 있다. 
하지만, 컴퓨터 구조가 변경되면, 여러 부품들 사이의 배선까지 바꿔야 한다. 

마이크로 프로그램 방식에는 제어 메모리(control memory)가 사용된다. 
제어 메모리는 필요한 일련의 마이크로 연산을 시작할 수 있도록 프로그램되어있다. 
따라서, 메모리의 마이크로 프로그램만 갱신하면 된다.

<figure style="width: 700px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-30-computer_architecture_5/5.png' }}" alt=""> 
</figure> 

기본 컴퓨터의 제어 장치에서는 IR의 명령어와 SC가 생성하는 타이밍 신호가 사용된다. 

## 5.5. 명령어 사이클(Instruction Cycle)
기본 컴퓨터에서 각 명령어 사이클은 다음과 같다.
1. 명령어를 메모리에서 가져온다(fetch).
2. 명령어를 디코딩한다.
3. 간접 주소 방식의 명령어일 경우에 메모리로부터 유효 주소를 읽어온다.
4. 명령어를 실행한다.

### 5.5.1. Fetch 와 디코드
``` console
T_0: AR <- PC
T_1: IR <- M[AR], PC <- PC + 1
T_2: D_0, ..., D_7 <- Decode IR(12-14), AR <- IR(0-11), I <- IR(15)
```

### 5.5.2. 명령어 종류의 결정(Determine the Type of Instruction)
``` console
D_7`IT_3:  AR <- M[AR]
D_7`I`T_3: 아무 일도 하지 않음
D_7I`T_3:  레지스터 참조 명령어 수행
D_7IT_3:   입출력 명령어 수행
```

### 5.5.3. 레지스터 참조 명령어(Register-Reference Instructions)
<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-30-computer_architecture_5/6.png' }}" alt=""> 
</figure> 

## 5.6. 메모리 참조 명령어(Memory-Reference Instructions)
메모리 사이클은 프로세서의 클럭 사이클보다 길다. 사용되는 타이밍 신호가 길다. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-30-computer_architecture_5/7.png' }}" alt=""> 
</figure> 

BUN은 무조건 분기, BSA는 복귀 주소 저장 후 서브루틴 이동, ISZ는 반복처리 및 count에 활용될 수 있다. 

## 5.7. 입출력과 인터럽트(Input-Output and Interrupt)
이 절에서는 단말기와 입력 장치 키보드, 출력 장치 프린터를 예로 한다. 
보다 자세한 내용은 11장에 있다. 

### 5.7.1. 입출력 구조(Input-Output Configuration)
단말기는 정보를 직렬로 주고 받으며, 각 정보의 양은 영숫자 8비트이다. 
키보드로부터의 직렬 정보를 입력 레지스터(INPR)에 시프트되고, 프린터에 대한 직렬 정보는 출력 레지스터(OUTR)에 저장된다. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-30-computer_architecture_5/8.png' }}" alt=""> 
</figure> 

FGI는 제어 플립플롭이다. 이는 새로운 정보가 입력 장치에서 이용 가능한 상태일 때 세트된다. 
이처럼 플래그를 사용한 통신 방법을 프로그램 제어 전송(program controlled transfer)이라 한다. 

### 5.7.2. 입출력 명령어(Input-Output Instructions)

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-30-computer_architecture_5/9.png' }}" alt=""> 
</figure> 

### 5.7.3. 프로그램 인터럽트(Program Interrupt)
플래그를 사용한 통신 방법은 프로세서와 입출력 장치와의 속도 차이 때문에 비효율적이다. 
문자 전송시마다 많은 플래그 체크가 요구된다. 

따라서, 외부 장치가 전송 준비가 되었을 때 컴퓨터에 알리는 방법을 사용한다. 
이를 인터럽트라고 하며, 인터럽트 시 컴퓨터는 현재 실행하고 있는 프로그램을 잠시 중지하고 입출력을 실행한 후 복귀한다. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-30-computer_architecture_5/10.png' }}" alt=""> 
</figure> 

### 5.7.4. 인터럽트 사이클(Interrupt Cycle)

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-30-computer_architecture_5/11.png' }}" alt=""> 
</figure> 

## 5.8. 컴퓨터에 대한 완전한 기술(Complete Computer Description)

<figure style="width: 800px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-30-computer_architecture_5/12.png' }}" alt=""> 
</figure> 

## 5.9. 기본 컴퓨터의 설계(Design of Basic Computer)
기본 컴퓨터는 다음과 같은 하드웨어 요소들로 구성되어 있다.
1. 16비트의 4096워드를 가진 메모리 장치
2. 9개의 레지스터: AR, PC, DR, AC, IR, TR, OUTR, INPR, SC
3. 7개의 플립플롭: I, S, E, R, IEN, FGI, FGO
4. 3x8 동작 디코더와 4x16 타이밍 디코더
5. 16비트 공통 버스
6. 제어 논리 게이트들
7. AC의 입력에 연결된 가산 논리 회로

### 5.9.1. 제어 논리 게이트(Control Logic Gates)
제어 논리 게이트는 그림 5-6에 나온 IR 및 디코딩 된 값과 타이밍 신호 입력 외에 다른 입력들도 받는다. 
AC가 0인지 검사하기 위한 AC(0-15), 부호를 검사하기 위한 AC(15), DR이 0인지 검사하기 위한 DR(0-15), 7개 플립플롭의 값 등이다. 

그리고 출력에는 다음과 같은 것들이 있다. 
1. 9개 레지스터의 입력을 제어하는 신호
2. 메모리의 쓰기/읽기 입력을 제어하는 신호
3. 플립플롭을 세트, 클리어, 보수화시키는 신호
4. 버스를 사용할 레지스터를 선택하는 데 사용되는 S<sub>2</sub>, S<sub>1</sub>, S<sub>0</sub> 신호
5. AC에 대해 가산 논리 회로를 제어하는 신호

## 5.10. 누산기 논리의 설계(Design of Accumulator Logic)
<figure style="width: 800px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-30-computer_architecture_5/13.png' }}" alt=""> 
</figure> 
