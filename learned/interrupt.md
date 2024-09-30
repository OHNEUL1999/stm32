# interrupt

- [interrupt](#interrupt)
  - [Polling vs Interrupt](#polling-vs-interrupt)
  - [Reference](#reference)

## Polling vs Interrupt
|[Interrupt](#interrupt)||[Polling](polling.md)|
|---|---|---|
|CPU에 주의가 필요하다는 알림 전송|`설명`|CPU가 장치의 상태를 지속적으로 확인하여, 장치에 CPU의 주의가 필요한지 확인하는 과정|
|HW 메커니즘|`구분`|프로토콜|
|인터럽트 핸들러|`서비스 주체`|CPU|
|인터럽트 요청|`서비스 시점`|command-ready bit|
|장치가 서비스를 요구할 때만|`CPU의 사용`|시간 간격 확인을 위해 언제나 사용됨|
|절약함|`CPU Cycle`|낭비함|
|언제든지 가능|`발생 시점`|일정한 시간 간격|
|장비가 CPU를 방해하면|`언제 비효율적인가`|CPU가 서비스할 준비가 된 장치를 거의 찾지 못하면|

> interrupt는 `HW mechanism으로 조건부 작동`이지만, polling은 `항상 체크`하다가 작동시킴

## Reference
