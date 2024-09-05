# 2 Blink - Delay

- [2 Blink - Delay](#2-blink---delay)
  - [main 함수](#main-함수)
  - [Reference](#reference)

## main 함수
```c
int main(void)
{
  while (1)
  {
    HAL_GPIO_TogglePin(LED_GREEN);
    HAL_Delay(3000);
  }
}
```
윗 줄의 코드를 3초 실행하고, 3초동안은 코드를 실행하지 않음으로 blink 구현

> 간단하지만, 정확도와 멀티태스킹이 불가능한 단순한 코드! 타이머를 사용하면 더 정확하게 할 수 있다.

## Reference
[Step2 Blink LED](https://wiki.st.com/stm32mcu/wiki/STM32StepByStep:Step2_Blink_LED)
