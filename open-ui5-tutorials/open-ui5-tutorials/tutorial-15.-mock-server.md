---
description: Mock Server를 생성하여 샘플데이터를 만들어봅니다.
---

# Tutorial 15. Mock Server

## 들어가면서

Mock Server는 가짜 서버를 만들어서 마치 api가 있는 것처럼 사용할 수 있는 서버입니다. 이 기술은 사용가능한 OData 서비스가 없거나 개발 및 테스트를 위해 OData 백엔드 연결에 의존하고 싶지 않은 경우 이용할 수 있습니다. 즉, 프론트엔드 개발자와 백엔드 개발자가 따로 개발을 진행하고 연계에 필요한 시간을 줄일 수 있습니다.

## 파일구성

### mockServer.html

mockServer.html은 index.html과 비슷한 역할입니다. mockServer.html에선 우리는 mockserver.js를 이용하여 필요한 odata를 테스트할 수 있습니다. 

### metadata.xml

서비스 인터페이스에 대한 정보를 제공하는 xml입니다. 수동적으로 내용을 적을 필요는 없고,  브라우저로 부터 메타데이터 세부정보를 복사하여 사용할 수 있습니다. \(ex http://services.odata.org/V2/NorthWind/Northwind.svc/$metadata\)

### MockData \(JSON Files\)

Odata에 실질적인 데이터를 포함한 JSON 파일에 넣는 것으로, localService 아래 경로에 넣으면 됩니다. \(MockServer가 실행되면, JSON 파일의 배열정보가 보여집니다.\)

### mockServer.js 

MockServer  모듈은 서버를 시작함과 동시에 로딩됩니다.  실제 서버 대신에 테스트 서버가 rootURI에 서비스 되고, 전형적인 서버 반응시간을 모방하여, 1초의 딜레이를 넣을 수 있습니다. 마지막으로,  Mockserver.start\(\) 커맨드를 실행하여, mockServer를 실행할 수 있습니다.

```text
webapp
   |----- localService
   |          |-------- mockdata
   |          |-------- metadata.xml
   |          |-------- mockserver.js
   |
   |----- test
            |--------mockServer.js
```









