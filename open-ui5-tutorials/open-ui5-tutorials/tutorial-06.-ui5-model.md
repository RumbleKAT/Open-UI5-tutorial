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

```javascript
var testdata = "<?xml version=\"1.0\"?><some><xml>data</xml></some>";
var oModel = new sap.ui.model.xml.XMLModel();
oModel.setXML(testdata);
sap.ui.getCore().setModel(oModel);
```



