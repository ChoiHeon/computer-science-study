# Static Class vs Singleton

**[Static Class와 Singleton 비교]**

| 차이점          | Static Class                                                 | Singleton                |
| --------------- | ------------------------------------------------------------ | ------------------------ |
| 원리            | 인스턴스 생성 X (생성자 존재 X)                              | 하나의 인스턴스만 사용   |
| 인터페이스 구현 | 불가능                                                       | 거눙                     |
| follow OOP      | OOP에 따르기보단 함수에 가까움                               | OOP를 따름               |
| override        | 불가능                                                       | 가능                     |
| load            | static binding in compile-timestatic binding in compile-time | 필요할 때 lazy load 가능 |
| performance     | 빠름                                                         | Static Class보단 느림    |
| test            | hard to mock and test                                        | 쉬움                     |
| 저장 위치       | Stack                                                        | Heap                     |

* Singleton이 인터페이스를 구현할 수 있으므로 OOP와 다형성 관점에서 이점이 있음
* 만약 Singleton이 별도로 상태를 갖지 않고 global access를 제공한다면 속도 이점에 따라 Static Class를 고려