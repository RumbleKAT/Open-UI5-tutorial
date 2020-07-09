---
description: UI5에서 기본적으로 제공하는 UI Element에 대해 알아보자
---

# Tutorial 07. UI Elements

## 들어가면서

UI5는 다양한 UI Element를 제공합니다. SAPUI5는 Fiori라는 SAP 하이브리드 웹앱 플랫폼을 제작하기 위한 엔터프라이즈 라이브러리이기에 OPENUI5보다 많은 종류의 UI Elements를 제공합니다. 만약에, 상업적으로 UI5 어플리케이션을 만들어야한다면, \(SAP 솔루션을 쓰지 않는다는 전제\) open source인  OPEN UI5의 UI elements를 사용해야하 할 것입니다.  물론, 다른 외부 라이브러리를 불러 사용하는 것도 가능합니다.

## OPEN-UI5 UI vs SAP-UI5 UI

간단하게 두 라이브러리의 공식 사이트의 샘플 코드를 기준으로 나누어서 제공하는 UI Element들이 얼마나 다른지 알아보겠습니다. 둘의 큰 차이점은 차트나 프로세스, 지도, 스마트 컨트롤 관련 UI 컴포넌트를 제공하지 않는다는 것에 있습니다.

| OPEN UI5 | SAP UI5 |
| :--- | :--- |
| Input | Input |
| Lists | Lists |
| Tables | Tables |
| Pop-Ups | Pop-Ups |
| Tiles | Tiles |
| Messages | Messages |
| Bars | Bars |
| Trees | Trees |
|  | Smart Controls |
|  | Maps |
|  | Charts |
|  | Processes |
| Object Page | Object Page |
| Dynamic Page | Dynamic Page |
| Flexible Column Layout | Flexible Column Layout |
| Split App | Split App |

## Open-UI5 UI element 사용하기

UI5는 Single Page Application이기 때문에, XML View로 코드를 작성해도 결국에는 JS로 변환이 됩니다. 그러므로 UI5 Element를 사용하기 위해선 공식문서를 참고하는 것이 가장 좋습니다. 간단하게 Input 태그를 XML View와 JS View에서 사용하는 방법을 보면서, UI Element를 사용하는 방법을 익혀봅시다.

## Label 

Label은 SimpleForm 같은 폼에서 Input 태그를 설명하는 역할로 많이 사용되는 태그입니다. Label 태그의 공식문서를 살펴보도록 합시다.

![https://sapui5.hana.ondemand.com/\#/api/sap.m.Label](../../.gitbook/assets/image%20%286%29.png)

### sId

위의 공식문서 캡처본을 보시면 알수 있듯이, UI5의 UI Element는 sId와 mSetting 필드를 제공합니다. sId는 html 태그의 id 값을 의미합니다. 하지만, 이 id 값은 UI5에서 exporting 되면서, 앞에 수식어가 자동으로 붙기 때문에, document.getElementById\('XX'\)로 접근하기 어렵습니다. SAP에선 이러한 UI5 내의 DOM 접근을 위해서, sap.ui.getCore\(\).byId\('xx'\)나 this.getView\(\).byId\('xx'\)와 같은 문법을 제공합니다.

### mSetting

![https://sapui5.hana.ondemand.com/\#/api/sap.m.Label%23controlProperties](../../.gitbook/assets/image%20%288%29.png)

sId만 작성을 하면, 화면 상에는 아무것도 표시 되는 것이 없습니다. UI5에서는 mSetting 속성을 통해서 해당 UI Element 고유의 속성들을 제공합니다.  Label 태그의 경우에는 Label 태그의 글을 보여주는 text property나 꼭 작성해야되는 Label 필드를 표시하기 위한 required 필드 등 여러가지 사용 목적에 따라서 property의 값을 넣어주면 됩니다.

#### Example 

`<Label text="sample" required="true"/>`

![Label tag example](../../.gitbook/assets/image%20%284%29.png)

## Grid 

ui5 에서 Grid를 활용한 element 배치를 배워봅니다. ui5의 Grid는 화면을 12등분을 합니다. 또한, root em을 나타내는 rem단위를 사용하는 측정 시스템을 사용합니다. 이를 통해 사용가능한 화면크기에 따라 다양한 Grid control의 child element를 지정할 수 있습니다. Grid를 사용하면, 데스크탑이나, 태블릿 및 모바일과 같은 여러 디바이스에 대해 유연한 레이아웃 및 줄 바꿈을 사용할 수 있습니다.

Grid Control의 너비는 백분율 또는 픽셀 기반일 수 있으며, 미리 정의 된 다양한 값으로 설정할 수 있습니다. 특히 span을 이용하여 반응형 웹의 너비를 쉽게 구현할 수 있습니다.

defaultSpan 및 defaultIndent는 그리드의 특정 레이아웃을 정의할 수 있는 주요 두 속성입니다. 격자 열의 수는 항상 12이지만, 항목의 범위와 들여쓰기에 따라 한행에 표시되는 수가 결정됩니다.

 UI5에서는 디스플레이 크기에 따라 XL ,L, M, S 로 사이즈가 정의됩니다. 

| 사이즈 | 최대 컬럼수 |
| :--- | :--- |
| S | 4 |
| M | 8 |
| L | 12 |
| XL | 16 |

###  간단한 예시로 Grid layout 알아보기

```markup
<l:Grid defaultSpan="XL3 L4 M6 S12">
  <Label text="A"/>
  <Label text="B"/>
  <Label text="C"/>
  <Label text="D"/> 
</l:Grid>
```



