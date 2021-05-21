---
title: "데이터 베이스 요약 - 9. 트랜잭션"
last_modified_at: 2021-03-12
show_date: true
classes: wide
excerpt: ""
toc: true
toc_sticky: true
categories:
  - database
---

## 9. 트랜잭션(transaction)
많은 사용자들이 동시에 데이터베이스의 서로 다른 부분 또는 동일한 부분을 접근하면서 데이터베이스를 사용함. 

---

동시성 제어(concurrency control) 
- 동시에 수행되는 트랜잭션들이 데이터베이스에 미치는 영향을, 순차적으로 수행하였을 때 데이터베이스에 미치는 영향과 같도록 함 
- 많은 사용자가 데이터베이스를 동시에 접근하도록 하면서 데이터베이스의 일관성을 유지함 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-11-database_system_9/2.png' }}" alt=""> 
</figure> 

위의 두 개의 UPDATE문은 둘 다 완전하게 수행되거나 한 UPDATE문도 수행되어서는 안되도록, 
즉 하나의 트랜잭션(단위)처럼 DBMS가 보장해야함. 

기본적으로 각각의 SQL문이 하나의 트랜잭션으로 취급됨. 
두 개 이상의 SQL문들을 하나의 트랜잭션으로 취급하려면 사용자가 이를 명시적으로 표시해야 함.

---

회복(recovery)
- 데이터베이스를 갱신하는 도중에 시스템이 고장나도 데이터베이스의 일관성을 유지함

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-11-database_system_9/1.png' }}" alt=""> 
</figure> 

DBMS가 추가로 정보를 유지하지 않는다면, 
DBMS가 재기동된 후에 어느 직원의 투플까지 수정되었는가를 알 수 없음 → 로그(log) 유지


## 9.1 트랜잭션 개요
트랜잭션의 특성(ACID 특성)

(1) 원자성(Atomicity): 트랜잭션의 모든 연산들이 완전히 수행되거나 전혀 수행되지 않음. 

DBMS의 회복 모듈은 시스템이 다운되는 경우에, 부분적으로 데이터베이스를 갱신한 트랜잭션의 영향을 취소함으로써 트랜잭션의 원자성을 보장함. 
완료된 트랜잭션이 갱신한 사항은 트랜잭션의 영향을 재수행함으로써 트랜잭션의 원자성을 보장함. 

(2) 일관성(Consistency): 트랜잭션이 수행되기 전에 데이터베이스가 일관된 상태를 가졌다면, 
트랜잭션이 수행된 후에 데이터베이스는 또 다른 일관된 상태를 가짐. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-11-database_system_9/3.png' }}" alt=""> 
</figure> 

(3) 고립성(Isolation): 트랜잭션이 데이터를 갱신하는 동안, 이 데이터를 다른 트랜잭션들이 접근하지 못하도록 해야 함. 

다수의 트랜잭션들이 동시에 수행되더라도 그 결과는 어떤 순서에 따라 트랜잭션들을 하나씩 차례대로 수행한 결과와 같아야 함. 
DBMS의 동시성 제어 모듈이 트랜잭션의 고립성을 보장함. 
DBMS는 응용들의 요구사항에 따라 다양한 고립수준(isolation level)을 제공함. 

(4) 지속성(Durability): 트랜잭션이 완료되면 이 트랜잭션이 갱신한 것은 그 후에 시스템에 고장이 발생하더라도 손실되지 않음. 

완료된 트랜잭션의 효과는 시스템이 고장난 경우에도 데이터베이스에 반영됨. 
DBMS의 회복 모듈은 시스템이 다운되는 경우에도 트랜잭션의 지속성을 보장함. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-11-database_system_9/4.png' }}" alt=""> 
</figure> 

---

트랜잭션의 완료(commit) 
- 트랜잭션에서 변경하려는 내용이 데이터베이스에 완전하게 반영됨 
- SQL 구문상으로 COMMIT WORK 

트랜잭션의 철회(abort) 
- 트랜잭션에서 변경하려는 내용이 데이터베이스에 일부만 반영된 경우에는 원자성을 보장하기 위해서, 트랜잭션이 갱신한 사항을 트랜잭션이 수행되기 전의 상태로 되돌림 
- SQL 구문상으로 ROLLBACK WORK 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-11-database_system_9/5.png' }}" alt=""> 
</figure> 

트랜잭션이 성공하지 못하는 원인
- 시스템(사이트) 고장(중앙 처리 장치, 주 기억 장치, 전원 공급 장치 등이 고장남) 
- 트랜잭션 고장은 트랜잭션이 수행되는 도중에 철회됨 
- 매체 고장(디스크 헤드, 디스크 콘트롤러 등이 고장 나서 보조 기억 장치의 전부 또는 일부 내용이 지워짐) 
- 통신 고장
- 자연적 재해

## 9.2. 동시성 제어
여러 사용자들이 동시에 동일한 테이블을 접근하기도 함. 
DBMS의 성능을 높이기 위해 여러 사용자의 질의나 프로그램들을 동시에 수행하는 것이 필수적. 

동시성 제어 기법은 여러 사용자들이 다수의 트랜잭션들을 동시에 수행하는 환경에서 부정확한 결과를 생성할 수 있는, 트랜잭션들 간의 간섭이 생기지 않도록 함. 

---

직렬 스케줄(serial schedule) 
- 여러 트랜잭션들의 집합을 한 번에 한 트랜잭션씩 차례대로 수행함 

비직렬 스케줄(non-serial schedule) 
- 여러 트랜잭션들을 동시에 수행함 

직렬가능(serializable) 
- 비직렬 스케줄의 결과가 어떤 직렬 스케줄의 수행 결과와 동등함 

---

데이터베이스 연산 
- Input(X) 연산은 데이터베이스 항목 X를 포함하고 있는 블록을 주기억 장치의 버퍼로 읽어들임 
- Output(X) 연산은 데이터베이스 항목 X를 포함하고 있는 블록을 디스크에 기록함 
- read_item(X) 연산은 주기억 장치 버퍼에서 데이터베이스 항목 X의 값을 프로그램 변수 X로 복사함 
- write_item(X) 연산은 프로그램 변수 X의 값을 주기억 장치 내의 데이터베이스 항목 X에 기록함 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-11-database_system_9/6.png' }}" alt=""> 
</figure> 

---

동시성 제어를 하지 않고 다수의 트랜잭션을 동시에 수행할 때 생길 수 있는 문제 
- 갱신 손실(lost update): 수행 중인 트랜잭션이 갱신한 내용을 다른 트랜잭션이 덮어 씀으로써 갱신이 무효가 되는 것 
- 오손 데이터 읽기(dirty read): 완료되지 않은 트랜잭션이 갱신한 데이터를 읽는 것 
- 반복할 수 없는 읽기(unrepeatable read): 한 트랜잭션이 동일한 데이터를 두 번 읽을 때 서로 다른 값을 읽는 것 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-11-database_system_9/7.png' }}" alt=""> 
</figure> 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-11-database_system_9/8.png' }}" alt=""> 
</figure> 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-11-database_system_9/9.png' }}" alt=""> 
</figure> 

---

데이터 항목을 로킹(locking)하는 개념은 동시에 수행되는 트랜잭션들의 동시성을 제어하기 위해서 사용되는 기법. 
로크(lock)는 데이터베이스 내의 각데이터 항목과 연관된 하나의 변수. 

트랜잭션이 수행을 시작하여 데이터 항목을 접근할 때마다 요청한 로크에 관한 정보는 로크 테이블(lock table) 등에 유지됨. 

트랜잭션에서 데이터 항목을 접근할 때 로크를 요청하고, 접근을 끝낸 후에 로크를 해제(unlock)함. 
- 갱신 목적 접근: 독점 로크(X-lock, eXclusive lock)를 요청함 
- 판독(읽기) 목적 접근: 공유 로크(S-lock, Shared lock)를요청함 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-11-database_system_9/10.png' }}" alt=""> 
</figure> 

---

2단계 로킹 프로토콜(2-phase locking protocol)
- 로크를 요청하는 것과 로크를 해제하는 것이 2단계로 이루어짐 
- 로크 확장 단계가 지난 후에 로크 수축 단계에 들어감 
- 일단 로크를 한 개라도 해제하면 로크 수축 단계에 들어감 

로크 확장 단계(1단계)에서는 트랜잭션이 데이터 항목에 대하여 새로운 로크를 요청할 수 있지만, 보유하고 있던 로크를 하나라도 해제할 수 없음 

로크 수축 단계(2단계)에서는 보유하고 있던 로크를 해제할 수 있지만, 새로운 로크를 요청할 수 없음 
로크를 조금씩 해제할 수도 있고, 트랜잭션이 완료 시점에 이르렀을 때 한꺼번에 모든 로크를 해제할 수 도있음. 
일반적으로 한꺼번에 해제하는 방식이 사용됨 

로크 포인트(lock point)는 한 트랜잭션에서 필요로 하는 모든 로크를 걸어놓은 시점.

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-11-database_system_9/11.png' }}" alt=""> 
</figure> 

---

2단계 로킹 프로토콜에서는 데드록(deadlock)이 발생할 수 있음. 
데드록은 두 개 이상의 트랜잭션들이 서로 상대방이 보유하고 있는 로크를 요청하면서 기다리고 있는 상태를 말함. 

데드록을 해결하기 위해서는 데드록을 방지하는 기법이나, 데드록을 탐지하고 희생자를 선정하여 데드록을 푸는 기법 등을 사용함. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-11-database_system_9/12.png' }}" alt=""> 
</figure> 

---

트랜잭션들이 소수의 투플들을 접근하는 데이터베이스 응용에서는, 투플 단위로 로크를 해도 로크 테이블을 다루는 시간이 오래 걸리지 않음. 
그러나, 트랜잭션들이 많은 투플을 접근할 때, 투플 단위로만 로크를 한다면 로크 테이블에서 로크 충돌을 검사하고, 로크 정보를 기록하는 시간이 오래 걸림. 

트랜잭션이 접근하는 투플의 수에 따라 로크를 하는 데이터 항목의 단위를 구분하는 것이 필요함. 
한 트랜잭션에서 로크할 수 있는 데이터 항목이 두 가지 이상 있으면 다중 로크 단위(multiple granularity)라고 말함. 

데이터베이스에서 로크 할 수 있는 단위로는 데이터베이스, 릴레이션, 디스크 블록, 투플 등. 

일반적으로 DBMS는 트랜잭션에서 접근하는 투플 수에 따라 자동적으로 로크 단위를 조정함. 
로크 단위가 작을수록 로킹에 따른 오버헤드 및 동시성의 정도가 증가함. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-11-database_system_9/13.png' }}" alt=""> 
</figure> 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-11-database_system_9/14.png' }}" alt=""> 
</figure> 

---

팬텀 문제(phantom problem)란 한 트랜잭션에 속한 첫 번째 SELECT문과 두 번째 SELECT문의 수행 결과가 다르게 나타나는 것. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-11-database_system_9/15.png' }}" alt=""> 
</figure> 

## 9.3. 회복
트랜잭션을 수행하는 도중에 시스템이 다운되었을 때, 수행 효과가 디스크의 데이터베이스에 일부 반영되었을 수 있음. 
원자성을 보장하기 위해서 이 트랜잭션이 데이터베이스에 반영했을 가능성이 있는 갱신 사항을 취소(UNDO)해야 함. 

트랜잭션이 완료된 직후에 시스템이 다운되면, 모든 갱신 효과가 주기억 장치로부터 디스크에 기록되지 않았을 수 있음. 
회복 모듈은 이 트랜잭션의 갱신 사항을 재수행(REDO)하여 트랜잭션의 갱신이 지속성을 갖도록 해야 함. 

---

여러 응용이 주기억 장치 버퍼 내의 동일한 데이터베이스 항목을 갱신한 후에 디스크에 기록함으로써 성능을 향상시키는 것이 중요함. 

버퍼의 내용을 디스크에 기록하는 것을 가능하면 줄이는 것이 중요. 
예를 들어, 버퍼가 꽉 찼을 때 또는 트랜잭션이 완료될 때 버퍼의 내용을 디스크에 기록할 수 있음. 

---

주기억 장치와 같은 휘발성 저장 장치에 있는 내용은 시스템이 다운되면 사라짐. 
디스크와 같은 비휘발성 저장 장치에 있는 내용은 디스크 헤드 등이 손상을 입지 않는 한 시스템이 다운되어도 유지됨. 

안전 저장 장치(stable storage)는 모든 유형의 고장을 견딜 수 있는 저장 장치. 
두 개 이상의 비휘발성 저장 장치가 동시에 고장날 가능성은 낮으므로, 사본을 중복해서 저장함으로써 안전 저장 장치를 구현함. 

---

재해적고장
- 디스크가 손상을 입어서 데이터베이스를 읽을 수 없는 고장 
- 회복은 데이터베이스를 백업해 놓은 다른 저장 장치를 기반으로 함 

비재해적고장
- 그 이외의 고장
- 로그를 기반으로 한 즉시 갱신(또는 지연 갱신), 그림자 페이징(shadow paging) 등 여러 알고리즘 
- 대부분의 상용 DBMS에서 로그를 기반으로 한 즉시 갱신 방식을 사용 

---

트랜잭션의 원자성과 지속성을 보장하기 위해 DBMS는 로그(log)라고 부르는 특별한 파일을 유지함. 

데이터베이스의 항목에 영향을 미치는 모든 트랜잭션의 연산들에 대해서 로그 레코드를 기록함. 
각 로그 레코드는 로그 순서 번호(LSN: Log Sequence Number)로 식별됨.

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-11-database_system_9/16.png' }}" alt=""> 
</figure> 

주기억 장치내의 로그 버퍼에 로그 레코드들을 기록하고 로그 버퍼가 꽉 찰 때 디스크에 기록함. 

로그는 데이터베이스회복에 필수적이므로 일반적으로 안전 저장 장치에 저장됨. 
로그를 두 개의 디스크에 중복해서 저장하는 이중 로그(dual logging). 

각 로그 레코드가 어떤 트랜잭션에 속한 것인가를 식별하기 위해서 각 로그 레코드마다 트랜잭션 ID를 포함시킴. 
동일한 트랜잭션에 속하는 로그 레코드들은 연결 리스트로 유지됨. 

---

로그 레코드의 유형
- [Trans-ID, start] 
   - 트랜잭션이 생성될 때 기록되는 로그 레코드 
- [Trans-ID, X, old_value, new_value] 
   - Trans_ID를 갖는 트랜잭션이 데이터 항목 X를 old_value에서 new_value로 수정했음을 나타내는 로그 레코드 
- [Trans-ID, commit] 
   - Trans_ID를 갖는 트랜잭션이 데이터베이스에 대한 갱신을 모두 성공적으로 완료하였음을 나타내는 로그 레코드 
- [Trans-ID, abort] 
   - Trans_ID를 갖는 트랜잭션이 철회되었음을 나타내는 로그 레코드 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-11-database_system_9/17.png' }}" alt=""> 
</figure> 

---

트랜잭션의 갱신 연산이 모두 끝나고, 갱신 사항이 로그에 기록되었을 때를 트랜잭션의 완료점(commit point)라고 함. 

DBMS의 회복 모듈은 로그를 검사하여 로그에 [Trans-ID, start] 로그 레코드와 [Trans-ID, commit] 로그 레코드가 모두 존재하는 트랜잭션들은 재수행,  
[Trans-ID, start]로그 레코드는 로그에 존재하지만 [Trans-ID, commit] 로그 레코드가 존재하지 않는 트랜잭션들은 취소함. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-11-database_system_9/18.png' }}" alt=""> 
</figure> 

---

로그먼저쓰기(WAL: Write-Ahead Logging)

트랜잭션이 데이터베이스를 갱신하면 주기억 장치의 데이터베이스 버퍼에 갱신 사항을 기록하고, 
로그 버퍼에는 이에 대응되는 로그 레코드를 기록함. 

데이터베이스 버퍼가 먼저 기록되고 시스템이 다운되면, 로그 레코드가 없어서 이전값을 알 수 없으므로 트랜잭션의 취소가 불가능함. 
따라서 데이터베이스 버퍼보다 로그 버퍼를 먼저 디스크에 기록해야 함. 

---

체크포인트(checkpoint) 필요성
- 시스템이 다운된 시점으로부터 오래 전에 완료된 트랜잭션들은 이미 디스크에 반영되었을 가능성이 큼. 
- 로그를 사용하더라도 어떤 트랜잭션의 갱신 사항이 주기억 장치 버퍼로부터 디스크에 기록되었는가를 구분할 수 없음. 
- 따라서 DBMS는 회복시 재수행할 트랜잭션의 수를 줄이기 위해서 주기적으로 체크포인트를 수행함. 

체크포인트 시점에는 주기억 장치의 버퍼 내용이 디스크에 강제로 기록되므로, 
디스크상에서 로그와 데이터베이스의 내용이 일치하게 됨. 

체크포인트 작업이 끝나면 로그에 [checkpoint] 로그 레코드가 기록됨. 

---

체크포인트를 할 때 수행되는 작업
- 수행 중인 트랜잭션들을 중지시킴. 회복 알고리즘에 따라서는 이 작업이 필요하지 않을 수 있음 
- 주기억 장치의 로그 버퍼를 디스크에 강제로 출력 
- 주기억 장치의 데이터베이스 버퍼를 디스크에 강제로 출력 
- [checkpoint] 로그 레코드를 로그 버퍼에 기록한 후 디스크에 강제로 출력 
- 수행 중이던 트랜잭션들의 ID도 [checkpoint] 로그 레코드에 함께 기록 
- 중지된 트랜잭션의 수행을 재개 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-11-database_system_9/19.png' }}" alt=""> 
</figure> 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-11-database_system_9/20.png' }}" alt=""> 
</figure> 

---

## 9.4. PL/SQL의 트랜잭션
오라클에서 한 트랜잭션은 암시적으로 또는 명시적으로 끝날 수 있음. 

트랜잭션은 SQL문이 실행될 때 시작되어 데이터 정의어/데이터 제어어를 만나거나, Oracle SQL Developer를 정상 종료했을 때 암시적으로 완료(commit)됨. 

COMMIT, ROLLBACK, SAVEPOINT문을 사용하여 트랜잭션의 논리를 명시적으로 제어할 수 있음. 
- COMMIT: 트랜잭션에서 수행한 데이터 조작어의 결과를 데이터베이스에 모두 반영하고 트랜잭션을 완료 
- ROLLBACK: 트랜잭션에서 수행한 데이터 조작어의 결과를 데이터베이스에서 모두 되돌리고 트랜잭션을 철회 
- SAVEPOINT: 트랜잭션 내에 저장점을 표시하여 트랜잭션을 더 작은 부분으로 나눔 
- ROLLBACK TO SAVEPOINT: 트랜잭션에서 지정된 저장점 이후에 갱신된 내용만 되돌림 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-11-database_system_9/21.png' }}" alt=""> 
</figure> 

SQL*Plus에서는 묵시적으로 한 트랜잭션은 데이터 정의어나 데이터 제어어 이전까지 입력한 여러 개의 데이터 조작어로 이루어짐. 
set명령을 사용하여 각 데이터 조작어를 한 트랜잭션으로 처리할 수 있음. 
> set auto on; 또는 set autocommit on;

---

만일 트랜잭션이 데이터베이스를 읽기만 한다면 트랜잭션이 읽기 전용임을 명시하여 DBMS가 동시성의 정도를 높일 수 있음. 

```console
SET TRANSACTION READ ONLY;
SELECT AVG(SALARY) 
FROM EMPLOYEE 
WHERE DEPT='개발부';

// 읽기 전용에서는 갱신 작업이 불가능
SET TRANSACTION READ ONLY;
UPDATE EMPLOYEE 
SET SALARY = SALARY * 1.06; 

// 읽기, 갱신 가능
SET TRANSACTION READ WRITE;
UPDATE EMPLOYEE 
SET SALARY = SALARY * 1.06; 
```

---

고립 수준

SQL2에서 사용자가 동시성의 정도를 몇 가지로 구분하여 명시할 수 있음. 
고립 수준은 한 트랜잭션이 다른 트랜잭션과 고립되어야 하는 정도를 나타냄. 

고립 수준이 낮으면 동시성은 높아지지만 데이터의 정확성은 떨어짐. 
고립 수준이 높으면 데이터가 정확해지지만 동시성이 저하됨. 

응용에서 명시한 고립 수준에 따라 DBMS가 사용하는 로킹 동작이 달라짐. 
한 트랜잭션에 대해 명시한 고립 수준은 다른 트랜잭션의 고립 수준에 영향을 미치지 않음. 

---

오라클에서 제공하는 몇 가지 고립 수준

(1) READ UNCOMMITTED
- 가장 낮은 고립 수준 
- 트랜잭션 내의 질의들이 공유 로크를 걸지 않고 데이터를 읽음 
- 오손 데이터를 읽을 수 있음 
- 갱신하려는 데이터에 대해서는 독점 로크를 걸고, 트랜잭션이 끝날 때까지 보유함 

(2) READ COMMITTED
- 읽으려는 데이터에 대해서 공유 로크를 걸고, 읽기가 끝나자마자 로크를 해제함 
- 동일한 데이터를 읽으면, 이전에 읽은 값과 다른 값을 읽는 경우가 생길 수 있음 
- 갱신하려는 데이터에 대해서는 독점 로크를 걸고, 트랜잭션이 끝날 때까지 보유함 
- 이 고립 수준은 PL/SQL의 디폴트 

(3) REPEATABLE READ
- 검색되는 데이터에 대해 공유 로크를 걸고, 트랜잭션이 끝날 때까지 보유함 
- 트랜잭션 내에서 동일한 질의를 두 번 이상 수행할 때, 이전에 읽은 값이 항상 동일하게 유지됨 
- 갱신하려는 데이터에 대해서는 독점 로크를 걸고, 트랜잭션이 끝날 때까지 보유함 

(4) SERIALIZABLE
- 가장 높은 고립 수준 
- 검색되는 투플들 뿐만 아니라, 인덱스에 대해서도 공유 로크를 걸고 트랜잭션이 끝날 때까지 보유함 
- 한 트랜잭션 내에서 동일한 질의를 두 번 이상 수행할 때, 매번 같은 값을 포함한 결과를 얻음 
- 갱신하려는 데이터에 대해서는 독점 로크를 걸고, 트랜잭션이 끝날 때까지 보유함 
- SERIALIZABLE은 SQL2의 디폴트 고립 수준 

REPEATABLE READ vs SERIALIZABLE 
> SELECT col1 FROM A WHERE col1 BETWEEN 1 AND 10;

REPEATABLE READ
- 이 범위에 해당하는 데이터가 col1=1과 5인 경우, 다른 사용자가 col1이 1이나 5인 Row에 대한 UPDATE가 불가능 
- col1이 1과 5를 제외한 나머지 범위에 해당하는 Row를 INSERT하는 것은 가능 

SERIALIZABLE
- 범위에 해당되는 데이터에 대한 수정 및 입력이 불가능 
- SQL의 결과가 항상 동일함 

```console
SET TRANSACTION READ WRITE
ISOLATION LEVEL SERIALIZABLE;

SET TRANSACTION READ WRITE
ISOLATION LEVEL REPEATABLE READ;

SET TRANSACTION READ WRITE
ISOLATION LEVEL READ COMMITTED;

SET TRANSACTION READ WRITE
ISOLATION LEVEL READ UNCOMMITTED;
```

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-11-database_system_9/22.png' }}" alt=""> 
</figure> 
