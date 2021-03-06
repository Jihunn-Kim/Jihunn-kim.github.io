---
title: "데이터 베이스 요약 - 4. 관계 대수와 SQL"
last_modified_at: 2021-03-07
show_date: true
classes: wide
excerpt: ""
categories:
  - database
---

# 4. 관계 대수와 SQL
관계 대수(relational algebra)는 어떻게 질의를 수행할 것인가를 명시하는 절차적 인어이다. 
관계 DBMS에서 널리 사용되는 SQL의 이론적인 기초이며, SQL을 구현하고 최적화하기 위해 DBMS의 내부 언어로서도 사용됨. 

상용 관계 DBMS들의 사실상의 표준 질의어는 SQL임. 
사용자는 SQL을 사용하여 관계 데이터베이스에 릴레이션을 정의하고, 
정보를 검색하고, 갱신하며, 무결성 제약조건들을 명시할 수 있음. 

## 4.1. 관계 대수
관계 대수는 기존의 릴레이션들로부터 새로운 릴레이션을 생성함. 
산술 연산자와 유사하게 단일 릴레이션이나 두 개의 릴레이션을 입력으로 받아 하나의 결과 릴레이션을 생성함. 
결과 릴레이션은 또 다른 관계 연산자의 입력으로 사용될 수 있음. 

<figure style="width: 400px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-06-database_system_4/1.png' }}" alt=""> 
</figure> 

<figure style="width: 400px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-06-database_system_4/2.png' }}" alt=""> 
</figure> 

집합 연산자의 입력으로 사용되는 두 개의 릴레이션은 합집합 호환(union compatible)이어야 함. 
두 릴레이션 R1(A1, A2, ..., An)과 R2(B1, B2, ..., Bm)이 합집합 호환일 필요 충분 조건은 n=m이고, 모든 1<=i<=n에 대해 domain(Ai)=domain(Bi). 

테이블에서 실렉션, 프로젝션, 합집합, 차집합, 카티션 곱은 관계 대수의 필수적인 연산자이다. 
다른 관계 연산자들은 필수적인 관계 연산자를 두 개 이상 조합하여 표현할 수 있으나 편의를 위해 존재한다. 
임의의 질의어가 적어도 필수적인 관계 대수 연산자들만큼의 표현력을 갖고 있으면 관계적으로 완전(relationally complete)하다고 말함. 

---

(1) 실렉션 연산자 (형식: σ<sub>실렉션 조건</sub>(릴레이션)): 
한 릴레이션에서 실렉션 조건(selection condition)을 만족하는 투플들의 부분 집합을 생성함. 
실렉션 조건은 릴레이션의 임의의 애트리뷰트와 상수, =, >=, < 등의 비교 연산자, AND, OR, NOT 등의 부울 연산자를 포함할 수 있음. 

<figure style="width: 400px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-06-database_system_4/3.png' }}" alt=""> 
</figure> 

---

(2) 프로젝션 연산자 (형식: π<sub>애트리뷰트 리스트</sub>(릴레이션)): 
한 릴레이션의 애트리뷰트들의 부분 집합을 구함. 
결과로 생성되는 릴레이션은 <애트리뷰트 리스트>에 명시된 애트리뷰트들만 가짐. 

<figure style="width: 400px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-06-database_system_4/4.png' }}" alt=""> 
</figure> 

---

(3) 합집합 연산자 (형식: 릴레이션1 ∪ 릴레이션2): R ∪ S는 R 또는 S에 있거나 R과 S 모두에 속한 투플들로 이루어진 릴레이션을 생성함. 
결과 릴레이션에서 중복된 투플들은 제외됨. 
결과 릴레이션의 애트리뷰트 이름들은 R의 애트리뷰트들의 이름과 같거나 S의 애트리뷰트들의 이름과 같음. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-06-database_system_4/5.png' }}" alt=""> 
</figure> 

---

(4) 교집합 연산자 (형식: 릴레이션1 ∩ 릴레이션2): R ∩ S는 R과 S 모두에 속한 투플들로 이루어진 릴레이션을 생성함. 
결과 릴레이션의 애트리뷰트 이름들은 R의 애트리뷰트들의 이름과 같거나 S의 애트리뷰트들의 이름과 같음. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-06-database_system_4/6.png' }}" alt=""> 
</figure> 

---

(5) 차집합 연산자 (형식: 릴레이션1 - 릴레이션2): R - S는 R에는 속하지만 S에는 속하지 않은 투플들로 이루어진 릴레이션을 생성함. 
결과 릴레이션의 애트리뷰트 이름들은 R의 애트리뷰트들의 이름과 같거나 S의 애트리뷰트들의 이름과 같음. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-06-database_system_4/7.png' }}" alt=""> 
</figure> 

---

(6) 카티션 곱 연산자 (형식: R × S): 카디날리티가 i인 릴레이션 R(A1, ..., An)과 카디날리티가 j인 릴레이션 S(B1, ..., Bm)의 카티션 곱 R × S는 차수가 n+m이고, 카디날리티가 
i*j이고, 애트리뷰트가 (A1, A2, ..., An, B1, B2, ..., Bm)이며, R과 S의 투플들의 모든 가능한 조합으로 이루어진 릴레이션이다. 
카티션 곱의 결과 릴레이션의 크기가 매우 클 수 있다. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-06-database_system_4/8.png' }}" alt=""> 
</figure> 

---

(7) 세타(θ) 조인 (형식: R ⋈<sub>조인 조건</sub> S): 두 릴레이션 R(A1, ..., An)과 S(B1, ..., Bm)의 세타 조인 결과는 차수가 n+m이고, 애트리뷰트가 (A1, ..., An, B1, ..., Bn)이며, 
조인 조건을 만족하는 투플들로 이루어진 릴레이션이다. 

조인 조건은 (R의 애트리뷰트 θ S의 애트리뷰트)의 형태로 주어지며 θ는 =, < 등이 가능.

(8) 동등 조인은 세타 조인 중에서 비교 연산자가 =인 조인.

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-06-database_system_4/9.png' }}" alt=""> 
</figure> 

---

(9) 자연 조인 (형식: R ⋈ S): 두 릴레이션의 공통된 애트리뷰트에 대해 동등 조인을 수행하고, 
동등 조인의 결과 릴레이션에 있는 두 개의 조인 애트리뷰트 중 하나를 제외한 조인. 

관계 데이터베이스에서 대부분의 질의는 실렉션, 프로젝션, 자연 조인으로 표현 가능.

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-06-database_system_4/10.png' }}" alt=""> 
</figure> 

---

(10) 디비전 연산자 (형식: R ÷ S): 차수가 n+m인 릴레이션 R(A1, ..., An, B1, ..., Bm)과 
차수가 m인 릴레이션 S(B1, ..., Bm)의 디비전 R ÷ S는 차수가 n이고, 
S에 속하는 모든 투플 u에 대하여 투플 tu(투플 t와 투플 u을 결합한 것)가 R에 존재하는 투플 t들의 집합. 

릴레이션 S의 모든(ALL) 투플 값과 쌍을 이루는 릴레이션 R의 A1, ..., An 값. 

"모든 …에 대해 ~하는" 형태의 질의에 사용될 수 있음. 

SQL로 표현할 때 동치를 활용: ~하지 않는 …가 없다. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-06-database_system_4/11.png' }}" alt=""> 
</figure> 

---

추가된 관계 연산자 (1) 집단(aggregation) 함수: AVG, SUM, MIN, MAX, COUNT 등.

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-06-database_system_4/12.png' }}" alt=""> 
</figure> 

---

추가된 관계 연산자 (2) 그룹화: 각 그룹에 대해 집단 함수를 적용. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-06-database_system_4/13.png' }}" alt=""> 
</figure> 

---

추가된 관계 연산자 (3) 외부조인: 상대 릴레이션에서 대응되는 투플을 갖지 못하는 투플이나, 
조인 애트리뷰트에 널값이 들어 있는 투플들을 다루기 위해서 조인 연산을 확장한 조인. 

두 릴레이션에서 대응되는 투플들을 결합하면서, 대응되는 투플을 갖지 않는 투플과 조인 애트리뷰트에 널값을 갖는 투플도 결과에 포함시킴. 

---

(3-1) 왼쪽 외부 조인 (형식: R ⟕ S): 릴레이션 R과 S의 왼쪽 외부 조인 연산은 R의 모든 투플들을 결과에 포함시키고, 
만일 릴레이션 S에 관련된 투플이 없으면 결과 릴레이션에서 릴레이션 S의 애트리뷰트들은 널값으로 채움. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-06-database_system_4/14.png' }}" alt=""> 
</figure> 

---

(3-2) 오른쪽 외부 조인 (형식: R ⟖ S): 릴레이션 R와 S의 오른쪽 외부 조인 연산은 S의 모든 투플들을 결과에 포함시키고, 
만일 릴레이션 R에 관련된 투플이 없으면 결과 릴레이션에서 릴레이션 R의 애트리뷰트들은 널값으로 채움. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-06-database_system_4/15.png' }}" alt=""> 
</figure> 

---

(3-3) 완전 외부 조인 (형식: R ⟗ S): 릴레이션 R와 S의 완전 외부 조인 연산은 R과 S의 모든 투플들을 결과에 포함시키고, 
만일 상대 릴레이션에 관련된 투플이 없으면 결과 릴레이션에서 상대 릴레이션의 애트리뷰트들은 널값으로 채움. 
> R ⟗ S = (R ⟕ S) ∪ (R ⟖ S)

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-06-database_system_4/16.png' }}" alt=""> 
</figure> 

## 4.2. SQL 개요
SQL은 비절차적 언어이므로 사용자는 자신이 원하는 바(what)만 명시하며, 원하는 것을 처리하는 방법(how)은 명시할 수 없음. 
관계 DBMS는 사용자가 입력한 SQL문을 번역하여 사용자가 요구한 데이터를 찾는데 필요한 모든 과정을 담당함. 
대화식 SQL(interactive SQL), 내포된 SQL(embedded SQL) 두 가지 인터페이스가 있음. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-06-database_system_4/17.png' }}" alt=""> 
</figure> 

## 4.3. 데이터 정의어와 무결성 제약조건

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-06-database_system_4/18.png' }}" alt=""> 
</figure> 

SQL2에서는 동일한 데이터베이스 응용에 속하는 릴레이션, 도메인, 제약조건, 뷰, 권한 등을 그룹화하기 위해서 스키마 개념도 지원. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-06-database_system_4/19.png' }}" alt=""> 
</figure> 

```console
// 예시

// 릴레이션 정의 및 제약 조건 등
CREATE TABLE EMPLOYEE (
	EMPNO 	NUMBER 	NOT NULL, 
	EMPNAME CHAR(10) UNIQUE,
	TITLE 	CHAR(10) DEFAULT '사원',
	SALARY	NUMBER	CHECK (SALARY < 60000),	// 제약 조건
	MANAGER CHAR(10),	
	DNO	NUMBER,
	PRIMARY KEY (EMPNO),
	FOREIGN KEY (MANAGER) REFERENCES EMPLOYEE (EMPNO) ON DELETE CASCAED // 외래 키, 참조 무결성 제약 조건
);

// 릴레이션 제거
DROP TABLE EMPLOYEE;

// 테이블 변경
ALTER TABLE EMPLOYEE ADD PHONE CHAR(13);

// 인덱스 생성
CREATE INDEX EMPDNO_IDX ON EMPLOYEE (DNO);

// 무결성 제약 조건의 추가 및 삭제
ALTER TABLE STUDENT ADD CONSTRAINT STUDENT_PK PRIMARY KEY (STNO);

ALTER TABLE STUDENT DROP CONSTRAINT STUDENT_PK;
```

## 4.4. SELECT 문
