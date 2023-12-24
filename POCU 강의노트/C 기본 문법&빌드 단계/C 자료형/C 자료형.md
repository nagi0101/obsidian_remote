---
Created: 2023-12-18T13:19:00
Updated: 2023-12-18T13:19:00
Course: COMP2200
Tags:
  - Best_Practice
  - Grammar
Reviewed: false
---
- [[#자료형|자료형]]
	- [[#자료형#unsigned와 signed|unsigned와 signed]]
	- [[#자료형#char|char]]
	- [[#자료형#short|short]]
	- [[#자료형#int|int]]
	- [[#자료형#long|long]]
	- [[#자료형#float|float]]
	- [[#자료형#double|double]]
	- [[#자료형#long double|long double]]
	- [[#자료형#C 기본 자료형 정리|C 기본 자료형 정리]]
	- [[#자료형#bool|bool]]
	- [[#자료형#열거형(enum)|열거형(enum)]]
	- [[#자료형#변수 선언|변수 선언]]


## 자료형

---
- 기본 자료형
    
    |   |   |   |   |
    |---|---|---|---|
    |char|short|int|long|
    |float|double|long double||
    

### unsigned와 signed

- 부호 없는 자료형 앞에 unsigned라는 단어를 넣어줘야함.
- 부호 있음을 명확히 보여주기 위해 signed를 붙일수도 있음.
- unsigned/signed 생략시 기본적으론 signed.
    - 예외: char

### char

- ==최소== 8비트인 정수형(표준은 8비트 이상이라고만 정의함)
- char가 몇 비트인지 찾는 방법
    - <limits.h> 헤더 include 한 뒤, CHAR_BIT를 보면 몇 비트인지 확인 가능.
        
        ![[Untitled 12.png|Untitled 12.png]]
        
    - C 표준은 기본 자료형의 정확한 바이트 수를 강요 안 함
    - 심지어 1 바이트를 CHAR_BIT만큼이라고 말함(==1바이트가 8비트가 아닐 수 있음==)
        - 기기에 따라 특정 크기를 사용하기 어려울 수 있기 때문
- char는 ASCII 문자를 표현하기 충분한 크기
    - ASCII는 0~127인 숫자(하위 7개 비트 사용해서 표현 가능한 크기)
- signed/unsigned
    - C 표준에선 생략시 기본값을 정해두지 않았음.
    - 컴파일러 따라 달라짐.
    - 아스키 범위는 최상위 비트를 사용하지 않기 때문에 부호 여부는 무관.
    - 단, ==8비트 정수형으로 쓸때엔 반드시 char 앞에 unsigned/signed를 넣어주는 것이 좋음==
    - 그렇지 않을 경우 포팅해도 문제 없는 정수 범위는 0~127 사이뿐.
- char의 부호 여부 판단법
    - <limits.h> 헤더 파일에서 CHAR_MIN을 보고 부호 없는 char가 signed인지 unsigned인지 판단 가능.
    - 그러나 char 앞에 signed, unsigned 넣어주는 습관을 가지면 확인할 필요가 없다!
- char로 표현 가능한 숫자
    - C 표준은 1의 보수를 사용하는 기계를 고려하여 signed char는 -127~127만 사용.
    - 대부분의 경우 signed는 -128~127, unsigned는 0~255.
    - 대부분의 컴파일러(VS, Clang, gcc)의 경우 기본이 signed이나, gcc를 android에서 돌릴 경우에는 unsigned가 기본인 등 예외는 있음. signed/unsigned 써주는게 확실.

### short
- 최소 16비트, char 크기 이상의 정수형.
- 포팅 문제 없는 값의 범위(표준)
    - unsigned: 0~65535
    - signed: -32767~32767
- int보다 짧아 메모리를 적게 쓰기 위해 사용
- 그러나 int 대신 short를 사용할 경우 성능이 느려질수도
    - CPU에서는 보통 int를 기본형으로 사용하기 때문
- 표준과 무관히 보통 안전하게 생각해도 되는 것
    - 크기: 16비트
    - 범위
        - unsigned: 0~65535
        - signed: -32768~32767

### int

- 표준에 따르면 ==최소 16비트== 그리고 short 크기 이상인 정수형
- int는 기본 정수라는 의미!
- 따라서 CPU가 기본적으로 처리하는 크기여야 함
    - 기본적으로 처리하는 크기란 ALU가 사용하는 기본 데이터.
    - 즉, 워드(word) 크기라 부르는 것.
    - 워드 크기는 레지스터 크기랑 일치함.
- 즉, CPU에 따라 다르다.
- 예전에는 16비트 CPU가 흔했기에 최소 16비트.
- 그 뒤에 32비트 컴퓨터가 나오며 int의 크기는 32비트가 됨.
- 그러나 이제 64비트 컴퓨터인데?
    - 그러나 여전히 32비트에 머물고 있음
    - 원칙적으로 말하면 C표준을 어긴 것
    - 너무 오랫동안(다른 언어들 포함) int를 32비트로 사용함.
    - 32비트에서 64비트로 바꾼다고 성능이 무조건 빨라지지도 않음(캐시 메모리 등 때문)
    - int를 64비트로 올리면 short는 32비트가 되어야 하나?
    - 이런 이유들로 인해 여전히 대부분 32비트를 사용함
- 포팅에 안전한 범위: short와 같음
- 표준 무관하게 보통 안전하게 생각해도 되는 것
    - 크기: 32비트
    - 범위
        - unsigned: 0~4,294,967,295
        - signed: -2,147,483,648~2,147,483,647
- int의 리터럴
    
    ```C
    int signed_int = -1024;
    unsigned int unsigned_int1 = 394;
    unsigned int unsigned_int2 = 2147483648; /* 경고 */
    unsigned int unsigned_int3 = 2147483648u; /* 경고 없음. 대문자 U도 가능. */
    ```
    
    - 리터럴
        - ‘u’ 혹은 ‘U’: unsigned를 표현하는 접미사
        - ==signed의 최댓값보다 큰 값을 unsigned int에 대입할 경우 ‘u’ 혹은 ‘U’를 붙여야함==
        - 안 붙이면 경고 발생
            
            ```Shell
            warning: integer literal is too large to be represented in type 'long', interpreting as 'unsigned long' per C89; this literal will have type 'long long' in C99 onwards [-Wc99-compat]
            ```
            

### long

- int가 16비트일 때 그것보다 2배 큰 자료형이 필요했음
- 따라서 long은 최소 32비트, int 이상의 크기
    - 다른 언어에선 long이 보통 64비트. 혼동 주의.
- 최소 64비트인 정수형은 C89에는 없음.
- 포팅에 안전한 범위: -2,147,483,647~2,147,483,647
- 표준 무관하게 보통 안전하게 생각해도 되는 것: int와 같음
- 리터럴
    - ‘l’ 혹은 ‘L’: long을 의미하는 접미사
        - ==그러나 숫자 1 뒤에 ‘l’이 오면 햇갈릴 수 있으니 가능한 대문자 L을 사용하는 편이 좋을 듯 하다. lu보다는 ul, UL 사용.==
    - ‘u’ 혹은 ‘U’: unsigned를 표현하는 접미사
    - 두 접미사를 같이 쓸 수 있음: unsigned long이라는 의미.
        
        ```C
        long signed_long = -200000000l;    /* 대문자 L도 가능 */
        unsigned long unsigned_long1 = 2147483647;
        unsigned long unsigned_long2 = 2147483648;    /* 경고 */
        unsigned long unsigned_long3 = 2147483648ul;  /* 경고 없음. ul, lu, UL, LU등 가능 */
        ```
        

  

### float

- 부동 소수점형 자료형은 대부분 IEEE754로 대동단결 됨.
    - float은 IEEE 754 Single(32bit)
    - double은 IEEE 754 Double(64bit)
- 그러나 C는 CPU가 IEEE754를 지원하는 실수 계산 장치를 장착하기 전부터 쓰임.
- 표준에 따르면
    - C의 float은 IEEE754가 아닐 수 있음
    - 컴파일러 구현에 따라 다름(최소한 C89에서는)
    - 크기는 char 이상이기만 하면 됨
- unsigned형 없음
- 표준에 무관하게 보통 안전하게 생각되는 것
    - 크기: 32bit
    - 범위: IEEE 754 Single과 동일
- 관련 헤더 파일: float.h
- 리터럴
    
    ```C
    float pi1 = 3.14f; /* F도 됨 */
    float pi2 = 3.14uf; /* 컴파일 오류. float은 접미사 u를 안 씀 */
    ```
    
    - ‘f’혹은 ‘F’: float을 의미하는 접미사.
        - 대부분 소문자 ‘f’ 사용. 대문자는 잘 안 씀.
        - 3.14f, 3.0f등으로 표기 가능. 소수점 아래가 0인 경우 보통 3.f와 같이 많이 씀.

### double

- 표준에 따르면 CPU가 계산에 사용하는 ==기본== 데이터 크기
    - 크기는 float 이상이면 됨
    - float은 그저 double보다 빠르게 연산하기 위해 만든 작은 부동소수점.
> [!important]  
> double이 CPU가 사용하는 기본 데이터 크기라면 int와 같이 double이 float보다 빨라야 할 것 같지만 그렇지 않음.64비트 기계에서도 평균적으로 32비트인 int 연산이 64비트인 long long 보다 빠름 (이유는 다양. 캐시 메모리 크기도 그중 하나)보통 float은 32비트 double은 64비트인데 부동소수점 연산할때는 요즘 compiler가 벡터 레지스터에 packing해줘서 여러 부동소수점을 한번에 연산. 벡터 연산자가 128비트면 double 2개 한번에 연산할 시간에 float 4개 연산이 가능등등  
        
- 역시 컴파일러 구현따라 다름(IEEE 754 Double)이라는 보장이 없음.
- unsigned형 없음.
- 표준 무관하게 보통 안전하게 생각해도 되는 것
    - 크기: 64bit
    - 범위: IEEE 754 Double과 동일.
- 관련 헤더 파일: float.h

### long double

- double보다 정밀도가 높음
- double 이상의 크기면 됨
- unsigned 없음
- 관련 헤더 파일: float.h

  

### C 기본 자료형 정리

- 데스크톱에서는 다른 언어와 비슷하게 사용 가능
    - 예외: long(32비트)
- 소형 기기를 다룰때에는 메뉴얼에서 자료형 크기 확인 후 사용할 것
- 여기저기 사용할 코드라면
    - 포팅이 보장되는 범위의 값으로만 사용할 것
    - float/double은 플랫폼 사이에 값이 정확하지 않을 수 있음.
        - 비트 패턴에 의존하지 말것.

### bool

- C89에 없음
- C99에서 (좀 이상한 형태로) 새로 들어옴
- 대부분의 C 프로그래머는 bool을 사용하지 않음.
    - 정수로 대신 사용 가능
    - 0이면 false, 아니면 true
    - 하드웨어에 실제 bool이 없음. 0이냐 아니냐만 있을 뿐. 어셈블리 까보면 확인 가능.
    - while문의 조건으로 숫자를 사용 가능함. 물론 가독성은 별로…
- ==코딩 표준: 참, 거짓을 반환해야 할 때는?==
    - C에서 참이나 거짓을 반환해야 하는 함수의 경우 보통 이렇게 함
        
        ```C
        int is_student(const int id)
        {
        	if(/* 조건 */)
        	{
        		return 1;
        	}
        
        	return 0;
        }
        ```
        
        - 거짓일 때 0을 반환
        - 참일 때 1을 반환

### 열거형(enum)

- C# 열거형과 다른 점    
    - int와 섞어서 사용 가능(형을 강제하지 않음)
    - 따라서 int→enum, enum→int, enum→enum 다 대입 됨
    - 형을 강제하지 않아 안 쓰는것보다 보기는 좋으나 실수를 막아주지는 못함.
- C에서 열거형은 그냥 정수에 별명 붙이는 수준.
- ==코딩 표준: C에서는 enum 멤버의 이름 앞에 emun의 이름을 붙여라==
    
    ```C
    enum day {Monday, TUESDAY, WEDNESDAY, /* 생략 */} /* 나쁜 예 */
    enum day {DAY_Monday, DAY_TUESDAY, DAY_WEDNESDAY, /* 생략 */} /* 좋은 예 */
    ```
    
    - 다른 언어에서와 달리 enum이 어디서 온 enum인지 한눈에 보이지 않기 때문
- 사용 예
    
    ```C
    enum champ {
    	CHAMP_ZED,
    	CHAMP_JAX,
    	CHAMP_VAYNE,
    	CHAMP_LULU,
    	CHAMP_LEESIN
    };
    
    enum champ my_champ = CHAMP_VAYNE;
    ```
    


### 변수 선언

![[Untitled 5 3.png|Untitled 5 3.png]]

- 변수 선언은 반드시 블럭의 시작에서만 해야함.
- 코드 중간에 사용하는 변수들은 코드 시작 부분에서 선언 후 뒤에서 대입해야 한다.