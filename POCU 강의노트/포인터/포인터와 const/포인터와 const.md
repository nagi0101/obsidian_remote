---
Created: 2024-01-17T14:46:00
Updated: 2024-01-17T14:46:00
Course: COMP2200
tags: 
Reviewed: false
---
## 포인터와 const
---
### const 포인터: 주소를 보호함
- 기본 자료형 변수의 경우 const를 붙이면 그 변수에 저장한 값을 변경 불가
	- 반드시 필요하다고 느끼지 않는 경우가 많아 강요 안 하는 코딩 표준도 많음
	- 실수가 발생해도 함수 범위 내에서 발생하는 실수이기 때문
- 포인터 변수에 const를 붙이면 메모리 주소를 바꿀 수 없음
	- 포인터 변수에 저장된 값이 메모리 주소이니까
	- 그러나 포인터 변수는 오른쪽에서 왼쪽으로 읽음
		- "p is a const pointer to int"
		- 때문에 아래와 같이 써야함
```C
int* const p = &num; /* 이렇게 써야 주소 변경 불가 */
const int* p = &num; /* 이건 다른거임 */
```
- 생성과 동시에 초기화
- 초기화 후 값 변경 불가
- const가 아닌 변수에 대입 가능
- const 포인터가 가리키는 대상의 값 변경은 가능
### const 포인터: 값을 보호함
- 실수가 있을 경우 함수 내에서 뿐만 아니라 전역적으로 문제가 발생
- 이게 주소 보호보다 더 중요
- 이 const는 반드시 신경써야함
- 주소에 저장되어 있는 값을 변경하는 것 방지
- 영어로
	1.  "p is a pointer to int, which is const"
	1.  "p is a pointer to const int"
```c
const int* p = &num;  /* 방법1 */
int const * p = &num; /* 방법2 */
```
- 논리적으로 방법 2가 더 말이 되나 흔히 방법 1로 씀
- 수const int