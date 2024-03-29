---
Created: 2024-01-17T14:46:00
Updated: 2024-01-17T14:46:00
Course: COMP2200
tags:
  - Best_Practice
Reviewed: false
---
- [[#const 포인터: 주소를 보호함|const 포인터: 주소를 보호함]]
- [[#const 포인터: 값을 보호함|const 포인터: 값을 보호함]]
- [[#같이 쓰기|같이 쓰기]]
- [[#const는 절대로 제거하지 말 것|const는 절대로 제거하지 말 것]]
- [[#==const 베스트 프랙티스==|const 베스트 프랙티스]]

## const 포인터: 주소를 보호함
---
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

## const 포인터: 값을 보호함
---
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
- 상수 int를 선언할 때 const int를 사용해서 그렇게 쓰는 듯
- 그러니 방법 1을 코딩 표준으로 쓰자.

## 같이 쓰기
---
```C
const int* const p = &num;
```
- "p is a const pointer to const int"
- 자주 쓰이진 않음
	- 초기화된 후 절대 바뀌지 않는 변수가 있을 때 정도만 유용할 듯
		- 전역변수 or 구조체 멤버 변수 등
- 중요한 것은 값을 보호하는 const가 더 중요하다는 것

## const는 절대로 제거하지 말 것
---
```C
void print_array(const int* data, const int length)
{
        *((int*)data) = 10; /* 절대 하지 말 것. */
}

int main(void)
{
        int nums[] = { 1024, 9 };
        int* p = nums;
}
```
- 외부에서 볼 때 저 함수는 값을 바꾸지 않는 함수.
- 저러한 동작을 꼭 해야한다면 함수 시그니처를 잘못 작성한 것임.

## ==const 베스트 프랙티스==
---
- const는 최대한 다 붙이라 했다
	- 그러니 다 붙이는게 좋음
	- 반드시 const가 필요 없는 경우가 아니라면
- const 캐스팅은 하지 말 것
	- 함수 시그니처에서 안 바꾼다고 약속하고 어기면 안 됨