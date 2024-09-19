# Setting

- [Setting](#setting)
  - [Circuit](#circuit)
  - [Saleae](#saleae)
    - [tick](#tick)
    - [interrupt](#interrupt)
  - [STM32CubeIDE Setting](#stm32cubeide-setting)
    - [I2C Setting](#i2c-setting)
    - [Parameter Setting](#parameter-setting)
    - [GPIO Setting](#gpio-setting)
    - [DMA Setting](#dma-setting)
    - [NVIC Setting](#nvic-setting)
  - [Reference](#reference)

## Circuit
![I2C Circuit](./images/I2C_circuit.png)

- 보드 : STM32 NUCLEO-C031C6,
- 모듈 : NUCLEO-IKSO1A1
- 디버깅 : SALEAE Logic Pro 8
---
- `0`번 채널은 모듈의 `I2C의 SDA`와 연결
- `1`번 채널은 모듈의 `II2C의 SCL`과 연결
- `3`번 채널은 stm 보드의 `LD4`로 연결해 Blinking을 확인할 수 있도록 하였다.

## Saleae
### tick
![tick level](./images/tick_level.png)

[**2.LED - 5. Blink switch by Button**](../2.%20LED/5_Blink_Button.md)에서 작성한 코드 중 버튼을 한 번 눌렀을 때(`1초 간격으로 blink`)의 상태로 두고 saleae를 측정해보면 다음과 같은 파형을 볼 수 있다.
정확히 `1초는 아니지만, 근접하게 blink되고 있음`을 확인할 수 있었다.

### interrupt
![interrupt level](./images/interrupt_level.png)

[**2. LED - 3. Blink Timer : Code in Interrupt Handler**](../2.%20LED/3_Blink_Timer(1).md)에서 작성한 코드로 측정해보았다. tick과 동일하게 `1초는 아니지만, 근접하게 blink되고 있음`을 확인할 수 있었다.

## STM32CubeIDE Setting
### I2C Setting
![I2C Setting](./images/I2C_Setting.PNG)

사진처럼 Connectivity에서 I2C를 활성화할 수 있다.

### Parameter Setting
![Parameter Setting](./images/I2C_Parameter_Settings.PNG)

Parameter중 Rise Time, Fall Time을 0으로 설정하였다.
- Rise Time : L -> H로 전압이 올라가는 데 걸리는 시간
- Fall Time : H -> L로 전압이 떨어지는 데 걸리는 시간

이 두 파라미터는 I2C통신의 속도와 신뢰성에 영향을 미친다. 일반적으로 부하에 따라 조정된다.

바로 변경값을 보기 위해서 Setting을 0으로 하면 되지만, 이 값은 `물리적으로 불가능`하다. 따라서, 100(ns)로 임의 설정하였다.

### GPIO Setting
![GPIO Setting](./images/I2C_GPIO_Settings.PNG)

SCL과 SDA의 GPIO Pull-up/Pull-down을 `Pull-up`으로, Maximum output speed를 `Very High`로 설정했다.

**Pull-up 설정**

Pull-up: GPIO 핀에 내장된 저항을 사용하여 기본 전압을 높게 유지합니다. 이 설정은 SCL과 SDA 라인이 기본적으로 HIGH 상태를 유지하게 하여, I2C 통신 시 안정적인 시작 상태를 보장합니다. I2C 프로토콜에서 SDA와 SCL 라인은 오픈 드레인(open-drain) 방식으로 동작하므로, 기본적으로 HIGH 상태를 유지하는 것이 필요합니다.

**Maximum Output Speed 설정**

Very High: GPIO의 최대 출력 속도를 매우 높게 설정한 것입니다. 이는 SCL의 주파수를 높일 수 있도록 하여, I2C 통신의 데이터 전송 속도를 증가시킵니다. 그러나 너무 높은 속도로 설정하면 신호의 rise time과 fall time이 짧아져서 전송 오류가 발생할 수 있으므로, 이를 고려해야 합니다.

### DMA Setting
![DMA Setting](./images/I2C_DMA_Settings.PNG)

SDA, SCL의 채널을 설정하였다.

### NVIC Setting
![NVIC Setting](./images/I2C_NVIC_Settings.PNG)

DMA Setting을 하면 사진과 같이 채널이 NVIC에 생성된다.

체크 박스(interrupt enabled)를 활성화하면 인터럽트를 사용하겠다는 뜻이다.

## Reference
[STM32 - Getting started with I2C](https://wiki.st.com/stm32mcu/wiki/Getting_started_with_I2C)
