# CMSIS
Debugging중 cmsis error가 떠서 알아보려고 한다.

- [CMSIS](#cmsis)
  - [CMSIS란](#cmsis란)
  - [Versions](#versions)
  - [CMSIS Error](#cmsis-error)
  - [Coding Rules](#coding-rules)
  - [Reference](#reference)

## CMSIS란
CMSIS (Common Microcontroller Software Interface Standard) 

API, SW 구성요소, 도구, 워크플로우의 모음이다.

CMSIS가 있어서 재사용, MCU개발자의 러닝커브 감소, 디버깅 시간 단축, 새 어플리캐이션 개발 시간 단축에 도움을 줄 수 있다.

## Versions
2024/09/12기준 최신 버전은 `6.1.0`

- compiler가 자동선택?
  - CMSIS는 보통 특정 컴파일러에 의존하지 않으며, ARM의 Keil MDK와 같은 IDE와 함께 사용되지만, 코드에 CMSIS가 직접적으로 의존하지 않는 한 컴파일러가 자동으로 CMSIS의 버전을 선택하지는 않습니다. CMSIS는 보드와 관련된 라이브러리와 드라이버에 대한 스펙을 제공하지만, IDE나 툴체인에서의 자동 업데이트는 사용자 설정에 따라 달라질 수 있습니다.
  - 즉, 버전에 맞게 잘 선택해두어야 한다! STM32CubeIDE에서는 최신버전으로 자동 생성이 되는 듯 하다.
- 보드별 버전
  - 보드에 맞는 버전이 있을 수 있으니 유의하기\
- [Coding Rules](#coding-rules)를 보면 MISRA 따르는데, 요구하지는 않는다,,?
  - CMSIS가 MISRA를 요구하지는 않음. 그런데 MISRA가 코드 품질/안전성을 보장하기 때문에 '필수'는 아니지만 따르는게 좋음

## CMSIS Error
드라이버에서 에러가 났었는데 `CMSIS가 제대로 보드에 로드되지 않은 게` 문제였다.

## Coding Rules
The CMSIS uses the following essential coding rules and conventions:

- Compliant with ANSI C (C99) and C++ (C++03).
- Uses ANSI C standard data types defined in **<stdint.h>**.
- Variables and parameters have a complete data type.
- Expressions for #define constants are enclosed in parenthesis.
- Conforms to MISRA 2012 (but does not claim MISRA compliance). MISRA rule violations are documented.

In addition, the CMSIS recommends the following conventions for identifiers:

- CAPITAL names to identify Core Registers, Peripheral Registers, and CPU Instructions.
- CamelCase names to identify function names and interrupt functions.
- Namespace_ prefixes avoid clashes with user identifiers and provide functional groups (i.e. for peripherals, RTOS, or DSP Library).

The CMSIS is documented within the source files with:

- Comments that use the C or C++ style.
- [`Doxygen`](Doxygen.md) compliant function comments that provide:
  - brief function overview.
  - detailed description of the function.
  - detailed parameter explanation.
  - detailed information about return values.

## Reference
- [CMSIS Documentation](https://arm-software.github.io/CMSIS_6/latest/General/index.html)
