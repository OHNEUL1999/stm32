# polling

- [polling](#polling)
  - [Polling vs Interrupt](#polling-vs-interrupt)
  - [Reference](#reference)

폴링은 하나의 장치(또는 프로그램)가 충돌 회피 또는 동기화 처리 등을 목적으로 `다른 장치(또는 프로그램)의 상태를 주기적으로 검사`하여 일정한 `조건을 만족할 때 송수진 등의 자료처리`를 하는 방식

주로 여러 개의 장치가 동일 회선을 사용하는 상황에서 사용한다
<br>(ex. 버스, 멀티포인트 등)

서버의 제어 장치는 순차적으로 각 단말 장치에 회선을 사용하기 원하는지 물어본다. 클라이언트 프로그램이 동기 활동으로 외부 장치의 상태를 적극적으로 샘플링하는 것을 의미한다.

I/O 측면에서 가장 자주 사용된다.

## Polling vs Interrupt

|[Interrupt](interrupt.md)||[Polling](#polling)|
|---|---|---|
|CPU에 주의가 필요하다는 알림 전송|`설명`|CPU가 장치의 상태를 지속적으로 확인하여, 장치에 CPU의 주의가 필요한지 확인하는 과정|
|HW 메커니즘|`구분`|프로토콜|
|인터럽트 핸들러|`서비스 주체`|CPU|
|인터럽트 요청|`서비스 시점`|command-ready bit|
|장치가 서비스를 요구할 때만|`CPU의 사용`|시간 간격 확인을 위해 언제나 사용됨|
|절약함|`CPU Cycle`|낭비함|
|언제든지 가능|`발생 시점`|일정한 시간 간격|
|장비가 CPU를 방해하면|`언제 비효율적인가`|CPU가 서비스할 준비가 된 장치를 거의 찾지 못하면|

> interrupt는 HW mechanism으로 조건부 작동이지만, polling은 항상 체크하다가 작동시킴

## Reference

- [tutorialspoint - Difference Between Interrupt and Polling in OS](https://www.tutorialspoint.com/difference-between-interrupt-and-polling-in-os#:~:text=An%20interrupt%20is%20a%20process,it%20needs%20the%20CPU's%20attention.)
