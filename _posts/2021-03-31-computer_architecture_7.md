---
title: "컴퓨터 구조 요약 - 7. 마이크로 프로그램된 제어"
last_modified_at: 2021-03-31
show_date: true
classes: wide
excerpt: ""
categories:
  - computer_architecture
---

## 7.1. 제어 메모리(Control Memory)
제어 장치는 요구되는 마이크로 연산들을 연속적으로 수행하게 하는 신호를 보냄으로써 명령어를 수행하게 한다. 

특정한 마이크로 연산을 수행하기 위해서는 이를 위한 이진 비트가 필요하다. 
이런 비트들의 모임을 제어 워드라 하며, 어떤 명령을 수행할 수 있도록 된 일련의 제어 워드가 기억 장치(ROM/RAM 등) 속에 저장될 때, 이를 마이크로 프로그램이라 한다. 

또, 이러한 방식의 제어 장치를 마이크로 프로그램된 제어 장치라 하며, 제어 워드를 마이크로 명령어라 부르기도 한다. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-31-computer_architecture_7/1.png' }}" alt=""> 
</figure> 

다음에 수행될 마이크로 명령어의 위치는 제어 메모리의 어디에나 있을 수 있기 때문에, 다음 주소 생성기가 사용된다. 
제어 주소 레지스터는 마이크로 명령어의 주소를 지정하고, 제어 데이터 레지스터는 마이크로 명령어를 보관한다. 

## 7.2. 주소 시퀀싱(Address Sequencing)
제어 메모리에서 주소를 결정하는 방법은 다음 중 하나이다.
1. 제어 주소 레지스터 값을 하나 증가
2. 무조건 분기와 상태 비트 조건에 따른 조건부 분기
3. 명령어의 비트들로부터 제어 메모리의 주소를 매핑
4. 서브루틴을 호출하고, 복귀

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-31-computer_architecture_7/2.png' }}" alt=""> 
</figure> 

### 7.2.1. 조건부 분기(Conditional Branching)
분기 논리(branch logic)에서는 상태 비트를 사용하여, 주어진 주소로 분기할지 말지를 결정한다. 
무조건 분리는 상태 비트를 고정시켜 구현할 수 있다. 

### 7.2.2. 명령어의 매핑(Mapping of Instruction)
명령어의 OP-코드를 매핑하여 제어 메모리의 주소를 생성한다. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-31-computer_architecture_7/3.png' }}" alt=""> 
</figure> 

### 7.2.3. 서브루틴(Subroutines)
흔히 많은 마이크로 프로그램이 동일한 코드의 모임을 같게 되는데, 이 부분을 서브루틴화함으로써 제어 메모리를 절약할 수 있다. 
예를 들어, 피연산자의 유효 주소를 계산하는 것은 메모리 참조 명령어 모두가 사용하므로 서브루틴으로 만드는 것이 편리하다. 

리턴 주소는 스택 구조의 레지스터를 사용하는 것이 편리하다. 

## 7.3. 마이크로 프로그램의 예(Microprogram Example)

### 7.3.1. 마이크로 명령어 형식(Microinstruction Format)

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-31-computer_architecture_7/4.png' }}" alt=""> 
</figure> 

### 7.3.2. 기호로 표시된 마이크로 명령어(Symbolic Microinstructions)

<figure style="width: 800px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-31-computer_architecture_7/5.png' }}" alt=""> 
</figure> 

### 7.3.3. 기호로 표시된 마이크로 프로그램(Symbolic Microprogram)

<figure style="width: 700px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-31-computer_architecture_7/6.png' }}" alt=""> 
</figure> 

### 7.3.4. 이진 마이크로 프로그램(Binary Microprogram)

<figure style="width: 800px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-31-computer_architecture_7/7.png' }}" alt=""> 
</figure> 

## 7.4. 제어 장치의 설계(Design of Control Unit)

<figure style="width: 700px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-31-computer_architecture_7/8.png' }}" alt=""> 
</figure> 

### 7.4.1. 마이크로 프로그램 시퀀서(Microprogram Sequencer)

<figure style="width: 700px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-31-computer_architecture_7/9.png' }}" alt=""> 
</figure> 


