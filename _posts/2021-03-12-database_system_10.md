---
title: "데이터 베이스 요약 - 10. 데이터베이스 보안과 권한 관리"
last_modified_at: 2021-03-12
show_date: true
classes: wide
excerpt: ""
categories:
  - database
---

## 10.1. 데이터베이스 보안
세 가지 유형의 보안 
- 물리적 보호: 자연재해, 도둑, 컴퓨터 시스템에 대한 우연한 손상 등의 위험으로부터 데이터베이스를 보호하는 것 
- 권한 보호: 권한을 가진 사용자만 특정한 접근 모드로 데이터베이스를 접근할 수 있도록 보호 
- 운영 보호: 데이터베이스의 무결성에 대한 사용자 실수의 영향을 최소화하거나 제거하는 조치 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-12-database_system_10/1.png' }}" alt=""> 
</figure> 

---

DBMS가 데이터베이스 보안과 관련하여 제공해야 하는 두 가지 기능 
- 접근 제어(access control) 
   - 데이터베이스 시스템에 대한 접근을 통제할 수 있는 기능 
   - DBMS는 로그인 과정을 통제하기 위하여 사용자 계정과 암호를 관리함 
- 보안 및 권한 관리 
   - DBMS는 특정 사용자 또는 그룹이 지정된 데이터베이스 영역만 접근 하도록 통제하는 기능을 제공함 

---

두 가지 보안 기법
- 임의 보안 기법(discretionary security mechanism) 
   - 사용자들에게 특정 릴레이션, 투플, 또는 애트리뷰트를 지정된 모드(읽기, 삽입,수정 등)로 접근할 수 있는 권한을 허가하고 취소하는 기법 
   - 대부분의 상용 관계 DBMS에서 사용되는 기법 
   - DBMS는 시스템 카탈로그에 누가 권한을 허가받았고 권한을 취소 당했는가를 유지함 
- 강제 보안 기법(mandatory security mechanism) 
   - 데이터와 사용자들을 다양한 보안 등급(1급 비밀, 2급 비밀, 일반 정보 등)으로 분류하고 해당 조직에 적합한 보안정책을 적용함 
   - 다단계 보안을 시행하기 위해 사용됨 

---

데이터베이스 보안을 위해 데이터베이스 관리자가 수행하는 작업
- 사용자 또는 그룹에 대한 계정과 암호의 생성, 권한 부여와 취소 
- 로그인 세션 동안 사용자가 데이터베이스에 가한 모든 연산들을 기록할 수 있음 
- 데이터베이스 감사, 특정 기간 동안 데이터베이스에서 수행된 모든 연산들을 검사하기 위해서 시스템 로그를 조사 

## 10.2. 권한 관리
객체의 생성자(소유자)는 객체에 대한 모든 권한을 가짐. 
생성자는 소유한 객체에 대한 특정 권한을 GRANT문을 사용하여 다른 사용자나 역할에게 허가할 수 있음. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-12-database_system_10/2.png' }}" alt=""> 
</figure> 

GRANT절에 SELECT, INSERT, DELETE, UPDATE, REFERENCES 중 한 개 이상의 권한을 포함할 수 있음. 

애트리뷰트를 수정하려면 그 애트리뷰트에 대한 UPDATE 권한이 필요. 

외래 키 제약조건을 만들려면 해당 릴레이션에 대해 REFERENCES 권한이 필요. 

사용자가 WITH GRANT OPTION절과 함께 권한을 허가받았으면, 그 사용자도 이 옵션과 함께 또는 없이 권한을 다른 사용자에게 허가할 수 있음. 

다른 사용자들이 릴레이션에 직접 접근하지 못하게 하려는 경우에는 릴레이션에 대한 권한은 허가하지 않고, 
릴레이션을 참조하는 뷰를 정의한 후 이 뷰에 대해 권한을 부여할 수 있음.

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-12-database_system_10/3.png' }}" alt=""> 
</figure>

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-12-database_system_10/4.png' }}" alt=""> 
</figure>

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-12-database_system_10/5.png' }}" alt=""> 
</figure>

---

사용자에게 허가한 권한을 취소하기 위해서 REVOKE문을 사용함. 
권한을 취소하면, 권한을 취소 당한 사용자가 WITH GRANT OPTION을 통해서 다른 사용자에게 허가했던 권한들도 연쇄적으로 취소됨. 

취소하려는 권한을 허가했던 사람만 그 권한을 취소할 수 있음. 
권한을 허가했던 사람은 자신이 권한을 허가했던 사용자의 권한만을 취소할 수 있음. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-12-database_system_10/6.png' }}" alt=""> 
</figure>

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-12-database_system_10/7.png' }}" alt=""> 
</figure>

---

여러 사용자들에 대한 권한 관리를 단순화하기 위해 역할(role)을 사용함. 
역할은 사용자에게 허가할 수 있는 권한들의 그룹으로서 이름을 가짐. 

사용자는 여러 역할들에 속할 수 있으며, 여러 사용자들이 동일한 역할을 허가 받을 수 있음. 
권한들을 역할에게 허가하고, 역할을 각 사용자에게 허가함. 

역할과 연관된 권한들에 변화가 생기면, 그 역할을 허가받은 모든 사용자들은 자동적으로 변경된 권한들을 가지게됨. 

역할을 생성하는 방법은 DBMS마다 차이가 있음. 
오라클에서는 CREATE ROLE문을 사용하여 역할을 생성함. 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-12-database_system_10/8.png' }}" alt=""> 
</figure>

## 10.3. 오라클의 보안 및 권한 관리
오라클 사용자는 접속하려는 데이터베이스에 계정과 패스워드를 가져야 함. 
별도로 권한을 허가 받지 않으면, 데이터베이스에서 어떤 작업도 수행할 수 없음. 

시스템 권한과 객체 권한 등 두 가지 유형의 권한이 있음. 
- 시스템 권한: 사용자가 데이터베이스에서 특정 작업을 수행할 수 있도록 함(CREATE TABLE 시스템 권한 등) 
- 객체 권한: 사용자가 특정 객체(테이블, 뷰 등)에 대해 특정 연산을 수행할 수 있도록 함 

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-12-database_system_10/9.png' }}" alt=""> 
</figure>

---

데이터베이스 관리자는 GRANT문을 사용하여 사용자에게 특정 시스템 권한들을 허가. 
> GRANT CREATE SESSION TO KIM WITH ADMIN OPTION;

WITH ADMIN OPTION을 사용하여 허가하면 권한을 받은 사용자가 다시 이 권한을 다른 사용자에게 허가할 수 있음. 
시스템 권한을 취소할 때는 연쇄적인 취소가 일어나지 않음. 

---

객체의 소유자는 객체에 대한 모든 권한을 보유하며, 객체에 대한 권한을 다른 사용자나 역할에게 허가할 수 있음. 
PUBLIC 키워드를 사용하여 권한을 허가하면 모든 사용자에게 권한을 부여하게 됨. 

각 객체 마다 허가할 수 있는 권한들에 차이가 있음.

<figure style="width: 600px" class="align-center">
 	<img src="{{ '/assets/img/2021-03-12-database_system_10/10.png' }}" alt=""> 
</figure>

---

사용 패턴을 여러 관점에서 분석하여 각 사용 패턴에 맞게 미리 정해 놓은 역할이 약 20여개 있음(connect, resource, dba, SYSOPER, SYSDBA 등). 

connect 역할만 있으면 오라클 데이터베이스에 로그인하고, 
만일 다른 사용자의 데이터를 검색/갱신할 수 있도록 권한을 받았으면 이를 검색/갱신할 수 있음. 

connect 역할과 함께 resource 역할이 있으면 테이블과 인덱스를 생성하고, 
자신의 객체에 대해 다른 사용자에게 권한을 허가하거나 취소할 수 있음.

---

데이터 사전 뷰를 사용하여 권한에 관련된 정보를 검색할 수 있음. 
USER_TAB_PRIVS 또는 DBA_TAB_PRIVS 데이터 사전 뷰를 통해서 검색할 수 있음. 

```console
SELECT * 
FROM   USER_TAB_PRIVS;
```

사용자 LEE가 허가 받은 시스템 권한 확인. 
```console
// LEE로 로그인을 한 후에 아래의 질의를 수행
SELECT *
FROM   USER_SYS_PRIVS;
```

---

사용자 LEE로 로그인해서 KIM의 EMPLOYEE 테이블 대한 질의들을 수행. 
다른 소유자의 테이블을 접근할 때는 테이블 앞에 소유자의 계정을 붙여야 함. 

```console
INSERT INTO KIM.EMPLOYEE 
VALUES(3428, '김지민', '사원', 2106, 1500000, 2);
```

LEE가 KIM의 EMPLOYEE 테이블을 자주 접근한다면 매번 테이블 앞에 KIM을 붙이는 것이 번거로움. 

동의어(synonym)를 만드는 구문
> CREATE SYNONYM 동의어 FOR 객체; CREATE SYNONYM EMP FOR KIM.EMPLOYEE;


