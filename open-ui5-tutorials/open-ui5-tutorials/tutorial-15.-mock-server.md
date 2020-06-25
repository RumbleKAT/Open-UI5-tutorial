---
description: Mock Server를 생성하여 샘플데이터를 만들어봅니다.
---

# Tutorial 15. Mock Server

## 들어가면서

Mock Server는 가짜 서버를 만들어서 마치 api가 있는 것처럼 사용할 수 있는 서버입니다. 이 기술은 사용가능한 OData 서비스가 없거나 개발 및 테스트를 위해 OData 백엔드 연결에 의존하고 싶지 않은 경우 이용할 수 있습니다. 즉, 프론트엔드 개발자와 백엔드 개발자가 따로 개발을 진행하고 연계에 필요한 시간을 줄일 수 있습니다.

## 파일구성

### mockServer.html

mockServer.html은 index.html과 비슷한 역할입니다. mockServer.html에선 우리는 mockserver.js를 이용하여 필요한 odata를 테스트할 수 있습니다. 

### metadata.xml

서비스 인터페이스에 대한 정보를 제공하는 xml입니다. 수동적으로 내용을 적을 필요는 없고,  브라우저로 부터 메타데이터 세부정보를 복사하여 사용할 수 있습니다. \(ex http://services.odata.org/V2/NorthWind/Northwind.svc/$metadata\)

### MockData \(JSON Files\)

Odata에 실질적인 데이터를 포함한 JSON 파일에 넣는 것으로, localService 아래 경로에 넣으면 됩니다. \(MockServer가 실행되면, JSON 파일의 배열정보가 보여집니다.\)

### mockServer.js 

MockServer  모듈은 서버를 시작함과 동시에 로딩됩니다.  실제 서버 대신에 테스트 서버가 rootURI에 서비스 되고, 전형적인 서버 반응시간을 모방하여, 1초의 딜레이를 넣을 수 있습니다. 마지막으로,  Mockserver.start\(\) 커맨드를 실행하여, mockServer를 실행할 수 있습니다.

```text
webapp
   |----- localService
   |          |-------- mockdata 
   |          |            |-------- sEntities.json
   |          |-------- metadata.xml
   |          |-------- mockserver.js
   |
   |----- test
            |--------mockServer.js
```

![test &#xC640; localService &#xD3F4;&#xB354;&#xB9CC; &#xBC14;&#xAFD4;&#xC8FC;&#xBA74; &#xB429;&#xB2C8;&#xB2E4;.](../../.gitbook/assets/image%20%2841%29.png)

## Step 01. localService 폴더 수 

### metadata.xml

Tutorial 12. Using OData 파트에서 저희는 metadata.xml에는 API에서 사용되는 엔티티 타입 정보를 표현한다고 배웠습니다. metadata.xml에서 먼저 EntitySet을 표현하고 해당 Set안에 들어갈 프로퍼티를 정의해주세요.

{% code title="metadata.xml" %}
```markup
<edmx:Edmx Version="1.0" xmlns:edmx="http://schemas.microsoft.com/ado/2007/06/edmx">
	<edmx:DataServices m:DataServiceVersion="1.0" m:MaxDataServiceVersion="3.0"
					   xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata">
		<Schema Namespace="sModel" xmlns="http://schemas.microsoft.com/ado/2008/09/edm">
			<EntityType Name="sEntity">
				<Key>
					<PropertyRef Name="sTitle"/>
					<PropertyRef Name="sDescription"/>
				</Key>
				<Property Name="sTitle" Type="Edm.String" Nullable="false" MaxLength="40" FixedLength="false"
						  Unicode="true"/>
				<Property Name="sDescription" Type="Edm.String" Nullable="false" MaxLength="40" FixedLength="false"
						  Unicode="true"/>
			</EntityType>
		</Schema>
		<Schema Namespace="ODataWebV2.Sample.Model" xmlns="http://schemas.microsoft.com/ado/2008/09/edm">
			<EntityContainer Name="sEntitiesContainer" m:IsDefaultEntityContainer="true" p6:LazyLoadingEnabled="true"
							 xmlns:p6="http://schemas.microsoft.com/ado/2009/02/edm/annotation">
				<EntitySet Name="sEntities" EntityType="sModel.sEntity"/>
			</EntityContainer>
		</Schema>
	</edmx:DataServices>
</edmx:Edmx>
```
{% endcode %}

### mockdata/sEntities.json

mockdata 폴더 아래의 json 파일에는 앞서 언급했던 것처럼 실질적인 데이터를 담는 부분입니다. 이번 튜토리얼에선 간단한 테스트를 하기 위해서 json 배열을 아래와 같이 만들었습니다.

{% code title="mockdata/sEntities.json" %}
```javascript
[
    {
        "sTitle": "Title A",
        "sDescription": "Description A"
    }, 
    {
        "sTitle": "Title B",
        "sDescription": "Description B"
    }
]
```
{% endcode %}

### mockserver.js 

mockserver.js는 rootUri를 통해서 특정 uri로 odata 접근을 허용하고,  mockserver를 실행하는 부분입니다.

{% code title="mockserver.js" %}
```javascript
sap.ui.define([
    "sap/ui/core/util/MockServer",
    "sap/base/util/UriParameters"
], function(MockServer, UriParameters){
    "use strict";

    return {
        init : function(){
           // create
			var oMockServer = new MockServer({
				rootUri : "/sap/opu/odata/Entities/"
			});

			var oUriParameters = jQuery.sap.getUriParameters();
			console.log(oUriParameters);

			// configure mock server with a delay
			MockServer.config({
				autoRespond: true,
				autoRespondAfter: oUriParameters.get("serverDelay") || 500
			});

			// simulate
			var sPath = jQuery.sap.getModulePath("com.myorg.ui5Router.localService");
			console.log(sPath);
			oMockServer.simulate(sPath + "/metadata.xml", {
				'sMockdataBaseUrl' : sPath + "/mockdata/" ,
				'bGenerateMissingMockData' : true
			});

			// start
			oMockServer.start();
        }
    }
});
```
{% endcode %}

## Step 02. Test 폴더 수정 

### initMockServer.js

mockserver는 localService에서 정의한 내용을 Test 폴더에서 수행하면서 시작됩니다. 이를 위해 initMockServer.js 파일에서 localService에서 구현한 mockserver.js를 불러와 mockserver를 수행합니다.

{% code title="test/initMockServer.js" %}
```javascript
sap.ui.define([
    "../localService/mockserver"
],function(mockserver){
    "use strict"
    mockserver.init();
    sap.ui.require(["sap/ui/core/ComponentSupport"]);
});
```
{% endcode %}

### mockServer.html

mockServer.html은 index.html 같이 화면을 보여주면서, mockserver의 데이터를 로드합니다. 이를 위해 resoucreroots 경로를 test 폴더 경로로 \( "../" \) 설정하고, oninit 경로를 "module:com/myorg/ui5Router/test/initMockServer"와 같이 설정합니다. 

```markup
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>SAPUI5 Walkthrough - Test Page</title>
	<script
		id="sap-ui-bootstrap"
		src="https://openui5.hana.ondemand.com/resources/sap-ui-core.js"
		data-sap-ui-theme="sap_belize"
		data-sap-ui-resourceroots='{
			"com.myorg.ui5Router": "../"
		}'
		data-sap-ui-oninit="module:com/myorg/ui5Router/test/initMockServer"
		data-sap-ui-compatVersion="edge"
		data-sap-ui-async="true">
	</script>
	<script>
	</script>
</head>
<body class="sapUiBody" id="content">
	<div data-sap-ui-component data-name="com.myorg.ui5Router" data-id="container" data-settings='{"id" : "ui5"}'></div>
</body>
</html>
```

## Step 03. Manifest.json 파일 설정

### manifest.json

mockserver를 운영하기 위해선, 가장 중요한 부분이 바로 Manifest.json 파일 설정입니다. Manifest.json은 라우터 경로 외에도, oData 경로를 설정할 수 있습니다. 그리고 이 경로는 앞서 설정한 rootUri와 같아야 정상적으로 oData를 호출할 수 있습니다.

```javascript
{
  "_version": "1.12.0",
  "sap.app": {
    "id": "com.myorg.ui5Router",
    "type": "application",
    "i18n": "i18n/i18n.properties",
    "applicationVersion": {
      "version": "1.0.0"
    },
    "title": "{{appTitle}}",
    "description": "{{appDescription}}",
    "dataSources": {
      "sEntityRemote": {
      "uri": "/sap/opu/odata/Entities/",
      "type": "OData",
      "settings": {
        "odataVersion": "2.0"
      }
      }
    }
  },
    "models": {
      "i18n": {
        "type": "sap.ui.model.resource.ResourceModel",
        "settings": {
          "bundleName": "com.myorg.ui5Router.i18n.i18n"
        }
      },
      "sEntities" : {
        "dataSource": "sEntityRemote"
      }
    }
}

```

## Step 04. 실행해보기

MainView.controller.js에서 아래의 소스를 수행해보면,  MockServer에서 제공하는 oData 를 조회하실수 있습니다.

{% code title="MainView.controller.js" %}
```javascript
sap.ui.define([
  "com/myorg/ui5Router/controller/BaseController",
  "sap/ui/model/json/JSONModel"
], function(Controller, JSONModel) {
  "use strict";
  return Controller.extend("com.myorg.ui5Router.controller.MainView", {
    onInit : function(){ 
      var sUri = "/sap/opu/odata/Entities/";
      var oModel = new sap.ui.model.odata.ODataModel(sUri, true);
      
      oModel.read('/sEntities',{
        success : function (param){
          console.log('load odata');
          console.log(param.results);
          sap.ui.getCore().setModel(new JSONModel(param.results),'sModel');
     
          console.log('get odata');
          console.log(sap.ui.getCore().getModel('sModel').getData());
        }
      });
    }
  });
});

```
{% endcode %}

![](../../.gitbook/assets/image%20%2840%29.png)



