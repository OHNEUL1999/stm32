# 5 Blink Button
버튼을 누르면 전원 off - blink(1sec) - blink(3sec) - on 을 반복하도록 하려고 한다.

- [5 Blink Button](#5-blink-button)
  - [ENUM](#enum)
  - [Function](#function)
  - [Interrupter](#interrupter)
  - [Main](#main)

## ENUM
`led_control.h` 파일을 만들어 이넘을 사용한다.
```c
/*
 * led_control.h
 *
 *  Created on: Sep 11, 2024
 *      Author: OYJ
 */

#ifndef SRC_LED_CONTROL_H_
#define SRC_LED_CONTROL_H_

#include "main.h"  // GPIO 및 BSP 관련 헤더 파일 포함

typedef enum {
    LED_OFF = 0,
    LED_BLINK_1SEC,
    LED_BLINK_3SEC,
    LED_ON
} LED_State;

extern LED_State led_state;  // LED 상태를 외부에서 참조할 수 있도록 선언

void Control_LED(void);  // LED 제어 함수의 프로토타입

#endif /* SRC_LED_CONTROL_H_ */
```

## Function
`led_control.c` 파일을 만들어 함수를 정의했다.
```c
/*
 * led_control.c
 *
 *  Created on: Sep 11, 2024
 *      Author: OYJ
 */

#include "led_control.h"

LED_State led_state = LED_OFF;  // LED 상태를 초기화

void Control_LED(void) {
    static uint32_t last_tick = 0;
    static uint32_t blink_interval = 1000;  // 기본 blink interval은 1초 (1000ms)

    uint32_t current_tick = HAL_GetTick();  // 현재 밀리초 단위의 시간 값을 얻습니다.

    switch (led_state) {
        case LED_OFF:
            BSP_LED_Off(LED_GREEN);
            break;

        case LED_BLINK_1SEC:
            blink_interval = 1000;  // 1초 (1000ms)
            if (current_tick - last_tick >= blink_interval) {
                BSP_LED_Toggle(LED_GREEN);
                last_tick = current_tick;
            }
            break;

        case LED_BLINK_3SEC:
            blink_interval = 3000;  // 3초 (3000ms)
            if (current_tick - last_tick >= blink_interval) {
                BSP_LED_Toggle(LED_GREEN);
                last_tick = current_tick;
            }
            break;

        case LED_ON:
            BSP_LED_On(LED_GREEN);
            break;
    }
}
```
앞서 해오던 방식인 인터럽트에서 blink를 설정하는게 아닌, 함수에서 설정한다. 그리고 `Tick`을 사용하였다.

## Interrupter
`stm32c0xx_it.c` 파일에서 버튼에 따른 이넘을 인식하도록 설정하였다. 이넘을 사용하기 위해 `led_control.h`파일을 포함하였다.
```c

#include "led_control.h"
/*중략*/

/**
  * @brief This function handles EXTI line 4 to 15 interrupts.
  */
void EXTI4_15_IRQHandler(void)
{
  /* USER CODE BEGIN EXTI4_15_IRQn 0 */
	if (__HAL_GPIO_EXTI_GET_IT(GPIO_PIN_13) != RESET) // 예: GPIO_PIN_13이 눌렸는지 확인
	{
		__HAL_GPIO_EXTI_CLEAR_IT(GPIO_PIN_13);  // 인터럽트 플래그 클리어
		// LED 상태를 변경합니다.
		led_state = (LED_State)((led_state + 1) % 4); // 0, 1, 2, 3을 순환
	}
  /* USER CODE END EXTI4_15_IRQn 0 */
  BSP_PB_IRQHandler(BUTTON_USER);
  /* USER CODE BEGIN EXTI4_15_IRQn 1 */

  /* USER CODE END EXTI4_15_IRQn 1 */
}
```

## Main
main함수 내의 while(1)문에서 함수를 호출해서 사용한다.
```c
while (1)
  {

    Control_LED();

    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
  }
```
