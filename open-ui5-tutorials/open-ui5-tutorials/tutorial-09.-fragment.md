---
description: View에서 반복되는 부분을 Fragement로 만들어 봅니다.
---

# Tutorial 09. Fragment

## 들어가면서

View를 만들면서 반복되는 UI control이 늘어나는 경우가 있습니다. React나 Vue를 예로 들면 이를 template이나 컴포넌트 형태로 View단을 나누어 효율적인 코드 유지보수를 가능하게 지원합니다. UI5에서도 이처럼 비슷한 기능을 제공하는데 바로, Fragment와 Custom controls입니다. 이 둘의 차이점은 Fragment는 단순한 UI control의 묶음으로, Fragment를 호출하는 controller에서 제어할 수 있습니다. 즉, 하나의 컨텐츠를 구성할  View control 요소들만 제공하는 것이지요. 반면, Custom controls의 경우 React의 컴포넌트처럼 사용이 가능합니다. 

