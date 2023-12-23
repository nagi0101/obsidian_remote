---
Created: 2023-12-18T13:47:00
Updated: 2023-12-18T13:47:00
Course: COMP2200
Tags:
  - Best_Practice
  - Grammar
Reviewed: false
---
- [[#범위|범위]]
- [[#블록 범위|블록 범위]]
- [[#파일 범위|파일 범위]]
- [[#함수 범위|함수 범위]]
- [[#함수 선언 범위|함수 선언 범위]]


## 범위
---
- 총 네 가지 범위가 있다.
    - 블록 범위
    - 파일 범위
    - 함수 범위
    - 함수 선언 범위

  

## 블록 범위
---

- {} 안에 선언된 것들은 그 블록 안에서만 사용 가능
- 블록 안에 또 다른 블록을 넣을 수 있다.
- 블록의 최상단에서 선언시 그 위치가 함수의 맨 위가 아니어도 컴파일 가능하다.
    
    ![[Untitled 7.png|Untitled 7.png]]
    
    - 함수 시작지점에서 모든 변수를 선언하면 실수할 여지가 있음.
        - 변수는 사용하기 직전에 선언하는 것이 이상적이나, C에선 불가능.
        - 정확하게 어디에서 사용하는 변수인지 파악 불가능.
        - 중간에 그 값이 바뀔수도
    - 블록을 이용해서 함수 중간에 선언하는 것도 하나의 방법.
    - ==코딩 표준: 변수 가리기(variable shadowing) 금지==
        
        ```C
        /* 안 좋은 코드 */
        int num = 0;
        printf("%d", num); /* 0 */
        {
        	int num = 1;
        	printf("%d", num); /* 1 */
        }
        ```
        
        ```C
        /* 좋은 코드 */
        int your_score = 0;
        {
        	int my_score = 1;
        	printf("%d", my_score); /* 1 */
        }
        ```
        
        - 모든 변수 이름을 다르게 지을 것.

## 파일 범위
---

```C
\#include <stdio.h>

static int s_num = 1024;

int add(int op1, int op2);

int main(void)
{
	s_num = add(10, 30);
	return 0;
}
```

- 어떤 블록이나 매개변수 목록에도 안 속하고 파일 안에 있는 것
    - 정확히는 파일이 아니라 트랜슬레이션 유닛(컴파일 기본 단위). 나중에 살펴봄.
- 파일 범위에 있는 메모리 위치: 데이터 섹션
	- 힙 섹션 아래, 코드 섹션 위.
- 이게 바로 전역변수 

## 함수 범위
---

```C
\#include <stdio.h>

int main(int argc, char** argv)
{
	if(argc != 3) {
		goto exit;
	}
	printf("You have 3 arguments!");

exit:
	return 0;
}
```

- 유일한 예: 레이블(lable)
- goto같은데서 쓰이는 것.
- 함수 안에서 선언된 레이블은 함수 어디에서라도 접근 가능
    - 다른 범위들은 위에서 선언된 것만 접근 가능함.

## 함수 선언 범위
---

- 함수 선언의 매개변수 목록에 있는 것은 그 목록 안에서만 접근 가능
- 많이 쓸 일은 없음.
- 다음과 같은 예는 괜찮음.
    
    ```C
    void do_something(
    	double value,                  /* 함수 선언 범위 */
    	char array[10 * sizeof(value)  /* value는 첫 번째 매개변수 */
    );
    ```