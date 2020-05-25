---
description: Local Storage를 활용한 TodoList 만들기
---

# Tutorial 09. TodoList

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



