---
description: Custom control을 사용하여 반복되는 컨트롤을 효과적으로 줄일 수 있습니다.
---

# Tutorial 10. Custom controls

## 들어가면서

UI5에서 주어지는  label 태그에는 다른 이벤트를 삽입할 수 없습니다. 하지만, 경우에 따라서는 이러한 것들을 자유롭게 사용하고 싶은 개발자의 니즈는 존재합니다. 반면, 반복되는 UI view를 일정 단위대로 묶고 하나의 컨트롤 단위로 묶어서 view단의 소스를 줄이고자 하는 니즈도 있을 수 있습니다. 이 두가지 니즈를 충족시키는 것이 바로, 이번 튜토리얼에서 배우는 Custom controls 입니다. 이는 말 그대로 하나의 커스텀 컨트롤을 만드는 것입니다. 앞서 언급한 label의 경우도 하나의 UI 컨트롤입니다. 만약 label 태그를 선택하여 특정 이벤트를 삽입하고자 한다면, 커스텀 컨트롤을 만든다면 가능합니다.

## 간단한 Custom Control 만들기

custom control의 특징 중 하나는 기존 html 태그를 그대로 사용할 수 있고 html 태그에 이벤트를 삽입할 수 있습니다.

{% code title="myControl.js" %}
```javascript
sap.ui.define([
    "sap/ui/core/Control",
],function(Control){
    "use strict"
    return Control.extend("com.myorg.ui5Router.controller.myControl",{
        metadata : {
            properties : {
                "text" : {
                    type : "string"
                }
            },
            events: {
                press: {enablePreventDefault : true}
             }
        },
        ontap: function () {
            alert(`click ${this.getText()}`);   
        },
        init : function(){

        },
        renderer : function(oRM, oControl){
            oRM.write(`<div`);
            oRM.writeControlData(oControl); //
            oRM.write(`>`);
            oRM.write(`<h1>${oControl.getText()}</h1>
            </div>`);
        }
    })
});
```
{% endcode %}

{% code title="MainView.view.xml" %}
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
          <ui:myControls text="Sample Title"/>
        </content>
      </Page>
    </pages>
  </App>
</mvc:View>
```
{% endcode %}

![](../../.gitbook/assets/image%20%2827%29.png)

![](../../.gitbook/assets/image%20%2825%29.png)

ES6문법을 활용한 커스텀 리스트 

{% code title="" %}
```javascript
sap.ui.define([
    "sap/ui/core/Control",
    "sap/ui/model/json/JSONModel"
],function(Control,JSONModel){
    "use strict"
    return Control.extend("com.myorg.ui5Router.controller.myControl",{
        metadata : {
            properties : {
                "text" : {
                    type : "string"
                }
            },
            events: {
                press: {enablePreventDefault : true}
             }
        },
        ontap: function () {
            alert(`click ${this.getText()}`);   
        },
        init : function(){
            var oData = [
                { "title" : "1"},
                { "title" : "2"},
                { "title" : "3"}
            ];
            sap.ui.getCore().setModel(new JSONModel(oData), 'oModel');
            // console.log(sap.ui.getCore().getModel("oModel"));
        },
        onGetList : function(){
            var lists = sap.ui.getCore().getModel("oModel").getData();
            
            var res = `<ul>`;
            lists.forEach((param)=>
                res += `<li>${param.title}</li>`
            );
            res += `</ul>`;
            return res;
        },
        renderer : function(oRM, oControl){
            oRM.write(`<div`);
            oRM.writeControlData(oControl); //
            oRM.write(`>`);
            oRM.write(`<h1>${oControl.getText()}</h1>
            ${oControl.onGetList()}
            </div>`);
        }
    })
});
```
{% endcode %}

![](../../.gitbook/assets/image%20%2826%29.png)

