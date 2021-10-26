# SDK & API

### SDK

* SDK (Software Development Kit)은 특정 플랫폼에서 개발자들이 애플리케이션을 개발하기 위해서 사용하는 툴 또는 프로그램의 집합입니다.
* 일반적으로 라이브러리, 문서 (document), 샘플 코드, 프로세스 그리고 개발자들의 사용하거나 앱에 통합시킬 수 있도록 가이드를 가지고 있습니다.

##### 좋은 SDK의 특징

* 다른 개발자들이 사용하기 쉬어야 함
* 어떻게 코드가 동작하는지에 대한 상세한 설명
* 다른 앱에 추가할만한 충분한 기능성
* 모바일 디바이스의 CPU, 배터리, 데이터 소비량에 나쁜 영향이 없어야 함
* 다른 SDK들과 충돌이 없어야 함



### API

* API (Application Protocol(Programming) Interface)은 간단하게 프로그램들이 서로 상호작용하는 것을 도와주는 매개체로 정의할 수 있습니다.
* 사용자에게 명령 목록을 제공하고 명령을 받으면 앱과 상호작용하여 명령에 대한 값을 사용자가에 전달합니다.

##### API의 이점

1. private API를 사용함으로서 개발자들이 애플리케이션 코드 작성 방법을 표준화할 수 있습니다.
   * Private API (내부 API): 회사의 개발자가 자체 제품과 서비스를 위해 작성한 API입니다. 제 3자에게 노출되지 않습니다.
   * 표준화함으로서 빠른 프로세스 처리가 가능하고 소프트웨어 통합시 협업을 용이하게 할 수 있습니다.
2. public API 또는 partner API를 통해 최소한의 시간과 비용으로 개발할 수 있습니다.
   * API와 자신의 회사 서비스의 연결 및 연동이 필요할 수 있습니다.

##### API 단점

* 해커의 최우선 타켓이 될 수 있습니다.

* API가 해킹되면 다른 애플리케이션이나 시스템에도 악영향이 갈 수 있습니다.
* man-in-the-middle attack에 사용될 수 있습니다.
  * man-in-the-middle attack: 네트워크 통신을 조작하여 통신 내용을 도청하거나 조작하는 공격 기법



### API vs SDK

* API => mailman
  * 앱의 요청을 다른 소프트웨어에 전달하고 결과를 다시 앱에 전달합니다.
  * ex) Goggle Calender와 여행 앱 사이의 커뮤니케이션을 위한 API가 있다면, 여행 앱을 통해 숙소를 예약했을 때 Calender에 동기화될 수 있습니다.
* SDK => post office + hardware store
  * 다른 소프트웨어와의 커뮤니케이션 방법 (APIs)과 새로운 애플리케이션을 만들기 위한 재료 (code libraries, debugging facilities, technical notes, tutorials, and documentation, etc)를 가질 수 있습니다.
* 결론
  1. API는 커뮤케이션을 위해 존재하며 SDK는 개발을 위해 존재
  2. SDK는 여러 개의 API를 가질 수 있음 (SDK가 더 포괄적인 개념)





