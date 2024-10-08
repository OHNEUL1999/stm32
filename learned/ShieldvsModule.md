# Shield vs Module

- [Shield vs Module](#shield-vs-module)
  - [쉴드 (Shield)](#쉴드-shield)
  - [모듈 (Module)](#모듈-module)
  - [Conclusion](#conclusion)

## 쉴드 (Shield)

마이크로컨트롤러에 직접 연결하여 다양한 기능을 확장할 수 있도록 설계된 확장 보드(컴퓨터에 사운드 카드나 그래픽 카드를 추가하는 것과 비슷)

- `표준화된 핀 배치`: 대부분의 쉴드는 Arduino와 호환되는 표준 핀 배치를 사용하여 다양한 STM32 보드와 호환됩니다.
- `간편한 사용`: 쉴드를 연결하고 필요한 라이브러리를 설치하면 바로 사용할 수 있습니다.
- `다양한 기능`: LCD, 키패드, 센서, 통신 모듈 등 다양한 기능을 제공하는 쉴드가 존재합니다.
- `빠른 개발`: 쉴드를 사용하면 하드웨어 설계 시간을 단축하고 빠르게 프로토타입을 제작할 수 있습니다.
- `유연성 부족`: 쉴드는 특정 기능에 최적화되어 있어 다른 기능을 추가하기 어려울 수 있습니다.
- `높은 비용`: 다양한 부품을 포함하고 있어 비교적 비쌀 수 있습니다.

## 모듈 (Module)

특정 기능을 수행하는 독립적인 회로 기판. 쉴드보다 더 작고 간단한 구조이며, 다양한 인터페이스를 통해 다른 시스템과 연결됨.

- `독립성`: 다른 보드에 의존하지 않고 단독으로 작동할 수 있습니다.
- `맞춤형 설계`: 원하는 기능을 구현하기 위해 모듈을 직접 설계하거나 수정할 수 있습니다.
- `개발 시간`: 쉴드에 비해 개발 시간이 더 오래 걸릴 수 있습니다.
- `전문 지식`: 모듈을 사용하려면 회로 설계 및 프로그래밍에 대한 지식이 필요할 수 있습니다.
- `높은 유연성`: 쉴드보다 더 작고 간단한 구조이므로 다양한 시스템에 쉽게 통합할 수 있습니다.
- `저렴한 비용`: 필요한 기능만 포함하여 비용을 절감할 수 있습니다.

## Conclusion

|특징|Shield|Module|
|---|---|---|
|정의|확장 보드|독립적인 회로 기판|
|기능|다양한 기능을 통합|특정 기능 수행|
|인터페이스|표준 핀 배치|다양한 인터페이스|
|유연성|낮음|높음|
|개발 시간|짧음|길음|
|비용|비쌈|저렴함|
