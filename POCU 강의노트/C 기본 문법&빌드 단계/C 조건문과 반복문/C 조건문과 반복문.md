---
Created: 2023-12-18T13:40:00
Updated: 2023-12-18T13:40:00
Course: COMP2200
Tags:
  - Best_Practice
  - Grammar
Reviewed: false
---
- [[#if|if]]
	- [[#if#if문과 boolean expression|if문과 boolean expression]]
- [[#switch/case|switch/case]]
- [[#for|for]]
- [[#while|while]]


## if
---

- 모습은 다른 언어와 크게 다를것 없음.

### if문과 boolean expression

- C의 비교, 조건 연산자를 사용한 표현식은 참일 경우 1, 거짓일 경우 0을 반환.
- (b > 10)은 true/false를 반환하는 것이 아니라, 1 또는 0을 반환함.
- 숫자를 if문의 조건식에 넣어도 곧바로 판단 가능.
- 메모리 주소(포인터)나 float도 마찬가지.
    - 모든 비트패턴이 0이 아니면 false, 아니면 true
        
        ![[Untitled 16.png|Untitled 16.png]]
        

  

## switch/case
---
- C는 정수형만 사용 가능.
- case에서 break를 빼먹으면 switch를 탈출하지 않고 그 아래 있는 코드를 계속 실행.
- 다른 곳에서 break를 만나거나 switch 블록의 끝에 도착하면 탈출함.
    - 이렇게 아래 있는 코드를 계속 실행하는 것을 fall-through라고 함.
- ==코딩 표준: fall-through를 명시적으로 표기==
    - 의도적으로 break를 사용하지 않을 경우 /* intentional fallthrough */ 주석을 반드시 붙이자!
    - break를 넣는 것이 일반적인 경우이기에 넣지 않는 경우 협업시 햇갈릴 수 있음.
- case 레이블은 상수만 가능.
    - 또한 상수는 컴파일 타임에 반드시 결정되어야 함.

## for
---

![[Untitled 2 9.png|Untitled 2 9.png]]

- for문의 초기화 코드에 size_t i = 0을 못 씀.
    - 변수 선언은 제일 위에서 해야하기 때문.

## while
---

- 특별한건 없음.
- ==코딩 표준: 조건식에 counter 변수를 넣는 대신, ‘== 0’ 혹은 ‘!= 0’을 넣자.==
    - counter 증감은 꼭 조건문 안이 아니라 반복문 마지막에 하는 것도 괜찮음.

```C
int day = 5;

/* 나쁜 예 */
while (day--) {
	printf("%d\n", day);
}
```

```C
int day = 5;

/* 좋은 예 */
while (day-- != 0) {
	printf("%d\n", day);
}
```