---
description: localization을 활용한 국가별 컨텐츠 바꾸기
---

# Tutorial 13. i18n

## 들어가면서

i18n이란 Internationalization의 준말입니다. 이처럼 UI5는 글로벌 UI 솔루션 라이브러리 답게 각 언어 상황에 따라 메뉴의 컨텐츠를 동적으로 매핑해줍니다. 예를 들면, 한국의 유저는 i18n\_kr 영어권 유저는 i18n\_en 처럼 말이지요. i18n 파일에서 해당 서버스가 제공하는 언어의 영역 만큼 지원이 가능합니다. 여기선 간단하게 영어와 한국어를 이용한 i18n 컨텐츠 적용하는 튜토리얼을 진행하겠습니다.

## i18n  properties 활용하기

i18n 파일도 일종의 OData Model입니다. i18n이라는 OData Model에서 View에 필요한 내용을 매핑합니다. 크게 i18n의 기능은 두가지로 나눠집니다. 먼저, 브라우저 언어 설정에 따라 언어를 자동으로 매핑하는것입니다. \(아랍권의 경우 컨텐츠의 방향이 다른데 이 부분도 보정해줍니다. = 오른쪽에서 왼쪽\) 다른 하나는, 바로 multiple parameter를 사용하여, 여러개의 Text를 매핑시킬수 있습니다.

i18n은 브라우저의 언어 설정에 따라 자동으로 매핑됩니다. 그러므로 강제로 특정 언어로 view를 매핑하고 싶을 때엔 아래와 같은 구문을 사용하면,  원하는 언어로 매핑이 가능합니다. 또한, 해당언어가 없을 경우 default 세팅된 i18n properties로 매핑이 됩니다. \(보통은 영어로 사용됩니다.\)

multiple parameter를 사용하는 방법은 `appDescription=예시 샘플 {0} {1} {2}` 처럼 중괄호안에 숫자를 넣고, setText 할때 숫자의 순서대로 넣을 텍스트 값을 배열 안에 넣어주면 됩니다.

{% code title="MainView.controller.js" %}
```javascript
sap.ui.getCore().getConfiguration().setLanguage("en"); 
```
{% endcode %}

{% code title="MainView.controller.js" %}
```javascript
sap.ui.define([
  "com/myorg/ui5Router/controller/BaseController",
  "sap/ui/model/json/JSONModel",
  "sap/ui/model/resource/ResourceModel"
], function(Controller, JSONModel,ResourceModel) {
  "use strict";

  return Controller.extend("com.myorg.ui5Router.controller.MainView", {
    onInit : function(){
      var oData = {
          country : "en"
      }
      this.getView().setModel(new JSONModel(oData),"country");

      var i18nModel = new ResourceModel({
        bundleName : "com.myorg.ui5Router.i18n.i18n"
      });
      this.getView().setModel(i18nModel, "i18n");
    },
    onAfterRendering : function(){
      var oResourceBundle = this.getView().getModel("i18n").getResourceBundle();
      this.byId("oLabel").setText(oResourceBundle.getText("appDescription",
        [
          oResourceBundle.getText("labelTitle1"),
          oResourceBundle.getText("labelTitle2"),
          oResourceBundle.getText("labelTitle3")
        ]));
    },
    onEvent : function(){
      var country = this.getView().getModel("country").getProperty("/country");
      var oResourceBundle = this.getView().getModel("i18n").getResourceBundle();

      if(country === "kr"){  
        this.getView().setModel(new JSONModel({ country : "en" }),"country");
        sap.ui.getCore().getConfiguration().setLanguage("en");   
      }else{
        this.getView().setModel(new JSONModel({ country : "kr" }),"country");
        sap.ui.getCore().getConfiguration().setLanguage("kr"); //강제로 한국어 설정 원래는 브라우저 언어 설정에 따라 자동으로 i18n properties가 반영      
      }

      this.byId("oLabel").setText(oResourceBundle.getText("appDescription",[oResourceBundle.getText("labelTitle")]));
    }
  });
});

```
{% endcode %}

{% code title="MainView.view.xml" %}
```markup
 <mvc:View controllerName="com.myorg.ui5Router.controller.MainView"
  displayBlock="true"
  xmlns="sap.m"
  xmlns:mvc="sap.ui.core.mvc"
  xmlns:ui="com.myorg.ui5Router.control">
  <App id="idAppControl" >
    <pages>
      <Page title="{i18n>title}">
        <content>
          <VBox>
            <Label text="{i18n>appDescription}" id="oLabel"/>
            <Button text="{i18n>btnEvent}" press="onEvent"/>
          </VBox>
        </content>
      </Page>
    </pages>
  </App>
</mvc:View>
```
{% endcode %}

{% code title="i18n\_en.properties" %}
```text
title=sample title
appTitle=sample title
appDescription=sample {0} {1} {2}
btnEvent=Change Nation
labelTitle1=label title1
labelTitle2=label title2
labelTitle3=label title3
```
{% endcode %}

{% code title="i18n\_kr.properties" %}
```text
title=한국 제목
appTitle=한국 제목
appDescription=예시 샘플 {0} {1} {2}
btnEvent=국가 바꾸기
labelTitle1=라벨 제목1
labelTitle2=라벨 제목2
labelTitle3=라벨 제목3
```
{% endcode %}

![](../../.gitbook/assets/image%20%2834%29.png)

