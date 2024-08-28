# [c++] name mangling

C++은 함수 오버로딩을 지원함.
### Function Overloading
C++ 컴파일러는 객체 코드를 생성할 때 서로 다른 함수를 구분함 `(같은 이름의 함수, 다른 파라미터)`

### Name Mangling
네임 맹글링은 C++ 컴파일러가 객체 코드에서 함수에 대한 고유 이름을 생성하는 프로세스 (C++이 함수 오버로딩을 지원하기 때문에 컴파일러가 오버로드된 함수를 구분하는 방법이 필요하기 때문)

컴파일러는 객체 코드를 생성할 때 함수 이름을 "맹글링"하여 함수의 매개변수 (예: 인수의 개수와 타입)에 대한 정보를 이름으로 인코딩함 이 맹글링을 통해 링커는 서로 다른 오버로드된 함수를 구분할 수 있음

예를 들어, 다음과 같은 두 개의 오버로드된 함수가 있다면:

```c++
void foo(int);
void foo(double);
```
컴파일러는 이 이름을 다음과 같이 맹글링할 수 있음

- void foo(int) -> foo_int
- void foo(double) -> foo_double

### 왜 네임 맹글링이 필요한가?
- 함수 오버로딩: 네임 맹글링은 링커가 동일한 이름을 가진 하지만 다른 매개변수를 사용하는 여러 함수를 구분할 수 있도록 해줌
- 네임스페이스: 네임 맹글링은 또한 네임스페이스와 연결을 관리하는 데 도움이 되어 서로 다른 범위와 라이브러리가 충돌없이 동일한 이름의 함수를 가질 수 있도록 함

### 컴파일러별 네임 맹글링 규칙
|Compiler|void h(int)|void h(int, char)|void h(void)|
|---|---|---|---|
|Intel C++ 8.0 for Linux<br>HP aC++ A.05.55 IA-64<br>IAR EWARM C++<br>GCC 3.x and higher<br>Clang 1.x and higher|_Z1hi|_Z1hic|_Z1hv|
|GCC 2.9.x<br>HP aC++ A.03.45 PA-RISC|	h__Fi|h__Fic|h__Fv|
|Microsoft Visual C++ v6-v10 (mangling details)<br>Digital Mars C++|?h@@YAXH@Z|?h@@YAXHD@Z|?h@@YAXXZ|
|Borland C++ v3.1|@h$qi|@h$qizc|@h$qv|
|OpenVMS C++ v6.5 (ARM mode)|H__XI|H__XIC|H__XV|
|OpenVMS C++ v6.5 (ANSI mode)|		|CXX$__7H__FIC26CDH77|CXX$__7H__FV2CB06E8|
|OpenVMS C++ X7.1 IA-64|CXX$_Z1HI2DSQ26A|CXX$_Z1HIC2NP3LI4|CXX$_Z1HV0BCA19V|
|SunPro CC|__1cBh6Fi_v_|__1cBh6Fic_v_|__1cBh6F_v_|
|Tru64 C++ v6.5 (ARM mode)|h__Xi|h__Xic|h__Xv|
|Tru64 C++ v6.5 (ANSI mode)|__7h__Fi|__7h__Fic|__7h__Fv|
|Watcom C++ 10.6|W?h$n(i)v|W?h$n(ia)v|W?h$n()v|

### Demangling
디버깅을 돕기 위해 맹글링된 이름을 원래 형태로 되돌리는 "디맹글링" 도구와 라이브러리가 존재함. 이를 통해 생성된 심볼을 더 쉽게 이해할 수 있음

### Summary
네임 맹글링은 함수 오버로딩과 같은 기능을 활성화하고 C++ 프로그램에서 올바른 연결과 범위를 유지하는 데 중요

### Reference
[WIKIPEDIA - Name Mangling](https://en.wikipedia.org/wiki/Name_mangling)
