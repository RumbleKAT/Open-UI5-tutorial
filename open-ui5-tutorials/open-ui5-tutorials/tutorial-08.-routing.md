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



