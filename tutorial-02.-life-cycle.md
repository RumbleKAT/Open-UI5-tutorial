---
description: UI5 컨트롤러의 생명주기를 알아보자
---

# Tutorial 02. Life Cycle

### 들어가면서

모든 어플리케이션은 생명 주기가 존재합니다. 저는 예전에 유니티라는 게임엔진을 이용해서 게임개발을 했던 경험이 있습니다. 게임을 만들 때 중요하게 생각해야했던 부분 중 하나가 바로 생명주기였습니다. 리소스를 로드하고 보여줘야하는 함수가 있는가 한편, 비동기로 서버와 연락하면서 그때 그때마다 값을 변동해줘야하는 함수도 존재했었지요. 이처럼 모든 프로그램에는 생성에서 소멸에 이르는 전 과정이 있고, 이에 따라 저희 개발자들은 상황에 맞는 로직을 준비해야 합니다. 

### UI5의 생명주기

이 글은 다음[ 링크](https://blogs.sap.com/2018/11/12/sapui5-controller-lifecycle-methods-explained/)에서 참고하여 작성되었습니다.

UI5는 크게 4가지의 생명주기를 지닙니다. onInit, onBeforeRendering, onAfterRendering, onExit이 바로 그것입니다. 지금부터 하나씩 알아보도록 하겠습니다. 

![UI5&#xC758; &#xC0DD;&#xBA85;&#xC8FC;&#xAE30;&#xB97C; &#xAC04;&#xB2E8;&#xD558;&#xAC8C; &#xC124;&#xBA85;&#xD558;&#xB294; &#xADF8;&#xB9BC;](.gitbook/assets/image.png)

### onInit\(\)

이 메서드는 뷰를 초기화 시킬때 호출됩니다. onBeforeRendering, onAfterRendering과 달리 딱 한번만 호출됩니다. 만약, 뷰를 표시하기 전에 뷰의 매핑 내용을 수정해야하는 경우엔 이 메서드에서 수정할수 있습니다. F5 새로고침을 할때마다 함수 실행 가능합니다.

### onBeforeRendering\(\)

이 메서드는 뷰가 렌더링 될 때마다 호출됩니다. DOM Tree에 HTML 코드가 배치되기 전, 즉 화면 렌더링이 되기 전에 호출되는 메서드입니다. 다시 렌더링하기 전에 정리작업을 수행하는데 사용할 수 있습니다.

### onAfterRendering\(\)

이 메소드는 HTML이 DOM Tree에 배치 된 후 View가 렌더링 될 때마다 호출됩니다. 화면 렌더링이 완료된 후 DOM에 추가 변경 사항을 적용하는 데 사용할 수 있습니다.

### onExit\(\)

이 메소드는 뷰가 삭제되면 호출됩니다. 컨트롤러는 이 메서드를 통해 소멸 작업을 수행해야합니다. onBeforeRendering 및 onAfterRendering와 달리 View 인스턴스 당 한 번만 호출됩니다. \(또한, onInit\(\)처럼  새로고침을 할때마다 실행이 되지 않습니다.\)

