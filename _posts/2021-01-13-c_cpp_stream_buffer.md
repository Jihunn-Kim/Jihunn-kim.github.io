---
title: "c언어 stream buffer"
last_modified_at: 2021-01-13
excerpt: "buffer 사용 이유, carriage return"
categories:
  - c_cpp
---

## Buffer 사용 이유
<a href="https://stackoverflow.com/questions/23298717/what-is-meant-by-stream-buffering" target="_blank"><b> 파일과 콘솔 I/O 접근은 메모리 연산보다 매우 느리기 때문이다. </b></a>  
파일 입출력이나 키보드 입력이 들어올 때마다 처리하는 것은 다른 연산이 비효율적으로 기다려야 한다.
<a href="https://stackoverflow.com/questions/33874548/c-what-is-the-need-of-both-buffer-and-stream" target="_blank"><b> 따라서 파일, 콘솔에 대한 추상화를 제공하는 stream의 data를 buffer에 잠시 저장했다가, 한번에 처리한다. </b></a>

## Buffer 종류
Buffer에는 unbuffered, block buffered, line buffered 종류가 있다. Block buffered는 buffer가 전부 찰 때 비우며, line buffered는 newline을 
발견할 때마다 buffer를 비운다. 마지막으로 unbuffered는 stderr처럼 바로바로 출력한다.

## Carriage return 문제
c의 scanf 함수의 경우 기본적으로 input stream buffer에 리턴 문자를 남긴다.  

아래와 같이 %c 가 아닌 변환 명세는 자동으로 리턴 문자를 버린다.
```c
int a, b, c, d;
scanf("%d %d", &a, &b);
scanf("%d %d", &c, &d); // 정상적으로 입력을 받음
```

하지만 %c 로 입력을 받는다면 예상외의 값을 받을 수 있다.
```c
int a, b, d;
char c;
scanf("%d %d", &a, &b); // 1 2 입력
scanf("%c %d", &c, &d); // 3 4 입력
printf("%c", c);	// ' ' 출력
printf("%d", c);	// 10 출력 - ' ' 아스키 값
```

scanf 문자열 앞에 공백을 넣어 리턴 문자를 강제로 없애버리면 문제를 해결할 수 있다.
```c
int a, b, d;
char c;
scanf("%d %d*c", &a, &b); // 1 2 입력
scanf(" %c %d", &c, &d); // 3 4 입력
printf("%c", c);	// 3 출력
```

c++ 또한 cin >> 은 리턴 문자를 버퍼에 남긴다. cin >> 과 getline 의 작동 원리가 다른 이유는,
<a href="https://stackoverflow.com/questions/28109679/why-does-cin-command-leaves-a-n-in-the-buffer" target="_blank"><b> 그냥 그렇게 "정의" 되었기 때문이라고 한다. </b></a>  
```c++
int a;
cin >> a;	 // 1 입력

string b;
getline(cin, b); // asdf 입력
cout << b;	 // ' ' 출력
```

cin >> 이후 getline을 사용하려면 리턴 문자를 버퍼에서 삭제해야 한다.
```c++
int a;
cin >> a;	 // 1 입력

string b;
cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
getline(cin, b); // asdf 입력
cout << b;	 // asdf 출력
```
