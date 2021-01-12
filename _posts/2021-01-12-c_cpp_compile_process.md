---
title: "c언어 컴파일 과정"
last_modified_at: 2021-01-12
categories:
  - c_cpp
---

## c언어 컴파일 순서
1. 전처리기 - #구문(#include, #define) 치환
2. 컴파일러 - 하드웨어 종속적인 어셈블리코드 생성
3. 어셈블러 - 오브젝트 파일(기계어) 생성
4. 링커     - 여러 오브젝트, 라이브러리를 합쳐서 실행파일 생성

<figure>
	<img src="{{ '/assets/img/2021-01-12-c_cpp_compile_process/1.png' }}" alt=""> 
</figure>

---

## 컴파일러 플랫폼 종속성
바이트코드로 변환 후, 인터프리터가 실행하는 python, java와 달리  
c언어 컴파일러는 운영체제, cpu에 따라 컴파일러가 다르게 요구된다.
| 운영체제 | 컴파일러 |
|:----:|:----:|
| 윈도우용 | visual c++, dev c++ |
| 리눅스용 | gcc |

