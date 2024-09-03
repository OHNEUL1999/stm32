# 1 Toggle

`Initial Code` of `NUCLEO-C031C6`

USING : BUTTON, LD4(GREEN)

- [1 Toggle](#1-toggle)
  - [헤더 및 라이브러리](#헤더-및-라이브러리)
  - [전역 변수 선언](#전역-변수-선언)
  - [main 함수](#main-함수)
  - [시스템 클록 설정 (SystemClock\_Config)](#시스템-클록-설정-systemclock_config)
  - [GPIO 초기화](#gpio-초기화)
  - [버튼 콜백 함수 (BSP\_PB\_Callback)](#버튼-콜백-함수-bsp_pb_callback)
  - [오류 처리 (Error\_Handler)](#오류-처리-error_handler)
  - [Assert 오류 처리](#assert-오류-처리)

## 헤더 및 라이브러리
```c
#include "main.h"
```
STM32의 HAL(Hardware Abstraction Layer) 라이브러리와 관련된 설정, 함수 선언 등이 포함된 main이라는 이름의 헤더 파일을 가져옴

## 전역 변수 선언
```c
COM_InitTypeDef BspCOMInit;
__IO uint32_t BspButtonState = BUTTON_RELEASED;
```
- `COM_InitTypeDef BspCOMInit`: COM 포트의 초기화 설정을 위한 구조체(UART 통신을 설정할 때 사용)
- `__IO uint32_t BspButtonState`: 버튼의 상태를 나타내는 전역 변수(BUTTON_RELEASED와 BUTTON_PRESSED로 상태를 관리)<br>__IO는 이 변수가 하드웨어 레지스터와 연동될 수 있음을 나타냄

## main 함수
```c
int main(void)
{
  HAL_Init();
  SystemClock_Config();
  MX_GPIO_Init();
  BSP_LED_Init(LED_GREEN);
  BSP_PB_Init(BUTTON_USER, BUTTON_MODE_EXTI);

  BspCOMInit.BaudRate   = 115200;
  BspCOMInit.WordLength = COM_WORDLENGTH_8B;
  BspCOMInit.StopBits   = COM_STOPBITS_1;
  BspCOMInit.Parity     = COM_PARITY_NONE;
  BspCOMInit.HwFlowCtl  = COM_HWCONTROL_NONE;
  if (BSP_COM_Init(COM1, &BspCOMInit) != BSP_ERROR_NONE)
  {
    Error_Handler();
  }

  printf("Welcome to STM32 world !\n\r");
  BSP_LED_On(LED_GREEN);

  while (1)
  {
    if (BspButtonState == BUTTON_PRESSED)
    {
      BspButtonState = BUTTON_RELEASED;
      BSP_LED_Toggle(LED_GREEN);
    }
  }
}

```
- `HAL_Init()`: STM32 HAL 라이브러리를 초기화(기본 하드웨어 설정을 수행)
- `SystemClock_Config()`: 시스템 클록을 설정(마이크로컨트롤러의 작동 속도와 관련이 있음!)
- `MX_GPIO_Init()`: GPIO(General Purpose Input/Output) 포트를 초기화
- `BSP_LED_Init(LED_GREEN)`: LED를 초기화
- `BSP_PB_Init(BUTTON_USER, BUTTON_MODE_EXTI)`: 사용자 버튼을 초기화(버튼이 눌릴 때 인터럽트를 발생)
- `BspCOMInit`: COM1 포트를 초기화(BaudRate, WordLength, StopBits, Parity 및 HwFlowCtl을 설정) printf를 통해 메시지를 COM 포트로 전송합니다.
- `while (1)`: 무한 루프를 돌면서 버튼 상태를 체크. 버튼이 눌리면 LED를 토글

## 시스템 클록 설정 (SystemClock_Config)
```c
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
  RCC_OscInitStruct.HSEState = RCC_HSE_ON;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }

  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK|RCC_CLOCKTYPE_PCLK1;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_HSE;
  RCC_ClkInitStruct.SYSCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_APB1_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_1) != HAL_OK)
  {
    Error_Handler();
  }
}
```
- `RCC_OscInitTypeDef와 RCC_ClkInitTypeDef 구조체`를 사용하여 시스템 클록을 설정
- `RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE`는 외부 고속 오실레이터(High-Speed External)를 사용하도록 설정
- `RCC_ClkInitStruct`에서 시스템 클록 소스와 분주비를 설정. HSE를 클록 소스로 사용하며, 분주비는 1로 설정

## GPIO 초기화
```c
static void MX_GPIO_Init(void)
{
  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOF_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();
}
```
- GPIO 포트 C, F, A의 `클록을 활성화`
- GPIO 포트는 입력 및 출력 핀을 설정할 때 필요

## 버튼 콜백 함수 (BSP_PB_Callback)
```c
void BSP_PB_Callback(Button_TypeDef Button)
{
  if (Button == BUTTON_USER)
  {
    BspButtonState = BUTTON_PRESSED;
  }
}
```
- 버튼이 눌렸을 때 호출되는 콜백 함수(버튼 상태를 BUTTON_PRESSED로 업데이트)

## 오류 처리 (Error_Handler)
```c
void Error_Handler(void)
{
  __disable_irq();
  while (1)
  {
  }
}
```
- 오류가 발생했을 때 호출되는 함수. 
- 시스템을 무한 루프로 돌리며, 일반적으로 디버깅 용도로 사용.

## Assert 오류 처리
```c
#ifdef  USE_FULL_ASSERT
void assert_failed(uint8_t *file, uint32_t line)
{
  // 사용자 정의 assert 실패 처리 코드 추가
}
#endif /* USE_FULL_ASSERT */
```
- Assert 실패 시 호출되는 함수
- 보통 디버깅 시 코드의 오류를 찾는 데 사용
