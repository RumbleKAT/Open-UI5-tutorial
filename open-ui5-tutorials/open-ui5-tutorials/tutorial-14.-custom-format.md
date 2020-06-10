---
description: custom formatter를 이용한 표기
---

# Tutorial 14. Custom formatter

## 들어가면서

UI5를 이용하면 데이터 모델의 속성 값에 따라서 바인딩을 바꿔주는 것이 가능합니다. 특정 status 값을 동적으로 List에서 보여주고 싶을 때 formatter는 효과적으로 이용됩니다. 

## 간단한 custom formatter 만들기

custom formatter를 만드는 방법은 먼저, Model 폴더에 formatter.js 파일을 배치하고, 데이터 모델에서 status를 가져온 후, resourceBundle에서 텍스트로 반환하는 함수를 만드는 것입니다. 이번 예시는 데이터 모델에 status 값이 A, B, C 인 경우에 따라서 small, middle, large 의 값이 custom formatter를 이용하여 나오도록 만들었습니다. 

{% code title="MainView.controller.js" %}
```javascript
sap.ui.define([
  "com/myorg/ui5Router/controller/BaseController",
  "sap/ui/model/json/JSONModel",
  "com/myorg/ui5Router/model/formatter",
  "sap/ui/model/resource/ResourceModel"
], function(Controller, JSONModel,formatter,ResourceModel) {
  "use strict";
  return Controller.extend("com.myorg.ui5Router.controller.MainView", {
    formatter : formatter,
    onInit : function(){ 
      var i18nModel = new ResourceModel({
        bundleName : "com.myorg.ui5Router.i18n.i18n"
      });
      this.getView().setModel(i18nModel, "i18n");

      var oData = [
      {
        id : "1",
        value : 1000,
        statusType : "A",
        curreny : "KRW"
      },
      {
        id : "2",
        value : 1,
        statusType : "B",
        curreny : "KRW"
      },
      {
        id : "3",
        value : 100,
        statusType : "C",
        curreny : "KRW"
      },
    ];
      this.getView().setModel(new JSONModel(oData), "oSample");
    }
  });
});

```
{% endcode %}

{% code title="model/formatter.js" %}
```javascript
sap.ui.define([], function () {
	"use strict";
	return {
		statusText : function (status){
			var resourceBundle = this.getView().getModel("i18n").getResourceBundle();
			switch(status){
				case "A" : 
					return resourceBundle.getText("statusA");
				case "B" :
					return resourceBundle.getText("statusB");
				case "C" : 
					return resourceBundle.getText("statusC");
				default: 
					return status;
			}
		}
	};
});

```
{% endcode %}

```markup
 <mvc:View controllerName="com.myorg.ui5Router.controller.MainView"
  displayBlock="true"
  xmlns="sap.m"
  xmlns:mvc="sap.ui.core.mvc"
  xmlns:ui="com.myorg.ui5Router.control"
  >
  <App id="idAppControl" >
    <pages>
      <Page title="{i18n>title}">
        <content>
          <List class="sapUiResponsiveMargin" width="auto" items="{path : 'oSample>/'}">
            <items>
              <ObjectListItem title="{oSample>id}" numberUnit="{oSample>/curreny}">
              <firstStatus>
                <ObjectStatus text="{
                  path : 'oSample>statusType',
                  formatter : '.formatter.statusText'
                }"/> <-- .formatter.statusText를 실행함으로서 동적으로 status 값을 매핑하는 함수 수행 -->
              </firstStatus>
              </ObjectListItem>
            </items>
          </List>
        </content>
      </Page>
    </pages>
  </App>
</mvc:View>
```

 

![](../../.gitbook/assets/image%20%2837%29.png)

