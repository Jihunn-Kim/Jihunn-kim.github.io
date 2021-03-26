---
title: "컴퓨터 구조 요약 - 1. 디지털 논리 회로"
last_modified_at: 2021-03-26
show_date: true
classes: wide
excerpt: ""
categories:
  - computer_architecture
---

## 1.1. 디지털 컴퓨터(Digital Computers)
CPU는 데이터를 조작하는 산술 및 논리 연산 부분, 데이터를 저장하는 레지스터, 명령어를 수행하는 제어 회로 등이 있다.  
RAM은 명령어와 데이터를 저장하고 있다.  
IOP는 컴퓨터와 외부와의 통신 및 데이터 전송을 제어한다.

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-15-computer_architecture_1/1.png' }}" alt=""> 
</figure> 

---

## 1.2. 논리 게이트(Logic Gates)
컴퓨터에서 이진 정보는 전압 신호를 사용하여 0 혹은 1을 나타낸다. 예를 들어 3v 는 1로, 0.5v 는 0을 나타낸다.  
이진 정보들은 OR, NAND 와 같은 논리 회로에서 처리된다.  
회로의 입출력 관계를 나타내는 진리표(truth table) 및 논리도(logic diagram)가 존재하며, 간소화 기법을 활용하여 회로를 최대한 단순하게 만든다.

## 1.3. 부울 대수(Boolean Algebra)
부울 대수는 이진 변수와 논리 동작을 다루는 대수(algebra)이다.  
진리표와 논리도의 입출력 관계를 대수 형식으로 표기하여, 이는 디지털 회로의 해석과 설계를 도와준다. 

## 1.4. 맵의 간소화(Map Simplification)
진리표로부터 논리 표현식을 얻을 수 있다.  
부울 대수를 사용하여 간소화된 식을 얻을 수 있다. 

## 1.5. 조합 회로(Combinational Circuits)
조합 회로는 입력과 출력을 가진 논리 게이트의 집합이다. 반가산기, 전가산기 등이 있다. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-15-computer_architecture_1/2.png' }}" alt=""> 
</figure> 

## 1.6. 플립-플롭(Flip-Flops)
조합 회로는 현재 입력에만 대응되는 출력을 가진다. 
반면, 순차 회로는 저장 요소를 가진다. 

순차 회로는 대부분 동기형(synchronous) 회로이다. 
이는 클럭 펄스(clock pulses) 신호가 회로에 같이 입력될 때에만 출력이 변한다. 
이러한 동기화(synchronization)에는 주기적으로 클럭 펄스를 생성하는 타이밍 디바이스(timing device; clock pluse generator라 불림)가 사용된다. 

클럭 동기식 순차 회로에 사용되는 저장 요소를 플립-플롭이라 하며, 이는 한 비트의 정보를 저장한다. 
SR, JK, T 플립-플롭 등이 있다. 

입력과 플립-플롭 현재 상태를 알 때, 다음 상태를 나타내는 표를 여기표(excitation)라 한다. 

## 1.7. 순차 회로(Sequential Circuits)
순차 회로는 플립-플롭과 게이트들을 서로 연결한 것이다. 이진 카운터 등이 있다. 

입력과 현재 상태의 조합에 대한 출력과 다음 상태를 나타낸 상태 표(state table)과 이를 그림으로 표시한 상태 도(state diagram)이 있다.  

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-15-computer_architecture_1/3.png' }}" alt=""> 
</figure> 

