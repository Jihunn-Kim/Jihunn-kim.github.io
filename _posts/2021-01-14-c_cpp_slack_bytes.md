---
title: "c언어 slack bytes"
last_modified_at: 2021-01-14
excerpt: "구조체 슬랙 바이트 사용 이유"
categories:
  - c_cpp
---

## Slack bytes 사용 이유
<a href="https://www.geeksforgeeks.org/slack-bytes-in-structures-explained-with-example" target="_blank"><b> 마이크로 프로세서의 데이터 접근 속도 향상을 위해 사용된다. </b></a>  
데이터가 짝수 주소를 가질 때, 홀수 주소보다 접근 속도가 빠르다. 이를 위해 컴파일러가 메모리를 더 사용하더라도 slack byte를 구조체에 추가한다. 
예를 들어, 구조체의 각 데이터에 2, 4, 8 .. 의 배수의 주소를 할당한다.

<figure>
	<img src="{{ '/assets/img/2021-01-14-c_cpp_slack_bytes/1.png' }}" alt=""> 
	<figcaption><a href="https://www.geeksforgeeks.org/slack-bytes-in-structures-explained-with-example" target="_blank"><b> 출처: GeeksforGeeks </b></a></figcaption>
</figure>

---

slack bytes 때문에, 변수 순서만으로도 구조체 크기가 달라질 수 있다. (실행 환경에 따라 다름)
```c++
struct x {
	int a;   // four bytes
	char b;  // one byte
		 // three bytes slack
	int c;   // four bytes
		 // three bytes slack
	char d;  // one byte
} xx;

struct xy {
	int a;   // four bytes
	int c;   // four bytes
	char b;  // one byte
	char d;	 // one byte
		 // three bytes slack
} xxyy;

cout << sizeof(xx) << ' ' << sizeof(xxyy); // 16 12 출력
```

