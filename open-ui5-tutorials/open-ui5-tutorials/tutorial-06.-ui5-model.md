---
description: UI5의 데이터 영역을 다루는 Model에 대해 알아봅시다.
---

# Tutorial 06. UI5 Model

## 들어가면서

UI5는 Model View Controller의 MVC 패턴을 사용합니다. MVC의 특징은 View는 화면단의 로직을 Model은 화면에 필요한 데이터를, 그리고 Controller는 View와 Model 두영역을 다루는 코드로 구성되어 있습니다. UI5에선 View 단에 필요한 Model을 XML, JSON 형태로 선언하고, 이를 필요할 때마다 꺼내서 쓸 수 있습니다. \(주로 JSON 형태로 선언이 많이 사용됩니다.\)

## 활용 예시

### JSONModel

아래 예시는 JSONModel을 사용해서 Model을 UI5 View에 선언하고, Text 구문에 이를 적용하는 구문입니다.

```javascript
<script>
sap.ui.getCore().attachInit(function(){
    var oModel = new sap.ui.model.json.JSONModel({
        name : "Harry"
    });

    sap.ui.getCore().setModel(oModel);
    // viewController 안에서 이 구문도 가능합니다 => this.getView().setModel(oModel);
    new sap.m.Text({ text : "{/name}"}).placeAt("content");
});
</script>
```

### XMLModel

아래 예시는 XMLModel을 사용해서 Model을 선언하는 예시입니다.

```javascript
var testdata = "<?xml version=\"1.0\"?><some><xml>data</xml></some>";
var oModel = new sap.ui.model.xml.XMLModel();
oModel.setXML(testdata);
sap.ui.getCore().setModel(oModel);
```

## 활용예시-UI5 Tabel List 

본격적으로 UI5 Model을 사용하기 위해서 Table에 해당 내용을 보여주는 예시를 들어보겠습니다. 이번 예시를 위해서 odata.org에서 제공하는 TripPinServiceRW 엔티티를 활용하겠습니다. 이는 odata.org에서 실제 odata를 사용할 때, CRUD \(create, read, update, delete\) 기능을 활용할 수 있게 제공하는 샘플입니다. TripPinServiceRW 엔티티 샘플의 리스트를 받아서 테이블에 그려주는 예시를 구현하는 것이 이번 튜토리얼의 목표입니다.

### 디렉토리 구성

```javascript
directory
    Webapp
      |----- Views
      |        |------ main.controller.js
      |        |------ main.view.xml
      |
      |------ index.html
      
```

디렉토리 구성은 다음과 같습니다. 이전 튜토리얼에서 보셨던 예제와 비슷하게 구성하시면 됩니다. main.controller.js에서는 특정 링크에 리퀘스트를 호출하고, 리턴된 값을 화면에 사용할 수 있는 모델 데이터로 변환할 것입니다. main.view.xml에서는 UI5에서 제공하는 table 태그를 활용하여 테이블 형태에 맞춰서 화면을 구성하는 코드를 작성할 것입니다.



