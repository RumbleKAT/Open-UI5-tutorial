---
description: Router를 이용해서 페이지간 이동을 해보자
---

# Tutorial 08. Routing

## 들어가면서 

UI5는 웹페이지 간의 Navigation 기능을 제공합니다. 이것이 바로 Routing입니다. Tutorial 01을 보신 분이라면 UI5가 SPA\(Single Page Application\)이라는 것을 알고 계실 것입니다. SPA의 특징은 바로 화면을 구성하는 html 파일이 index.html 파일 하나밖에 없다는 것이 특징입니다. 

## SPA 브라우저 Routing 원리 

예를 들어, 구글 페이지를 SPA로 구현을 한다고 가정을 해보겠습니다.  구글을 구성하는 여러 페이지 중에서 가장 먼저 보여지는 Main 페이지는 바로 [https://www.google.com/](https://www.google.com/) 의 도메인에 매핑된 구글 검색 창입니다. 

![&#xAD6C;&#xAE00; &#xBA54;&#xC778; &#xD398;&#xC774;&#xC9C0; / www.google.com ](../../.gitbook/assets/image%20%282%29.png)

이 메인 페이지에서 저희는 구글 검색창에 UI5라는 검색어를 친다고 가정을 할게요, 저희는 아래와 같은 화면을 볼 수 있습니다. 여기에서 가장 중요한 것은 바로 브라우저 주소창의 주소 값입니다. SPA는 JSP나 PHP 등과 같이 개발된 MPA\(Multi Page Application\)과 다르게, html 파일이 하나 밖에 없기 때문에 페이지를 구분하는데 있어서, 브라우저 주소창의 값을 기준으로 페이지를 렌더링 합니다. 이 방식은 UI5도 동일하지요. 

![https://www.google.com/search?sxsrf=ALeKk035DHH3fHIS10MwR3wVuC8LNFbMOA%3A1589800505279&amp;source=hp&amp;ei=OW7CXpXtDoamoASiwI2YBQ&amp;q=ui5&amp;oq=ui5&amp;gs\_lcp=CgZwc3ktYWIQAzIECCMQJzIECCMQJzIECCMQJzICCAAyAggAMgIIADICCAAyAggAMgIIADICCAA6BwgjEOoCECdQv\_YMWNuEDWCXjA1oBHAAeACAAXOIAa4EkgEDMC41mAEAoAEBqgEHZ3dzLXdperABCg&amp;sclient=psy-ab&amp;ved=0ahUKEwjV7YOzpL3pAhUGE4gKHSJgA1MQ4dUDCAc&amp;uact=5](../../.gitbook/assets/image%20%281%29.png)

## 간단한 브라우저 라우팅 예제

UI5에서 라우팅을 하는 방법은 여러가지가 있습니다. 라우터 객체를 이용하는 방법과 sap.m.App 객체를 이용하는 방법인데요, 먼저 sap.m.App 객체를 이용하는 방법에 대해서 알아보겠습니다.

sap.m.App 객체를 이용하는 방법은 각 페이지를 구성하는 JS View를 만들고 이를 App 객체에 추가하여 화면에 표시하는 방법입니다. 아래 소스는 index.html에 sap.m.App 객체를 선언하고 initalPage를 설정하여 초기 페이지 설정을 한 것입니다. Go to Second Page라는 버튼을 누르면 Page2로 화면이 전환되고, Page2에서 네비게이션 바 왼쪽 상단의 뒤로가기 버튼을 누르면, 첫번째 페이지로 되돌아갑니다. 

```markup
<!DOCTYPE html>
<html>
  <head>
    <title>OpenUI5 Hello world App</title>
    <script id = "sap-ui-bootstrap"
            src="https://openui5.hana.ondemand.com/resources/sap-ui-core.js"
            data-sap-ui-theme="sap_belize"
            data-sap-ui-libs="sap.m"
            id="sap.ui-bootstrap"
            data-sap-ui-resourceroots='{"views" : "./"}'
            data-sap-ui-xx-bindingsyntax="complex"
            >
     </script>
     <script>
      sap.ui.getCore().attachInit(function(){
        var app = new sap.m.App("myApp",{
          initalPage : "page1"
        });

        var page1 = new sap.m.Page("page1",{
          title : "first page",
          showNavButton : false,
          content : new sap.m.Button({
            text : "Go to Second Page",
            press : function(){
              app.to("page2")
            }
          })
        });

        var page2 = new sap.m.Page("page2",{
          title : "second page",
          showNavButton : true,
          navButtonPress : function(){
            app.back();
          }
        });


        app.addPage(page1);
        app.addPage(page2);
        app.placeAt("content");
      });
     </script>
  
  </head>
<body class="sapUiBody">
  <div id="content"></div>
</body>
</html>
```

### 결과화면 

![&#xCCAB;&#xBC88;&#xC9F8; &#xBA54;&#xC778; &#xD398;&#xC774;&#xC9C0;](../../.gitbook/assets/image%20%2812%29.png)

![&#xB450;&#xBC88;&#xC9F8; &#xC11C;&#xBE0C; &#xD398;&#xC774;&#xC9C0;](../../.gitbook/assets/image%20%2813%29.png)

위의 방법으로도 사실 많은 것을 개발할 수 있습니다. 하지만, Fiori나 UI5를 이용하는 많은 프로젝트는 manifest.json 파일에 각 view 화면의 정보를 매핑하여 라우팅 기능을 제공합니다. 

manifest.json을 쉽게 이용하기 위해서 이제부터는 generator-easy-ui5이라는 ui5 프로젝트 generator를 사용하겠습니다. \(이클립스에 ADT를 사용하셔도 무방합니다.\)

## generator-easy-ui5 설정

