---
description: View에서 반복되는 부분을 Fragement로 만들어 봅니다.
---

# Tutorial 09. Fragment

## 들어가면서

View를 만들면서 반복되는 UI control이 늘어나는 경우가 있습니다. React나 Vue를 예로 들면 이를 template이나 컴포넌트 형태로 View단을 나누어 효율적인 코드 유지보수를 가능하게 지원합니다. UI5에서도 이처럼 비슷한 기능을 제공하는데 바로, Fragment와 Custom controls입니다. 이 둘의 차이점은 Fragment는 단순한 UI control의 묶음으로, Fragment를 호출하는 controller에서 제어할 수 있습니다. 또한, 주로 popup 화면에 사용됩니다. 반면, Custom controls의 경우 React의 컴포넌트처럼 사용이 가능합니다. View.xml 파일에서 하나의 UI controls 묶음을 만들어 하나의 단위로 만들 수 있습니다.

## Fragment 알아보기

Fragment는 주로 Dialog 화면을 구현하는데 많이 사용됩니다. 예를 들면 팝업창 같은 용도이지요. jsView와 xmlView로 Fragment를 만들 수 있습니다. 이 튜토리얼에선 xmlFragment를 사용하겠습니다. xmlFragment는 화면을 구성하는 view 처럼 따로 xml 파일로 개발을 진행합니다.

### 디렉토리 구성

아래 디렉토리 구성을 보시면 MainView를 제외하고 DialogPanel.fragment.xml 파일이 있습니다. DialogPanel의 특징은 바로 controller.js 파일이 없다는 점입니다. DialogPanel에서 이벤트가 발생하면 이를 불러오는 view의 컨트롤러에서 이벤트 함수를 구현해주어야합니다.

```text
webapp
  |
  -----View 
  |     |----- DialogPanel.fragment.xml
  |     |----- MainView.view.xml
  | 
  -----Controller
        |----- MainView.controller.js     
```

{% code title="DialogPanel.fragment.xml" %}
```markup
<core:FragmentDefinition
  xmlns="sap.m"
  xmlns:core="sap.ui.core"
  xmlns:f="sap.ui.layout.form"
  >
    <Dialog id="CustomDialog" title="CustomDialog">
        <f:SimpleForm editable="true">
            <f:content>
                <Label text="Custom Dialog"/>
                <Input type="text" id="oInput" placeholder="text on this"/>
            </f:content>
        </f:SimpleForm>
        <FlexBox alignItems="Center" justifyContent="Center">
            <items>
                <Button type="Reject" text="Close" press="onClose"/>
            </items>
        </FlexBox>
    </Dialog>
</core:FragmentDefinition>
```
{% endcode %}

{% code title="MainView.view.xml" %}
```markup
 <mvc:View controllerName="com.myorg.Fragment.controller.MainView"
	displayBlock="true"
	xmlns="sap.m"
	xmlns:mvc="sap.ui.core.mvc">
	<App id="idAppControl">
		<pages>
			<Page title="{i18n>title}">
				<content>
					<Button text="open Dialog" press="openDialog"/>
				</content>
			</Page>
		</pages>
	</App>
</mvc:View> 

```
{% endcode %}

{% code title="MainView.controller.js" %}
```javascript
sap.ui.define([
  "sap/ui/core/mvc/Controller"
], function(Controller) {
  "use strict";
  return Controller.extend("com.myorg.Fragment.controller.MainView", {
    openDialog : function(){
      if (!this.oDialog){ //Dialog 객체가 없는 경우
        //특정 경로에 있는 Dialog Fragment를 로드하여 컨트롤러에 매핑
        this.oDialog = sap.ui.xmlfragment("com.myorg.Fragment.view.DialogPanel",this);
        this.oDialog.open();
      } else { //Dialog 객체가 있으면
        this.oDialog.open();
      }
    },
    onClose : function(){
      //Dialog에서 정의한 input 태그의 값을 가져오기 위해선 sap.ui.getCore로 접근해야함
      var oInput = sap.ui.getCore().byId("oInput").getValue();
      alert(oInput);
      this.oDialog.close();
    }
  });
});

```
{% endcode %}

## 결과화면



