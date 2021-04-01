---
title: "컴퓨터 구조 요약 - 4. 레지스터 전송과 마이크로 연산"
last_modified_at: 2021-03-30
show_date: true
classes: wide
excerpt: ""
categories:
  - computer_architecture
---

## 4.1. 레지스터 전송 언어(Register Transfer Language)
레지스터에 저장된 데이터를 가지고 실행되는 동작을 마이크로 연산이라고 한다. 
이것은 하나의 클럭 펄스 동안에 실행되는 기본 동작이다. 

마이크로 연산을 간단하고 명료하게 표현하기 위해 기호를 사용하는 레지스터 전송 언어가 사용된다. 

## 4.2. 레지스터 전송(Register Transfer)
레지스터는 그 기능을 나타내기 위해 대문자로 표시된다. 
그리고 레지스터의 각 플립플롭은 0부터 번호가 매겨진다. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-17-computer_architecture_4/1.png' }}" alt=""> 
</figure> 

```console
레지스터간 전송의 예

R2 <- R1	// R1 레지스터의 비트를 R2로 병렬 로드
P: R2 <- R1	// 제어 변수 P가 1일 때 로드
```

아래 그림, 시간 t에서 클럭 펄스의 상승 모서리(rising edge)에서 제어 변수 P가 1이 된다. 
그리고 t + 1에서 로드가 수행되고, P는 0이 된다. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-17-computer_architecture_4/2.png' }}" alt=""> 
</figure> 

## 4.3. 버스와 메모리 전송(Bus and Memory Transfers)
많은 레지스터들이 정보 전송을 위해 각각의 전송 라인을 가지기는 어렵다. 
따라서 공통의 버스를 사용하며, 한 번에 하나의 전송만이 이루어지도록 제어 신호를 사용하여 전송될 레지스터를 선택한다. 

버스 시스템은 멀티플렉서 혹은 3-상태 버스 버퍼(Three-State Bus Buffers)를 사용하여 구성할 수 있다.  

```console
메모리 읽기/쓰기의 예

Read: DR <- M[AR]	// 주소 레지스터의 값을 읽어 메모리에 접근 후 데이터 레지스터에 전송
Write: M[AR] <- R1	// 메모리에 쓰기
```

## 4.4. 산술 마이크로 연산(Arithmetic Microoperations)
산술 마이크로 연산에는 덧셈, 뺄셈, 1의 보수, 2의 보수 등이 있다. 
그리고 산술 회로(Arithmetic Circuit)에 의해 수행된다. 

```console
산술 마이크로 연산의 예

R3 <- R1 + R2	
R1 <- R1 - 1	
```

## 4.5. 논리 마이크로 연산(Logic Microoperations)
논리 마이크로 연산에는 AND, OR, XOR 등이 있으며 비트별로 수행된다. 
이는 논리 회로(Logic Circuit)에서 수행된다. 

```console
논리 마이크로 연산의 예

F <- A ∨ B	// OR
F <- A ⊕ B	// XOR
```

## 4.6. 시프트 마이크로 연산(Shift Microoperations)
시프트 마이크로 연산에는 논리 시프트, 순환 시프트, 산술 시프트 연산이 있다.  
논리 시프트는 시프트 입력으로 0이 들어오며, 순환 시프트는 원래 비트가 순환되어 입력으로 들어온다.  
산술 시프트는 부호 비트는 시프트하지 않고, 나머지 비트만 시프트한다.  
조합 회로 시프터(combinational circuit shifter)가 사용된다. 

```console
시프트 마이크로 연산의 예

R <- shl R	// 왼쪽 논리 시프트
R <- cir R	// 오른쪽 순환 시프트
```

## 4.7. 산술 논리 시프트 장치(Arithmetic Logic Shift Unit, ALU)
컴퓨터는 각 마이크로 연산마다 독립된 레지스터를 사용하는 대신, ALU라고 하는 공용 연산 장치에 연결된 레지스터 그룹을 사용한다. 
이 경우 지정된 레지스터의 내용이 ALU의 입력으로 들어간다. 
그리고 앞의 연산들 중 하나가 수행되고 결과가 목적지 레지스터로 전송된다. 

