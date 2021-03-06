---
description: Local Storage를 활용한 TodoList 만들기
---

# Tutorial 11. TodoList

## 들어가면서 

여태까지 저희가 배운 것을 바탕으로 간단한 ui5 웹앱을 만드려고 합니다. 바로 TodoList 만들기 입니다. 인터넷 익스플로러를 제외한 대부분의 브라우저는 local storage를 제공합니다. local storage는 쿠키와 비슷하게 5MB 정도의 브라우저 내부 저장 공간에 데이터를 저장하고 읽을 수 있는 라이브러리입니다. local storage를 ui5에서 사용하기 위해선, 먼저 JQuery에서 local Storage를 연결한 후, 이를 모델 부분에서 import 하여 사용할 수 있습니다.

##  결과 예시 

![](../../.gitbook/assets/image%20%2824%29.png)

![](../../.gitbook/assets/image%20%2820%29.png)

이번 과정에서 배울 TodoList의 화면은 위의 캡처를 참고하시면됩니다. 크게 두개의 페이지로 구성이 되어있습니다. 하나는, todo, doing , done에 맞춰서 제목과 내용을 보여줍니다. 만약 todo 상태의 일이 doing으로 넘어 가면 체크 버튼을 눌러서 다음 단계로 이동시킬수 있습니다. 그리고 doing에 있던 일이 done 상태가 되면 이는 일이 완전히 끝난 것이기 때문에 체크 버튼과 삭제버튼이 사라집니다. 만약 모든 리스트를 삭제하고 싶으시다면, 오른쪽 상단의 휴지통 버튼을 눌러서 전체 삭제를 할 수 있습니다. 

반면, 다른 하나의 페이지는 add todoList 페이지로 todo 상태의 작업을 추가하는 페이지 입니다. submit버튼을 누르면 성공적으로 todoList 저장이 될 때, 데이터 생성에 성공했다는 메시지가 제공됩니다.

![](../../.gitbook/assets/image%20%2821%29.png)

![](../../.gitbook/assets/image%20%2822%29.png)

![](../../.gitbook/assets/image%20%2823%29.png)

### Mobile 결과 예시

UI5를 사용하면 장점 중 하나가 반응형 웹으로 태블릿이나 모바일의 해상도에 맞춰서 자동으로 화면을 그려주는 것이 있습니다. 

![](../../.gitbook/assets/image%20%2839%29.png)

![](../../.gitbook/assets/image%20%2838%29.png)

## LocalStorage 활용

UI5은 자체적으로 jQuery를 포함하고 있습니다. UI5에서 localStorage를 사용하기 위해선,  아래의 선언문으로 localStorage 객체를 정의할수 있습니다. 그리고 TodoList는 데이터를 읽고, 쓰고, 수정하고, 지우는 기능을 가지고 있습니다. \(Create, Read, Update, Delete\) 반면, localStorage는 데이터를 불러오고\(GET\), 정의하는\(SET\)기능을 제공합니다. CRUD 기능을 제공하기 위해 생성과 수정 기능은 localStorage의 SET 함수를 이용하였고, 삭제 기능은 JS filter 함수를 이용하여 특정 element를 제거한 객체를 저장하는 방식으로 구현을 진행했습니다.

```javascript
this._storage = jQuery.sap.storage(jQuery.sap.storage.Type.local);
```

{% code title="model/localStorage.js" %}
```javascript
sap.ui.define([], function() {
	"use strict";
	return {
		init : function(){
			this._storage = jQuery.sap.storage(jQuery.sap.storage.Type.local);
			let temp = this._storage.get("todoList");
			if(temp){
				this.todoList = JSON.parse(temp);
			}else{
				this.todoList = [
					{ title : "song" , id : 1, type : "todo", description : "설명....."},
					{ title: "song", id: 5,type: "done" ,description : "설명....."},
					{ title: "song", id: 4, type : "doing" ,description : "설명....."},
					{ title: "song", id: 3, type : "todo" ,description : "설명....."},
				  ];
			}
		},
		getData : function(type){
			return this.getDatas().filter(element => {
				return element.type === type
			});
		},
		getDatas : function(){
			return this.todoList;
		},
		deleteDataAll : function(){
			this.todoList = [];
			this.setData([]);
		},
		deleteData : function(id){
			let leftData = JSON.parse(this.getDatas()).filter( element => element.id !== id);
			this.setData(leftData);
			this.updateData(leftData);
		},
		setData : function(data){
			this._storage.put('todoList',JSON.stringify(data));
			this.updateData(data);
		},
		updateData : function(data){
			this.todoList = data;
		}
	};
});

```
{% endcode %}

## Add TodoList

todoList를 만들기 위해선 먼저, todoList를 추가하는 것부터 시작해야 합니다. 일단 todoList 특성상 todo, doing, done의 3가지 카테고리를 가집니다. 그리고 제목과 내용을 카드뷰로 보여주는 구조이지요. 특히 add TodoList 페이지에선 데이터를 추가하는 역할만 하면 되기 때문에, dataObject 객체에 입력할 데이터 형식을 정의하였습니다.

```javascript
var dataObject = {
   title: "",
   type : "",
   description : ""
}

var dataObjectJSON = new JSONModel(dataObject);
var categoriesJSON = new JSONModel(categories);

this.getView().setModel(dataObjectJSON);
this.getView().setModel(categoriesJSON,'categories');
```

UI5는 one-way binding과 two-way binding을 사용할 수 있습니다. todoList를 예로 들면 할일을 보여주는 카드뷰는 데이터가 일방향적으로 사용자에게 보여집니다. 반면, 할일을 추가하는 뷰는 초기엔 빈값으로 초기화가 되어있지만, 나중에 사용자의 입력을 받으면, 이에 따라 Model의 값도 자동으로 변환이 되어 사용자가 작성한 값을 저장해야 합니다. 

그러므로, Add todoList에서는 two-way binding을 이용하여 사용자의 입력 따라 변화하는 값을 저장할 것입니다. onSave 버튼을 누르면,  앞서 선언한 dataObjectJSON  객체에 매핑된 값이 있는 경우에 데이터를 저장하는 방식으로 구현을 진행했습니다. 

```javascript
sap.ui.define([
   'sap/ui/core/mvc/Controller',
   "com/myorg/todoList/model/localStorage",
   "sap/ui/model/json/JSONModel"
],function(Controller,models, JSONModel){
    'use strict'
    return Controller.extend('com.myorg.todoList.controller.AddTodoList',{
        onInit(){
            var categories = {
                "types" : [
                    {
                        name : "todo"
                    },
                    {
                        name : "doing"
                    },
                    {
                        name : "done"
                    }
                ]
            };
            var dataObject = {
                title: "",
                type : "",
                description : ""
            }
            var dataObjectJSON = new JSONModel(dataObject);
            var categoriesJSON = new JSONModel(categories);

            this.getView().setModel(dataObjectJSON);
            this.getView().setModel(categoriesJSON,'categories');
        },
        NavToMain(){
            var oRouter = this.getRouter();
            oRouter.initialize(); // restart the router
            oRouter.navTo('RouteMainView',true);
        },
        getRouter : function () {
          return sap.ui.core.UIComponent.getRouterFor(this)
        },
        onSave : function(){
            var oModel = this.getView().getModel().getData();
            
            if(oModel.title === '' || oModel.type === ''  || oModel.description === '' ){
                alert("please write all items!");   
                this.onClear();
                return;
            }else if(!(oModel.type === "todo" || oModel.type === "doing" || oModel.type === "done" )){
                alert("please select right item!");   
                this.onClear();
                return;
            }
            
            //save item
            var todoLists = models.getDatas();
            const maxIdx = todoLists.sort((el1,el2)=>el2.id - el1.id);
            let nextIdx = undefined;
            console.log(maxIdx);
            if(maxIdx.length === 0){
                nextIdx = 1;
            }else{
                nextIdx = maxIdx[0].id + 1;
            }
            
            oModel.id = nextIdx;
            todoLists.push(oModel);

            console.log(todoLists);
            models.setData(todoLists);
            alert("save success!");            
            
            this.onClear();
        },
        onClear : function(){
            var dataObject = {
                title: "",
                type : "",
                description : ""
            }
            this.getView().setModel(new JSONModel(dataObject));
        }
    })
})
```

```markup
<mvc:View
    controllerName="com.myorg.todoList.controller.AddTodoList"
    xmlns="sap.m"
    xmlns:mvc="sap.ui.core.mvc"
    xmlns:l="sap.ui.layout"
    xmlns:f="sap.ui.layout.form"
    xmlns:core="sap.ui.core"
    >
    <App id="idAppControl">
        <Page id="addTodoList" 
            title="{i18n>AddTodoList}" 
            navButtonPress="onNavBack"
            class="sapUiResponsiveContentPadding">
             <customHeader>
                <Bar>
                    <contentLeft>
                        <Button icon="sap-icon://arrow-left" press="NavToMain" />
                    </contentLeft>
                    <contentMiddle>
		                <Text text = "Add TodoList"></Text>
                    </contentMiddle> 
                </Bar>
            </customHeader>
            <Panel width="auto" headerText="Add TodoList">
                <content>
                    <f:SimpleForm layout="ResponsiveGridLayout" id="SimpleForm" editable="true" width="100%">
                    <f:content id="FormContent">                    
                        <VBox>
                            <items>
                                <Label text="Title" id="title"/>
                                <Input width="100%" id="title_id" placeholder="Please write the title.." value="{/title}"/>

                                <Label text="Type" id="type"/>
                                <ComboBox id="comboBox" items="{path: 'categories>/types'}" width="100%" value="{/type}">
                                    <items>
                                        <core:Item key="{categories>name}" text="{categories>name}"/> 
                                    </items>
                                </ComboBox>
                                <Label text="Description" id="description"/>
                                <TextArea width="100%" value="{/description}" rows="5" id="description_id" placeholder="Please write the description.."/>
                            </items>
                        </VBox>
                    </f:content>
                </f:SimpleForm>
                <FlexBox
					height="100px"
					alignItems="Start"
					justifyContent="Center">
					<items>
						<Button width="100px" type="Accept" text="submit" press="onSave" class="sapUiSmallMarginEnd" />
						<Button width="100px" type="Reject" text="clear" press="onClear" class="sapUiSmallMarginEnd" />
					</items>
				</FlexBox>
                </content>
            </Panel>
        </Page>
    </App>
</mvc:View>
```

## TodoList 만들기

todoList를 보여주는 부분인 MainView.controller.js에서는 앞서 정의한 todo doing done 3가지 status에 따라서 값을 필터링하여 보여주는 작업을 진행합니다. 우선 localStorage에 저장된 값을 onBeforeRendering 상태에서 가져오고, 이를 todoList Model로 정의하였습니다. 

todoList 특성상 완료된 일은 다음 단계로 보여지고, 삭제할 일은 삭제되어야합니다. 완료된 일을 처리하기 위해 onAccept 함수에서는 해당 이벤트가 발생한 부모 컴포넌트에 바인딩된 정보를 가지고 해당 type이 todo인 경우에는 doing으로, doing인 경우에는 done으로 status 값을 바꿔주었습니다. UI5는 Model의 데이터가 바뀌면 자동으로 렌더링을 진행합니다. \(마치 리엑트와 비슷하네요\) 만약 type이 바뀐다면, 별다른 처리를 하지않아도 바뀐 type의 verticalLayout에 매핑이 되어 있을 것입니다.

### Filter 스마트하게 사용하기

UI5에서 VBox나 Vertical Layout과 같이 Model의 데이터를 배열의 형태로 보여주는 태그에선 content 속성값에 매핑할 model의 배열 정보를 입력합니다. 하지만 여기에 필요한 값만 보여주는 Filter를 이용한 커스터마이징 작업이 가능합니다. ​

#### Filter 객체를 선언하여 필터 추가 

Filter 객체를 사용하면 IF문이나 CASE문과 같은 문법을 사용하지 않아도 조건문을 사용할 수 있습니다. 바로, Filter 객체와 Filter 객체의 조건을 담는 FilterOperator를 사용하면 가능합니다. 

{% code title="MainView.view.xml" %}
```markup
 <l:VerticalLayout content="{ path : 'todoList>/' }" id = "todo">
	<f:Card
	class="sapUiSmallMargin"
	width="300px"
	>
		<f:header>
			<card:Header
				title="{todoList>title}"
				subtitle="{todoList>name}"/>
		</f:header>
		<f:content>
			<VBox class="sapUiSmallMarginBegin sapUiSmallMarginTopBottom" >
				<VBox>
					<Text text="{todoList>description}"/>
				</VBox>
				<FlexBox
					alignItems="Start"
					justifyContent="Center">
					<items>
						<Button type="Accept"
						icon="sap-icon://accept" 
						press="onAccept" class="sapUiSmallMarginEnd" />
						<Button type="Reject"
						icon="sap-icon://delete"
						press="onDelete" class="sapUiSmallMarginEnd" />
					</items>
				</FlexBox>
			</VBox>
		</f:content>
	</f:Card>
</l:VerticalLayout>
```
{% endcode %}

{% code title="MainView.controller.js" %}
```javascript
  var oFilter = new sap.ui.model.Filter({
    filters: [
      new sap.ui.model.Filter("type", sap.ui.model.FilterOperator.EQ, 'todo'),
    ],
    and: false
  });

  var oBinding =  this.getView().byId("todo").getBinding("content");
  oBinding.filter(oFilter, sap.ui.model.FilterType.Application);
```
{% endcode %}

#### Content 속성에 필터를 추가​

Content 속성은 Model의 배열 정보와 바인드 됩니다. 이처럼 바인딩된 배열 정보는 한줄씩 렌더링이 되어 보여집니다. 앞선 [Tutorial 06. UI5 Model](tutorial-06.-ui5-model.md) 편을 보시면,  path 경로를 활용한 OData Model 바인딩 방법을 보실수 있습니다. path 경로로 OData Model을 바인드 한 후,  Filters 설정을 추가하면 Filter 객체를 선언하지 않아도 필터 기능을 사용할 수 있습니다.

아래 예시는 type이 todo 인 값이 있다면 verticalLayout에 값을 매핑한다라는 의미입니다. 여기에 filter 조건을 추가하고 싶으시다면, 아래 조건과 같이  선언을 해주시면 됩니다.`{path : '대상값' , operator : 'FilterOperator의 operator 값', value1 : '비교대상값' }` 

```markup
	<l:VerticalLayout content="{ path : 'todoList>/', 
		filters : [  { path : 'type', operator : 'EQ', value1 : 'todo' } ] }" >
```

![](../../.gitbook/assets/cardinality.png)

{% code title="MainView.controller.js" %}
```javascript
sap.ui.define([
  "sap/ui/core/mvc/Controller",
  "com/myorg/todoList/model/localStorage",
  "sap/ui/model/json/JSONModel"
], function(Controller,models, JSONModel) {
  "use strict";

  return Controller.extend("com.myorg.todoList.controller.MainView", {
	onInit : function(){
    models.init();
    let oRouter = sap.ui.core.UIComponent.getRouterFor(this);
    oRouter.attachRouteMatched(this._onObjectMatched, this);
  },
  _onObjectMatched: function(oEvent) {
    var todoList = Object.assign([],models.getDatas());
		var oModel = new JSONModel(todoList);
    this.getView().setModel(oModel,"todoList");
  },
  onBeforeRendering : function(){
    var todoList = Object.assign([],models.getDatas());
		var oModel = new JSONModel(todoList);
    console.log(todoList);
		this.getView().setModel(oModel,"todoList");
  },
  onAccept : function(oEventArgs){
    let row = oEventArgs.getSource();
    let context = row.getBindingContext("todoList");
    let content = context.oModel.getProperty(context.sPath);

    if(content.type === "todo"){
      content.type = "doing";
    }else if(content.type === "doing"){
      content.type = "done";
    }
    
    let oModel =this.getView().getModel("todoList").getData();
    oModel.forEach(element => {
      if(element.id === content.id){
        element = content;
      } 
    });
    models.setData(oModel);

    this.getView().setModel(new JSONModel(oModel),"todoList");
  },
 
  onDelete : function(oEventArgs){
    let row = oEventArgs.getSource();
    let context = row.getBindingContext("todoList");
    let content = context.oModel.getProperty(context.sPath);
    
    let oModel =this.getView().getModel("todoList").getData();
    let temp = oModel.filter(item => content.id != item.id);
    models.setData(temp);

    this.getView().setModel(new JSONModel(temp),"todoList");
  },
  NavToAddTodoList : function(){
    console.log("!!");
    this.getRouter().navTo("AddTodoList",{}, false);
  },
  getRouter : function () {
    return sap.ui.core.UIComponent.getRouterFor(this)
  },
  clearAllData : function(){
    models.deleteDataAll();
    let oModel = [];
    this.getView().setModel(new JSONModel(oModel),"todoList");
  }
  });
});

```
{% endcode %}

```markup
 <mvc:View controllerName="com.myorg.todoList.controller.MainView"
	xmlns="sap.m"
    xmlns:mvc="sap.ui.core.mvc"
	xmlns:f="sap.f"
	xmlns:card="sap.f.cards"
    xmlns:core="sap.ui.core"
	xmlns:l="sap.ui.layout"
    displayBlock="true"
    height="100%">
	<App id="idAppControl">
		<pages>
			<Page title="{i18n>title}">
				<headerContent>
				<Button
					icon="sap-icon://add"
					press="NavToAddTodoList" />
				<Button
					icon="sap-icon://delete"
					press="clearAllData" />
				</headerContent>
				<content>
				<l:Grid defaultSpan="L4 M8 S12" class="sapUiSmallMarginTop">
					<l:content alignItems="Center">
					<VBox alignItems="Center">
					<Title text="Todo" level="H2"/>
					<l:VerticalLayout content="{
					 path : 'todoList>/', 
					 filters : [ 
				 		{ path : 'type', operator : 'EQ', value1 : 'todo' }
					]
				 }" >
					<f:Card
					class="sapUiSmallMargin"
					width="300px"
					>
						<f:header>
							<card:Header
								title="{todoList>title}"
								subtitle="{todoList>name}"/>
						</f:header>
						<f:content>
							<VBox class="sapUiSmallMarginBegin sapUiSmallMarginTopBottom" >
								<VBox>
									<Text text="{todoList>description}"/>
								</VBox>
								<FlexBox
									alignItems="Start"
									justifyContent="Center">
									<items>
										<Button type="Accept"
										icon="sap-icon://accept" 
										press="onAccept" class="sapUiSmallMarginEnd" />
										<Button type="Reject"
										icon="sap-icon://delete"
										press="onDelete" class="sapUiSmallMarginEnd" />
									</items>
								</FlexBox>
							</VBox>
						</f:content>
					</f:Card>
				</l:VerticalLayout>
				</VBox>
				<VBox alignItems="Center">
					<Title text="Doing" level="H2"/>
				<l:VerticalLayout content="{
					 path : 'todoList>/', 
					 filters : [ 
				 		{ path : 'type', operator : 'EQ', value1 : 'doing' }
					]
				 }" >
					<f:Card
					class="sapUiSmallMargin"
					width="300px"
					>
						<f:header>
							<card:Header
								title="{todoList>title}"
								subtitle="{todoList>name}"/>
						</f:header>
						<f:content>
							<VBox class="sapUiSmallMarginBegin sapUiSmallMarginTopBottom" >
								<VBox>
									<Text text="{todoList>description}"/>
								</VBox>
								<FlexBox
									alignItems="Start"
									justifyContent="Center">
									<items>
										<Button type="Accept"
										icon="sap-icon://accept" 
										press="onAccept" class="sapUiSmallMarginEnd" />
										<Button type="Reject"
										icon="sap-icon://delete"
										press="onDelete" class="sapUiSmallMarginEnd" />
									</items>
								</FlexBox>
							</VBox>
						</f:content>
					</f:Card>
				</l:VerticalLayout>
								</VBox>
					<VBox alignItems="Center">
					<Title text="Done" level="H2"/>
					<l:VerticalLayout content="{
							path : 'todoList>/', 
							filters : [ 
								{ path : 'type', operator : 'EQ', value1 : 'done' }
							]
						}" >
							<f:Card
							class="sapUiSmallMargin"
							width="300px">
								<f:header>
									<card:Header
										title="{todoList>title}"
										subtitle="{todoList>name}"/>
								</f:header>
								<f:content>
									<VBox class="sapUiSmallMarginBegin sapUiSmallMarginTopBottom" >
										<VBox>
											<Text text="{todoList>description}"/>
										</VBox>
									</VBox>
								</f:content>
							</f:Card>
						</l:VerticalLayout> 
						</VBox>
					</l:content>
				</l:Grid>
				</content>
			</Page>
		</pages>
	</App>
</mvc:View> 
```

