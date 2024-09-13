# Variables

- [Variables](#variables)
  - [매개변수?](#매개변수)
  - [Local vs Global vs Static](#local-vs-global-vs-static)
  - [Static Local](#static-local)
  - [Static Global](#static-global)
  - [Conclusion](#conclusion)

## 매개변수?
매개변수에 static을 붙이더라도 매개변수는 정적 변수가 되지 않으며, 값이 유지되지 않음

```c
void func_1(static int a)
{
    print("%d", a);
    a++;
}
```
위와 같이 매개변수로 사용하면 `warning C4042: 'a': 저장소 클래스가 잘못되었습니다.`

## Local vs Global vs Static
||지역 변수 (Local)|전역 변수(Global)|정적 변수(Static)|
|---|---|---|---|
|디폴트 초기값|쓰레기값|0|0(초기화는 1번만)|
|사용 범위|중괄호 내부|프로그램 전체|**Local** : `중괄호 내부`<br>**Global** : `선언된 소스 파일`|
|메모리 생성 시점|중괄호 내부|프로그램 시작|프로그램 시작|
|메모리 소멸 시점|중괄호 탈출|프로그램 종료|프로그램 종료|
|메모리 할당 공간|Stack|Data영역(초기화)<br>BSS영역(비초기화)|Data영역(초기화)<br>BSS영역(비초기화)

## Static Local
```c
#include <stdio.h>

void test() {
    static int a = 10; // static 지역 변수 a 선언
    a++;
    printf("%d\n", a);
}

int main() {
    test();
    test();
    test();
    return 0;
}
```

출력 : 11
12
13

`함수는 끝났지만, 변수는 남아있어 값이 계속 증가함.`

## Static Global
```c
// file1.c
#include <stdio.h>

static int global_counter = 0; // static 전역 변수

void increment_counter() {
    global_counter++;
}

void print_counter() {
    printf("Counter: %d\n", global_counter);
}
```

```c
// file2.c
#include <stdio.h>

extern void increment_counter();
extern void print_counter();

int main() {
    increment_counter();
    increment_counter();
    print_counter(); // 출력: Counter: 2
    return 0;
}
```
**file1.c**

`global_counter` 접근 가능

**file2.c**

`global_counter` 접근 불가능<br>
`increment_counter()`와 `print_counter()` 함수를 통해서만 값을 조작하거나 조회 가능

## Conclusion
`값이 유지돼야 하지만, 유지보수 등등 때문에 전역변수도, 지역변수도 마땅치 않을 때` static 변수를 사용한다.

`값이 유지 된다는 전역 변수의 특징`과 `지정된 스코프에서만 접근 가능하다는 지역 변수의 특징`이 섞인 것.