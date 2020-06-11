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

이번 튜토리얼에서 중요하게 다룰 부분은 바로 UI5 Filter입니다. 



## TodoList

