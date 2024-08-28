# [c/c++] extern

## extern "C"
**Name Mangling**이란 C++ 컴파일러가 함수 이름에 클래스 정보, 인자 타입 등을 추가하여 고유한 이름을 생성하는 과정입니다. 이렇게 생성된 이름은 C 컴파일러가 인식하지 못하기 때문에 C에서 C++ 함수를 호출할 때 문제가 발생할 수 있습니다. **extern "C"** 키워드는 이러한 문제를 해결하기 위해 C++ 컴파일러에게 C 스타일로 이름을 변환하도록 지시합니다.

### extern "C"의 역할
- **C 스타일 이름 변환:** C++ 컴파일러가 함수 또는 변수의 이름을 C 스타일로 변환하여 C와 C++ 코드 간의 호환성을 높입니다.
- **C와 C++ 코드의 상호 운용성:** C로 작성된 라이브러리 함수를 C++에서 호출하거나, C++로 작성된 함수를 C에서 호출할 수 있도록 합니다.


### 예시 - C++에서 C의 변수 사용

```h
#include <string>
#include <vector>

extern "C" {
    extern int TestInt;
    extern std::string TestString;
    extern std::vector<std::int> TestVectorInt;
    extern std::vector<std::string> TestVectorString;
}
```
^^^이렇게 extern "C"로 감싸준 후 사용하면 Name Mangling되지 않고 변수를 원본 그대로 사용할 수 있다.

### 예시 - C++에서 C의 표준 라이브러리 함수 호출
```c++
#include <stdio.h>

extern "C" {
    int printf(const char *format, ...);
}

int main() {
    printf("Hello, world!\n");
    return 0;
}
```
### 예시 - C++에서 C로 작성된 라이브러리 함수 호출
```c++
// my_c_lib.h (C 헤더 파일)
void my_c_function(int a, int b);

// my_c_lib.c (C 소스 파일)
void my_c_function(int a, int b) {
    // C로 작성된 함수 로직
    printf("a = %d, b = %d\n", a, b);
}

// main.cpp (C++ 소스 파일)
#include "my_c_lib.h"

extern "C" {
    void my_c_function(int a, int b);
}

int main() {
    my_c_function(3, 4);
    return 0;
}
```
^^^ my_c_function이 C에서 작성된 함수일 경우, extern "C"를 활용해 C++컴파일러에게 함수의 이름을 C방식으로 변환하도록 지시함.

### 예시 - C에서 C++의 멤버 함수 호출
```c++
// C++ 클래스
class MyClass {
public:
    void my_method(int x) {
        // ...
    }
};

// C 파일에서 C++ 클래스 멤버 함수 호출 (주의: 일반적으로 권장되지 않음)(C와 C++의 예외 처리 방식이 다르기 때문)
extern "C" void call_cpp_method(MyClass* obj, int x) {
    obj->my_method(x);
}
```

### 예시 - 템플릿 함수
```c++
// 템플릿 함수
template <typename T>
void my_template_function(T a, T b) {
    // ...
}

// C에서 사용할 수 있도록 특정 타입에 대해 인스턴스화
extern "C" void my_int_function(int a, int b) {
    my_template_function<int>(a, b);
}
```
^^^템플릿 함수는 C에서는 사용할 수 없기 때문에, 특정 타입에 대해 미리 인스턴스화하여 C에서 호출할 수 있도록 해야 함

### 예시 - 네임스페이스
```c++
namespace my_namespace {
    extern "C" void my_function(int a);
}
```
^^^ 네임스페이스 내에서 extern "C"를 사용하여 이름 충돌을 방지하고 코드를 조직화할 수 있음

### 유의사항
- `extern "C"의 위치`: extern "C"는 함수 선언이나 변수 선언 바로 앞에 위치
- `.h` : C함수가 선언된 헤더 파일을 포함해야 함
- `link 설정` : 컴파일 시 C라이브러리를 링크해야 함
- `namespace` : C++의 네임스페이스와 함께 사용 가능

- extern "C"를 남용하면 코드의 `가독성이 저하`될 수 있음
- C++ 클래스 멤버 함수를 `C에서 직접 호출하는 것은 일반적으로 비권장`됨

## Epilogue
I've used OOP languages(python, java, c++). class, capsulization etc were familiar for me, but not in PP. I think i should learn about differences and usage to use c language well!

## Reference
- [Microsoft Learn extern(C++)](https://learn.microsoft.com/ko-kr/cpp/cpp/extern-cpp?view=msvc-170)
