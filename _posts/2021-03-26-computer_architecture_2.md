---
title: "컴퓨터 구조 요약 - 2. 디지털 부속품"
last_modified_at: 2021-03-26
show_date: true
classes: wide
excerpt: ""
categories:
  - computer_architecture
---

## 2.1. 집적 회로(Integrated Circuits)
디지털 회로는 집적 회로들로 구성된다. 
집적 회로는 디지털 게이트를 구성하는 전자 부품들을 포함하는 작은 실리콘 반도체 결정(semiconductor crystal; chip 이라 불림)이다. 
CMOS, TTL 등이 있다. 

## 2.2. 디코더(Decoders)
n비트 입력, m <= 2<sup>n</sup>개 출력하는 조합 회로. 

### 2.2.1. NAND 게이트 디코더(NANA Gate Decoder)
보수화된 출력을 위해 디코더를 NAND로 구성한 것.

### 2.2.2. 인코더(Encoders)
디코더의 반대되는 동작을 수행한다.  
2<sup>n</sup>개 입력 값, n비트 출력.

## 2.3. 멀티플렉서(Multiplexers)
2<sup>n</sup>개 입력 값, n개의 선택 비트 입력, 1개의 출력 값을 가지는 조합 회로. 

## 2.4. 레지스터(Registers)
n비트 정보를 저장하기 위한 n개의 플립-플롭과 데이터 처리를 위한 조합 회로로 구성됨.  

## 2.5. 시프트 레지스터(Shift Registers)
레지스터에 저장되어 있는 이진 정보를 단/양방향으로 이동시킬 수 있음. 
레지스터의 각 플립-플롭들은 각각의 입력과 출력이 연쇄적으로 연결되어 있음. 

## 2.6. 이진 카운터(Binary Counters)
카운트가 1씩 올라가는 것과 같이 미리 정해진 순서대로 상태가 변하는 레지스터. 

## 2.7. 메모리 장치(Memory Unit)
메모리 장치는 저장 요소들의 집합과 저장 장치에 정보를 입출력하는데 필요한 회로로 구성되어 있다. 
메모리는 워드(words)라 불리는 비트의 그룹으로 이진 정보를 저장한다. 

8비트의 그룹을 바이트(byte)라 하며, 대부분의 컴퓨터 메모리는 8의 배수 크기의 워드를 사용한다. 

### 2.7.1. 임의접근 메모리(Random-Access Memory, RAM)
RAM에서는 워드의 물리적인 위치에 관계없이 접근 시간이 동일하다. 
입/출력 라인(input/output lines), 메모리내 주소 지정 라인(address selection lines), 읽기 혹은 쓰기를 나타내는 두 개의 제어 라인(control lines)이 있다. 

### 2.7.2. 읽기전용 메모리(Read-Only Memory, ROM)
읽기 동작만 수행하는 메모리다. 
RAM과 달리 저장 요소가 없으며, 디코더와 OR 게이트 집합으로만 구성될 수 있다. 

ROM은 고정된 프로그램이나 변경되지 않는 상수 등을 저장하는 데 사용된다. 

### 2.7.3. ROM의 종류(Types of ROMs)
PROM, EPROM, EEPROM 등이 있다. 

---

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-26-computer_architecture_2/1.png' }}" alt=""> 
	<figcaption>TTL7400,  하나의 칩을 같은 게이트로 구성하는 경우가 많다</figcaption>
</figure> 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-26-computer_architecture_2/2.png' }}" alt=""> 
	<figcaption>동일 입출력 회로를 NAND로만 구성하면, 사용되는 게이트의 종류가 줄어들어 더 경제적이다</figcaption>
</figure> 

