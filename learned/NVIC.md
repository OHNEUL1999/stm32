# NVIC

- [NVIC](#nvic)
  - [NVIC : Nested Vectored Interrupt Controller](#nvic--nested-vectored-interrupt-controller)
    - [NVIC Feature(특징)](#nvic-feature특징)
    - [인터럽트 처리 순서](#인터럽트-처리-순서)
  - [Reference](#reference)

## NVIC : Nested Vectored Interrupt Controller

중첩 벡터형 인터럽트 제어기, 특수 PIC(Programmable Interrupt Controller).

- `모든 인터럽트(core exceptions 포함)를 제어`한다.
- 모든 인터럽트 & 예외에 대한 `우선 순위를 결정하고, 순위에 따라 순차적으로 처리`한다.

### NVIC Feature(특징)

- Fast response to interrupt requests
- Dynamic reprioritization of interrupts
- Dynamic relocation of interrupt vector table

### 인터럽트 처리 순서

1. `Stacking 스택킹`

   인터럽트 처리에 사용될 레지스터의 현재 값(PC, Status reg) 등을 스택에 저장

2. `Vector Fetch 벡터 인출`

    벡터 테이블에서 인터럽트 핸들러의 시작 주소를 Fetch

3. `Register Update 레지스터 업데이트`

    인터럽트 핸들러로 진입할 때 관련된 레지스터 업데이트

4. `Run Interrupt Service Routine 인터럽트 서비스 루틴 실행`

    발생된 인터럽트에 해당되는 인터럽트 서비스 루틴 실행

5. `Close Interrupt 인터럽트종료 빠져나오기`

    인터럽트 서비스 루틴 실행 끝나면, 종료 위해 `1`에서 스택에 넣었던 내용을 다시 읽어와 레지스터의 원래 값으로 복구(Unstacking)

## Reference

- [wikidocs - 마이크로프로세서 STM32F4 기초 및 응용](https://wikidocs.net/214829)
- [Life Seed - Cortex-M3 Peripheral : Nested Vectored Interrupt Controller (NVIC)](https://lifeseed.tistory.com/66)
- [네이버블로그 지금 그리고 여기 - NVIC이란 무엇일까? / 중첩 인터럽트, 벡터형 인터럽트, 테일 체이닝 인터럽트](https://blog.naver.com/ycpiglet/222112201976)
- [STM32G4 - NVIC (Revision 1.0)](https://www.st.com/resource/en/product_training/STM32G4-System-Nested_Vectored_Interrupt_Control_NVIC.pdf)
