---
title: "데이터 베이스 요약 - 6. 물리적 데이터베이스 설계"
last_modified_at: 2021-03-10
show_date: true
classes: wide
excerpt: ""
toc: true
toc_sticky: true
categories:
  - database
---

# 6. 물리적 데이터베이스 설계
논리적인 설계의 데이터 구조를 보조 기억 장치상의 화일(물리적인 데이터 모델)로 사상함.  
예상 빈도를 포함하여 데이터베이스 질의와 트랜잭션들을 분석함.  
데이터에 대한 효율적인 접근을 제공하기 위하여 저장 구조와 접근 방법들을 다룸.  
특정 DBMS의 특성을 고려하여 진행됨.  
질의를 효율적으로 지원하기 위해서 인덱스 구조를 적절히 사용함.  

## 6.1. 보조 기억 장치
데이터를 검색하기 위해서 DBMS는 디스크상의 데이터베이스로부터 데이터를 포함하고 있는 블록을 읽어서 주기억 장치로 가져옴. 
데이터가 변경된 경우에는 블록들을 디스크에 다시 기록함. 

블록 크기는 512바이트부터 수 킬로바이트까지 다양함. 
전형적인 블록 크기는 4,096바이트. 

각 화일은 고정된 크기의 블록들로 나누어져서 저장됨. 
자기 디스크는 데이터베이스를 장기간 보관하는 주된 보조 기억 장치. 

---

자기 디스크는 자기 물질로 만들어진 여러 개의 판으로 이루어짐. 
판의 각 면마다 디스크 헤드가 있음. 

각 판은 트랙과 섹터로 구분됨. 
정보는 디스크 표면 상의 동심원(트랙)을 따라 저장됨. 
여러 개의 디스크 면 중에서 같은 지름을 갖는 트랙들을 실린더라고 부름. 

블록은 한 개 이상의 섹터들로 이루어짐. 
디스크에서 임의의 블록을 읽어오거나 기록하는데 걸리는 시간은 탐구 시간(seek time), 회전 지연 시간(rotational delay), 전송 시간(transfer time)의 합. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-09-database_system_6/1.png' }}" alt=""> 
</figure> 

## 6.2. 버퍼 관리와 운영체제
디스크 입출력은 컴퓨터 시스템에서 가장 속도가 느린 작업이므로, 입출력 횟수를 줄이는 것이 DBMS의 성능을 향상하는데 매우 중요. 
가능하면 많은 블록들을 주기억 장치에 유지하거나, 자주 참조되는 블록들을 주기억 장치에 유지하면 블록 전송 횟수를 줄일 수 있음. 

버퍼는 디스크 블록들을 저장하는 데 사용되는 주기억 장치 공간. 
버퍼 관리자는 운영체제의 구성요소로서, 주기억 장치 내에서 버퍼 공간을 할당하고 관리하는 일을 맡음. 

운영체제에서 버퍼 관리를 위해 흔히 사용되는 LRU 알고리즘은 데이터베이스를 위해 항상 우수한 성능을 보이지는 않음. 

## 6.3. 디스크 상에서 화일의 레코드 배치
릴레이션의 애트리뷰트는 고정 길이 또는 가변 길이의 필드로 표현됨. 
연관된 필드들이 레코드가 됨. 
릴레이션을 구성하는 레코드들의 모임이 화일이라고 부르는 블록들의 모임에 저장됨. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-09-database_system_6/2.png' }}" alt=""> 
</figure> 

한 화일에 속하는 블록들이 반드시 인접해 있을 필요는 없음. 
하지만, 인접한 블록들을 읽는 경우에는 탐구 시간과 회전 지연 시간이 없으므로, 입출력 속도가 빠름. 
블록들이 인접하도록 화일의 블록들을 재조직할 수 있음. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-09-database_system_6/3.png' }}" alt=""> 
</figure> 

---

BLOB(Binary Large Object)은 이미지(GIF, JPG), 동영상(MPEG) 등 대규모 크기의 데이터를 저장하는데 사용됨. 

---

채우기 인수(fill factor)는 각 블록에 레코드를 채우는 공간의 비율. 
레코드가 삽입될 때 기존의 레코드들을 이동하는 가능성을 줄이기 위해 사용됨. 
추가 연산이 많은 경우, 적은 채우기 인수를 사용하면 성능이 올라갈 수 있음

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-09-database_system_6/4.png' }}" alt=""> 
</figure> 

---

화일 내의 클러스터링(intra-file clustering)은 
한 화일 내에서 함께 검색될 가능성이 높은 레코드들을 디스크상에서 물리적으로 가까운 곳에 모아두는 것. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-09-database_system_6/5.png' }}" alt=""> 
</figure> 

화일 간의 클러스터링(inter-file clustering)은 
논리적으로 연관되어 함께 검색될 가능성이 높은 두 개 이상의 화일에 속한 레코드들을 디스크상에서 물리적으로 가까운 곳에 저장하는 것. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-09-database_system_6/6.png' }}" alt=""> 
</figure> 

## 6.4. 화일 조직
(1) 히프 파일(heap file; 비순서화일)
일반적으로 레코드들이 삽입된 순서대로 화일에 저장됨. 
- 삽입: 새로운 레코드는 화일의 가장 끝에 추가됨 
- 검색: 레코드를 찾기 위해서는 모든 레코드들을 순차적으로 탐색해야 함 
- 삭제: 레코드를 찾은 후에 삭제함. 삭제된 레코드가 차지하던 공간을 재사용하지 않음 
좋은 성능을 유지하기 위해서 히프 화일을 주기적으로 재조직할 필요가 있음.

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-09-database_system_6/7.png' }}" alt=""> 
</figure> 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-09-database_system_6/8.png' }}" alt=""> 
</figure> 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-09-database_system_6/9.png' }}" alt=""> 
</figure> 

---

(2) 순차 화일(sequential file)
레코드들이 하나 이상의 필드 값에 따라 순서대로 저장된 화일. 
일반적으로 레코드의 탐색 키(search key) 값의 순서에 따라 저장됨. 
탐색 키는 순차 화일을 정렬하는데 사용되는 필드. 
- 삽입: 삽입하려는 레코드의 순서를 고려해야 하기 때문에 시간이 많이 걸림 
- 삭제: 삭제된 레코드가 사용하던 공간을 빈 공간으로 남기기 때문에, 주기적으로 순차 화일을 재조직해야 함 
기본 인덱스가 순차 화일에 정의되지 않는 한 순차 화일은 데이터베이스 응용을 위해 거의 사용되지 않음. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-09-database_system_6/10.png' }}" alt=""> 
</figure> 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-09-database_system_6/11.png' }}" alt=""> 
</figure> 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-09-database_system_6/12.png' }}" alt=""> 
</figure> 

이 외에 인덱스된 순차 화일(indexed sequential file), 직접 화일(hash file) 등이 있음. 

## 6.5. 단일 단계 인덱스
인덱스란 탐색키 를 가진 레코드의 빠른 검색을 위해 <탐색키, 레코드 포인터>의 쌍을 관리하는 구조. 

인덱스된 순차 화일은 인덱스를 통해서 임의의 레코드를 접근할 수 있는 화일. 

단일 단계 인덱스의 각 엔트리는 <탐색키,레코드에대한포인터>. 
엔트리들은 탐색 키 값의 오름차순으로 정렬됨.

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-09-database_system_6/13.png' }}" alt=""> 
</figure> 

인덱스는 데이터 화일과는 별도의 화일에 저장됨. 
인덱스의 크기는 데이터 화일의 크기에 비해 훨씬 작음. 
하나의 화일에 여러 개의 인덱스들을 정의할 수 있음. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-09-database_system_6/14.png' }}" alt=""> 
</figure> 

인덱스가 정의된 필드를 탐색 키라고부름. 
키를 구성하는 애트리뷰트뿐만 아니라 어떤 애트리뷰트도 탐색 키로 사용될 수 있음. 

탐색 키의 값들은 후보 키처럼 각 투플마다 반드시 고유하지는 않음. 
인덱스의 엔트리들은 탐색 키 값의 오름차순으로 저장되어 있으므로 이진탐색을 이용할 수 있음. 

---

탐색 키가 데이터 화일의 기본 키인 인덱스를 기본 인덱스(primary index)라고 부름. 
기본 인덱스는 기본 키의 값에 따라 정렬된 데이터 화일에 대해 정의됨. 

기본 인덱스는 흔히 희소 인덱스로 유지할 수 있음. 
각 릴레이션마다 최대 한 개의 기본 인덱스를 가질 수 있음.

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-09-database_system_6/15.png' }}" alt=""> 
</figure> 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-09-database_system_6/16.png' }}" alt=""> 
</figure> 

---

클러스터링 인덱스(clustering index)은 탐색 키 값에 따라	정렬된 데이터 화일에 대해 정의됨. 
각각의 상이한 키 값마다 하나의 인덱스 엔트리가 인덱스에 포함됨. 

범위 질의에 유용. 
범위의 시작 값에 해당하는 인덱스 엔트리를 먼저 찾고, 
범위에 속하는 인덱스 엔트리들을 따라가면서 레코드들을 검색하면 디스크에서 읽어오는 블록 수가 최소화됨. 
어떤 인덱스 엔트리에서 참조되는 데이터 블록을 읽어오면 그 데이터 블록에 들어있는 대부분의 레코드들은 범위를 만족함. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-09-database_system_6/17.png' }}" alt=""> 
</figure> 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-09-database_system_6/18.png' }}" alt=""> 
</figure> 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-09-database_system_6/19.png' }}" alt=""> 
</figure> 

---

한 화일은 한 가지 필드들의 조합에 대해서만 정렬될 수 있음(보통 기본 키). 

보조 인덱스(secondary index)는 탐색 키 값에 따라 정렬되지 않은 데이터 화일에 대해 정의됨. 
보조 인덱스는 일반적으로 밀집 인덱스이므로, 레코드들을 접근할 때 기본 인덱스를 통하는 경우보다 디스크 접근 횟수가 증가할 수 있음. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-09-database_system_6/20.png' }}" alt=""> 
</figure> 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-09-database_system_6/21.png' }}" alt=""> 
</figure> 

---

희소 인덱스는 데이터 블록마다 한 개의 엔트리를 갖고, 
밀집 인덱스는 레코드마다 한 개의 엔트리를 가짐. 

레코드의 길이가 블록 크기 보다 작은 일반적인 경우에, 
희소 인덱스의 엔트리 수가 밀집 인덱스의 엔트리 수보다 적음. 

또한, 희소 인덱스는 일반적으로 밀집 인덱스에 비해 인덱스 단계 수가 1정도 적으므로 인덱스 탐색시 디스크 접근 수가 1만큼 적을 수 있음. 

희소 인덱스는 밀집 인덱스에 비해 모든 갱신과 대부분의 질의에 대해 더 효율적. 
그러나, 질의에서 인덱스가 정의된 애트리뷰트만 검색(예: COUNT 질의)하는 경우에는, 
데이터 화일을 접근할 필요 없이 인덱스만 접근해서 질의를 수행할 수 있으므로 밀집 인덱스가 희소 인덱스보다 유리. 

한 화일은 한 개의 희소 인덱스와 다수의 밀집 인덱스를 가질 수 있음. 

---

클러스터링 인덱스와 보조 인덱스의 비교.

클러스터링 인덱스는 희소 인덱스일 경우가 많으며 범위 질의 등에 좋음. 
보조 인덱스는 밀집 인덱스이므로 일부 질의에 대해서는 화일을 접근할 필요 없이 처리할 수 있음. 

## 6.6. 다단계 인덱스
인덱스 자체가 클 경우에는 인덱스를 탐색하는 시간도 오래 걸릴 수 있음. 
인덱스 엔트리를 탐색하는 시간을 줄이기 위해서 단일 단계 인덱스를 순서 화일로 간주하고, 단일 단계 인덱스에 대해서 다시 인덱스를 정의할 수 있음. 

다단계 인덱스는 가장 상위 단계의 모든 인덱스 엔트리들이 한 블록에 들어갈 수 있을 때까지 이런 과정을 반복함. 
가장 상위 단계 인덱스를 마스터 인덱스(master index)라고 부름. 

마스터 인덱스는 한 블록으로 이루어지기 때문에 주기억 장치에 상주할 수 있음. 
대부분의 다단계 인덱스는 B<sup>+</sup>-트리를 사용.

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-09-database_system_6/22.png' }}" alt=""> 
</figure> 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-09-database_system_6/23.png' }}" alt=""> 
</figure> 

---

CREATE TABLE문에서 PRIMARY KEY절로 명시한 애트리뷰트에 대해서는 DBMS가 자동으로 기본 인덱스를 생성. 
UNIQUE로 명시한 애트리뷰트에 대해서는 DBMS가 자동으로 보조 인덱스를 생성. 

SQL2는 인덱스 정의 및 제거에 관한 표준 SQL문을 제공하지 않음. 
따라서, 다른 애트리뷰트에 인덱스를 정의하기 위해서는 DBMS마다 구문이 다른 CREATE INDEX문을 사용해야 함. 

```console
CREATE INDEX EmpIndex ON EMPLOYEE (DNO, SALARY);

// 이 인덱스는 아래의 질의들에 활용될 수 있음
SELECT * 
FROM EMPLOYEE 
WHERE DNO=3 AND SALARY = 4000000;

SELECT * 
FROM EMPLOYEE 
WHERE DNO>=2 AND DNO <=3 AND SALARY >= 3000000 
AND SALARY <= 4000000;

SELECT * 
FROM EMPLOYEE 
WHERE DNO = 2;

// 위 인덱스는 아래의 질의에는 활용될 수 없음
SELECT *
FROM EMPLOYEE
WHERE SALARY >= 3000000
AND SALARY <= 4000000;
```

---

인덱스는 검색 속도를 향상시키지만, 인덱스를 저장하기 위한 공간이 필요하고 삽입, 삭제, 수정 연산의 속도는 저하시킴. 
소수의 레코드들을 수정하거나 삭제하는 연산의 속도는 향상됨. 

릴레이션이 크고, 질의에서 릴레이션의 투플들 중에 일부(예: 2%~4%)를 검색하고, 
WHERE절이 잘 표현되었을 때 특히 성능에 도움이됨. 

## 6.7. 인덱스 선정 지침과 데이터베이스 튜닝
중요한 질의들의 수행 빈도, 중요한 갱신들의 수행 빈도들을 고려하여 인덱스를 선정함. 

1. 외래 키는 인덱스를 정의할 후보 
2. 애트리뷰트에 있는 상이한 값들의 개수가 거의 전체 레코드 수와 비슷하고, 애트리뷰트가 동등 조건에 사용된다면 비 클러스터링 인덱스를 생성하는 것이 좋음 
3. 릴레이션이 크고, 대부분의 질의에서 검색하는 투플이 2%~4%인 경우에는 인덱스를 생성 
4. 자주 갱신되는 애트리뷰트에는 인덱스를 정의하지 않는 것이 좋음 
5. 갱신이 빈번한 릴레이션에는 인덱스를 많이 만드는 것을 피함 
6. 후보 키는 기본 키가 갖는 모든 특성을 갖기에, 인덱스를 생성할 후보 
7. 인덱스는 화일의 레코드들을 충분히 분할할 수 있어야 함 
8. 정수형 애트리뷰트에 인덱스를 생성 
9. VARCHAR 애트리뷰트에는 인덱스를 만들지 않음 
10. 작은 화일에는 인덱스를 만들 필요가 없음 

---

언제 인덱스가 사용되지 않는가? 
1. 시스템 카탈로그가 오래 전의 데이터베이스 상태를 나타냄 
2. DBMS의 질의 최적화 모듈이 릴레이션의 크기가 작아서 인덱스가 도움이 되지 않는다고 판단함 
3. 인덱스가 정의된 애트리뷰트에 산술 연산자가 사용됨 
4. DBMS가 제공하는 내장 함수가 집단 함수 대신에 사용됨 
5. 널 값에 대해서는 일반적으로 인덱스가 사용되지 않음 

```console
// 내장 함수 사용
SELECT * 
FROM EMPLOYEE 
WHERE SUBSTR(EMPNAME, 1, 1) = '김';

// NULL 값
SELECT * 
FROM EMPLOYEE 
WHERE MANAGER IS NULL;
```

---

질의 튜닝을 위한 추가 지침
1. DISTINCT절의 사용을 최소화 
2. GROUP BY절과 HAVING절의 사용을 최소화 
3. 임시 릴레이션의 사용을 피함 
4. SELECT * 대신에 SELECT절에 애트리뷰트 이름들을 구체적으로 명시 

