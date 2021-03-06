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

## 활용예시-UI5 Table List 

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

### Main.Controller.js 

```javascript
sap.ui.define([
    "sap/ui/core/mvc/Controller",
    "sap/ui/model/json/JSONModel"
  ],function(Controller,JSONModel){
      "use strict"
      return Controller.extend("views.main",{
        onInit : function(){
            this.getView().setModel(new JSONModel({}), "TripPinServiceRW");
        },
        onAfterRendering : function(){
            this.onGetRequest('https://services.odata.org/V4/(S(epugafwj5m0rg30yoxbtltlk))/TripPinServiceRW/?$format=json');
        },
        onGetRequest : function(url){
            console.log(this.getView().getModel("TripPinServiceRW").getData());
            fetch(url)
            .then((res)=>res.json())
            .then((param)=>{
                console.log(param.value);
                this.getView().setModel(new JSONModel(param.value), "TripPinServiceRW");
                console.log(this.getView().getModel("TripPinServiceRW"));
            });
        }
      });
  });
  
```

UI5는 데이터를 관리하기 위해 여러 상태 함수를 제공합니다. 위의 코드를 보시면, 브라우저에 View.xml 파일이 업데이트가 되기 전인 onAfterRendering 함수에 TripPinServiceRW 엔티티를 호출합니다. 호출된 데이터는 json 형태로 리턴되지만\(=param.value\), 아직 이 상태로 setModel을 하시면, 브라우저에 원하는 데이터가 매핑이 안되는 문제점이 있습니다. 이를 해결하기 위해 JSONModel로 해당 JSON 배열을 감싸주면, 저희는 원할하게 Model의 데이터를 View단에 보여줄 수 있습니다.

![request&#xAC00; &#xC815;&#xC0C1;&#xC801;&#xC73C;&#xB85C; &#xD638;&#xCD9C;&#xB41C; &#xD6C4;, &#xBDF0;&#xC5D0; &#xB9E4;&#xD551;&#xC774; &#xB418;&#xC5C8;&#xC744; &#xB54C; &#xB85C;&#xADF8;&#xC785;&#xB2C8;&#xB2E4;.](../../.gitbook/assets/image%20%287%29.png)

### Main.view.xml

```markup
<core:View xmlns:core="sap.ui.core"
           xmlns:mvc="sap.ui.core.mvc"
           xmlns="sap.m"
           controllerName="views.main">

    <Table items="{path : 'TripPinServiceRW>/'}">
	    <headerToolbar>
            <OverflowToolbar>
                <Title id="title" text="TripPinServiceRW"/>
            </OverflowToolbar>
        </headerToolbar>
        <columns>
            <Column>
                <Label text="name" design="Bold"/>
            </Column>
            <Column>
                <Label text="kind" design="Bold"/>
            </Column>
            <Column>
                <Label text="url" design="Bold"/>
            </Column>
        </columns>
        <items>
			<ColumnListItem>
				<cells>
            <Text text="{TripPinServiceRW>name}" />
            <Text text="{TripPinServiceRW>kind}" />
            <Text text="{TripPinServiceRW>url}" />
        </cells>
			</ColumnListItem>
    	</items>
    </Table>
    
</core:View>
```

UI5 Table에 데이터를 매핑하기 위해선 items에 해당 모델명을 작성해야합니다. UI5에서 화면에 모델을 매핑하는 방법은 여러 방법이 있습니다. 위의 코드의 방식은 Controller.js 파일에 TripPinServiceRW이라는 모델명에 매핑되는 JSON 배열 데이터를 가져오고, 배열의 Property에 해당하는 값을 cells에 하나씩 매핑하여 보여주는 방식입니다. 반면, 다른 방식으로 TripPinServiceRW이라는 모델명을 선언하지 않고 데이터를 가져오는 방식도 있습니다. \(=아래 코드를 참고하세요\)

### 모델명을 선언하지 않고 XML View에 리스트 매핑

#### Main.view.xml

```markup
<core:View xmlns:core="sap.ui.core"
           xmlns:mvc="sap.ui.core.mvc"
           xmlns="sap.m"
           controllerName="views.main">

    <Table items="{/value}">
	    <headerToolbar>
            <OverflowToolbar>
                <Title id="title" text="TripPinServiceRW"/>
            </OverflowToolbar>
        </headerToolbar>
        <columns>
            <Column>
                <Label text="name" design="Bold"/>
            </Column>
            <Column>
                <Label text="kind" design="Bold"/>
            </Column>
            <Column>
                <Label text="url" design="Bold"/>
            </Column>
        </columns>
        <items>
			<ColumnListItem>
				<cells>
            <Text text="{name}" />
            <Text text="{kind}" />
            <Text text="{url}" />
        </cells>
			</ColumnListItem>
    	</items>
    </Table>
    
</core:View>
```

#### Main.controller.js

```javascript
sap.ui.define([
    "sap/ui/core/mvc/Controller",
    "sap/ui/model/json/JSONModel"
  ],function(Controller,JSONModel){
      "use strict"
      return Controller.extend("views.main",{
        onInit : function(){
        },
        onAfterRendering : function(){
            this.onGetRequest('https://services.odata.org/V4/(S(epugafwj5m0rg30yoxbtltlk))/TripPinServiceRW/?$format=json');
        },
        onGetRequest : function(url){
            fetch(url)
            .then((res)=>res.json())
            .then((param)=>{
                this.getView().setModel(new JSONModel(param));
            });
        }
      });
  });
  
```

보시고 뭔가 느낌이 오시나요? 위의 방식은 TripPinServiceRW을 호출하고 받은 JSON 배열의 Property를 활용해서 화면에 매핑한 방법입니다. 위의 방식이 복잡한 데이터를 관리하는데 있어서 편리할 것 같고, 지금 방식은 쉽고 빠르게 활용할 수 있는 장점이 있는 같습니다. 여러분들은 편하신대로 사용하시면 됩니다.

### Model을 수정하여, 화면에 데이터를 추가하

UI5에서 View.xml에 매핑된 Model 정보를 수정한다면, 화면 리스트를 실시간으로 변화시킬 수 있습니다. 아래 예제를 보시면,  TripPinServiceRW Odata 서비스에서 제공받은 데이터에서 새로 행을 추가하여 기존의   리스트에 추가한 행이 화면에 뿌려지는 것을 보실 수 있습니다. 

```javascript
sap.ui.define([
    "sap/ui/core/mvc/Controller",
    "sap/ui/model/json/JSONModel"
  ],function(Controller,JSONModel){
      "use strict"
      return Controller.extend("views.main",{
        onInit : function(){
            this.getView().setModel(new JSONModel({}), "TripPinServiceRW");
        },
        onAfterRendering : function(){
            this.onGetRequest('https://services.odata.org/V4/(S(epugafwj5m0rg30yoxbtltlk))/TripPinServiceRW/?$format=json');
        },
        onGetRequest : function(url){
            console.log(this.getView().getModel("TripPinServiceRW").getData());
            fetch(url)
            .then((res)=>res.json())
            .then((param)=>{
                console.log(param.value);
                this.getView().setModel(new JSONModel(param.value), "TripPinServiceRW");
                console.log(this.getView().getModel("TripPinServiceRW"));
            }).then(()=>{
                var temp = this.getView().getModel("TripPinServiceRW").getData();
                temp.push({
                            "name" : "testService",
                            "kind" : "EntitySet",
                            "url"  : "Photos"
                          });
    
                this.getView().setModel(new JSONModel(temp),"TripPinServiceRW");
                console.log(this.getView().getModel("TripPinServiceRW").getData());
            });
        },
        
      });
  });
```

### 화면결과

![Odata &#xD1B5;&#xC2E0;&#xC5D0;&#xC11C; &#xBC1B;&#xC740; &#xAC12;&#xC744; &#xD654;&#xBA74;&#xC5D0; &#xBFCC;&#xB9B0; &#xACB0;&#xACFC;](../../.gitbook/assets/tableview%20%281%29.PNG)

![Odata &#xD1B5;&#xC2E0;&#xC5D0;&#xC11C; &#xBC1B;&#xC740; &#xAC12;&#xC5D0; &#xC0C8;&#xB85C;&#xC6B4; &#xD589;&#xC744; &#xCD94;&#xAC00;&#xD558;&#xC5EC; &#xD654;&#xBA74;&#xC5D0; &#xBFCC;&#xB9B0; &#xACB0;&#xACFC;](../../.gitbook/assets/image%20%2811%29.png)

## 별책-this 지시자 사용하여 데이터 관리

UI5는 JS 라이브러리이기 때문에 JS 고유의 문법을 사용할 수 있습니다. UI5에서 Model로 굳이 표현하지 않아도 되는 데이터는 this 지시자를 통해서 접근도 가능합니다. \(만약, this 지시자가 main.controller.js값이 아닌 경우, bind 함수나 apply 함수를 사용하여 this 지시자 값을 맞춰주셔야합니다.\) 아래 소스는 위에서 진행한 방법과 마찬가지로 OData 서비스의 결과 값을 화면에 뿌려줍니다. 하지만, forEach 반복문에서 this 지시자를 main.controller.js를 맞추기 위해 arrow function을 사용하였습니다.

```javascript
sap.ui.define([
    "sap/ui/core/mvc/Controller",
    "sap/ui/model/json/JSONModel"
  ],function(Controller,JSONModel){
      "use strict"
      return Controller.extend("views.main",{
        onInit : function(){
            this.oModel = [];
        },
        onAfterRendering : function(){
            this.onGetRequest('https://services.odata.org/V4/(S(epugafwj5m0rg30yoxbtltlk))/TripPinServiceRW/?$format=json');
        },
        onGetRequest : function(url){
            fetch(url)
            .then((res)=>res.json())
            .then((param)=>{
                param.value.forEach((val)=>{
                    this.oModel.push(val);
                });
            }).then(()=>{
                this.oModel.push({
                    "name" : "testService",
                    "kind" : "EntitySet",
                    "url"  : "Photos"
                });
                console.log(this.oModel);
                this.getView().setModel(new JSONModel(this.oModel),"TripPinServiceRW");
                console.log(this.getView().getModel("TripPinServiceRW").getData());
            });
        },
        
      });
  });
```

![&#xC704;&#xC758; &#xC18C;&#xC2A4;&#xC640; &#xAC19;&#xC740; &#xACB0;&#xACFC; &#xD654;&#xBA74;&#xC744; &#xBCFC; &#xC218; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.](../../.gitbook/assets/image%20%2810%29.png)

