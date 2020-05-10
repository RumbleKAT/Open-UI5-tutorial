---
description: 함수를 효율적으로 다루는 방법. 모듈
---

# Tutorial 05. Module

## 들어가면서

자바스크립트 프로그램은 꽤 작게 시작되었습니다. 초기에 사용된 대부분의 스크립트는 독립적인 작업을 수행하며, 일반적으로 큰 스크립트가 필요하지 않았습니다. 요즘 모던 자바스크립트에선 브라우저 제어로직이나 Node.js와 같은 브라우저 외의 영역에서도 사용가능합니다.

따라서 자바스크립트는 필요에 따라 라이브러리를 가져오고, 이를 별도의 모듈로 분할하기 위한 매커니즘을 도입합니다. node.js가 그 예시이지요.

## UI5 모듈 제어

SAPUI5 프레임워크는 모듈화되고 이해하기 쉬운 자바스크립트 어플리케이션을 지원하기 위해 설계되었습니다. 이는해당 기능이 필요로 될때 즉시 로딩될 수 있도록, 더 작은 기능 단위로 기능 분화를 가능하게 합니다.

저희는 이전에 UI5 Controller.js에서 sap.ui.define이라는 구문을 사용하고 안에 해당 의존성을 불러오는 작업을 진행했습니다

아래 소스를 보시면 sap/ui/core/mvc/Controller경로에 있는 Controller.js 라이브러리를 불러온 후, 이에 대한 메모리를 할당하는 과정을 볼 수 있습니다.

```javascript
sap.ui.define([
  "sap/ui/core/mvc/Controller",    
],function(Controller){
    "use strict"
    return Controller.extend("view.Main",{

      onAfterRendering : function(){
         var btn1 = this.getView().byId("btn2");
         btn1.addStyleClass('btnStyleBlue');
      },
      onClicked : function(Event){
        alert("button Clicked!");
      }
    });
});

```

