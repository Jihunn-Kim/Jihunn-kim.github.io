---
title: "컴퓨터 구조 요약 - 6. 기본 컴퓨터 프로그래밍"
last_modified_at: 2021-03-31
show_date: true
classes: wide
excerpt: ""
categories:
  - computer_architecture
---

## 6.1. 개요(Introduction)
컴퓨터 시스템은 하드웨어와 소프트웨어로 구성되는데, 하드웨어란 컴퓨터와 관련된 장치들을 의미하고, 소프트웨어는 컴퓨터를 위해 쓰여진 프로그램들을 말한다. 
컴퓨터 내부의 기계어는 이진 형태로 되어 있으므로 사람이 이해하기가 힘들기 때문에, 영숫자 형태의 기호로 프로그램을 작성한다. 
따라서, 기호 프로그램을 하드웨어가 인지할 수 있는 이진 프로그램으로 옮길 필요가 있다. 

프로그램을 컴퓨터가 실행할 수 있는 이진 코드로 번역하는 번역 프로그램(translator program)은 기계 종속적 프로그램이다. 
특정한 컴퓨터의 하드웨어에 의해서 인지되어져야 하기 때문이다. 

## 6.2. 기계어(Machine Language)
프로그램에는 다음과 같은 종류가 있다. 
1. 이진 코드: 메모리상에 실제로 나타나는 형태의 명령어
2. 8/16 진수: 이진 코드를 그대로 8/16진수로 표현한 것
3. 기호 코드: 연산, 주소 부분 등에 기호를 사용. 각 기호 명령어는 어셈블러(assembler)라는 프로그램을 통해 하나의 이진 코드로 번역됨. 기호 프로그램은 어셈블리 언어라고도 불림. 
4. 고급 프로그래밍 언어: 포트란 등 문제 위주의 기호와 형식을 사용함. 각 문장은 컴파일러(compiler)라는 프로그램을 통해 여러 개의 이진 명령어로 번역됨.

<figure style="width: 500px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-18-computer_architecture_6/1.png' }}" alt=""> 
</figure> 

<figure style="width: 500px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-18-computer_architecture_6/2.png' }}" alt=""> 
</figure> 

<figure style="width: 500px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-18-computer_architecture_6/3.png' }}" alt=""> 
</figure> 

<figure style="width: 500px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-18-computer_architecture_6/4.png' }}" alt=""> 
</figure> 

<figure style="width: 500px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-18-computer_architecture_6/5.png' }}" alt=""> 
</figure> 

## 6.3. 어셈블리 언어(Assembly Language)
프로그램이 번역되기 위해서는 그 언어의 형식 규정을 잘 지켜야 한다. 
컴퓨터는 각각의 어셈블리 언어를 갖고 있으며, 이에 대한 규칙 등은 컴퓨터 제조업체로부터 출판되어 있다. 

### 6.3.1. 언어 규칙(Rules of Language)
어셈블리 언어의 각 줄은 필드(field)라 불리는 세 개의 부분으로 구성된다. 
1. 라벨 필드: 기호 주소를 나타내거나 빈칸
2. 명령어 필드: 기계/슈도 명령어 기술
3. 코멘트 필드: 주석/해설/빈칸

라벨 필드의 기호 주소는 콤마(,)로 분리된다. 
기호 주소는 영숫자들로 구성되며, 첫자는 문자이어야 한다. 

명령어 필드는 다음 중 하나를 기술한다.
1. 메모리 참조 명령어(MRI)
2. 레지스터 참조 또는 입출력 명령어(non-MRI)
3. 슈도 명령어

<figure style="width: 500px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-18-computer_architecture_6/6.png' }}" alt=""> 
</figure> 

## 6.4. 어셈블러(The Assembler)
어셈블러는 기호 프로그램을 읽어서 이진 프로그램으로 번역하는 프로그램이다. 
기호 프로그램을 소스 프로그램이라 하고, 이진 프로그램을 목적 프로그램이라 한다. 

### 6.4.1. 메모리내에서 기호 프로그램의 표현(Representation of Symbolic Program in Memory)
기호 프로그램의 영숫자는 ASCII 코드로 표현되어 메모리에 올라갈 수 있다.

<figure style="width: 500px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-18-computer_architecture_6/7.png' }}" alt=""> 
</figure> 

### 6.4.2. First Pass
two-pass 어셈블러는 기호 프로그램 전체를 두 번 스캔한다. 
firs pass 동안은 주소 기호와 이것에 해당하는 이진수 값의 관계를 나타내는 표를 작성한다. 
그리고 second pass 동안에는 이진수로의 번역이 실행된다. 

<figure style="width: 500px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-18-computer_architecture_6/8.png' }}" alt=""> 
</figure> 

<figure style="width: 500px" class="align-center">
 	<img src="{{ '/assets/img/2021-01-18-computer_architecture_6/9.png' }}" alt=""> 
</figure> 

### 6.4.3. Second Pass
기계 명령어는 second pass 동안에 table-lookup을 통해 번역된다. 
1. 슈도 명령어 테이블 - ORG, END ...
2. 메모리 참조 명령어 테이블 - LDA, BUN ...
3. 레지스터 참조 혹은 입출력 명령 테이블 - INP, CMA ...
4. 기호-주소 테이블 - first pass에서 생성

## 6.5 ~ 6.8
루프, 곱셈, 서브루틴, 입출력 등의 어셈블리 프로그램 예제

