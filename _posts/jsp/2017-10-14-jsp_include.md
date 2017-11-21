---
layout: post
title: jsp include 방법들
published: True
categories: 
- jsp
tags:
- jsp
- 동적 include
- 정적 include
---



신입 프로젝트를 하면서 jsp include하는 방법에 대해 알아본 부분을 정리해봤다.

기존에는 액션태그를 이용하는 동적 jsp include를 많이 사용했는데, 전용태그를 이용하는 정적 jsp include와 비교하여 각각의 장단점을 알게되었다.



## 정적 include와 동적 include

### `<%@ include file="URL" %>`

* jsp 전용태그 지시자로써, 정적 include라고 부른다.
* 부모 jsp 파일이 서블릿 파일로(.java) 변환 되기 전에, 자식 jsp 파일의 모든 내용이 부모 jsp 파일에 include 된 것과 같다.
* _**따라서,**_ 부모 서블릿 파일 내에 부모 jsp 내용과 자식 jsp 내용이 모두 포함되어 컴파일 되므로, 각 jsp 파일들에서 선언한 변수들이 공유된다.
* 그러나 이 방법으로 **많이** include 하면 하나의 서블릿에 여러 jsp에서 선언한 변수들이 공유되므로, 각 변수들이 겹쳐서 오류가 발생하지 않도록 주의해야한다. 
* _**즉,**_ 유지보수시에 어디서 선언된 변수들인지 추적이 어려워 질 수 있다.



* 다음은 정적 include를 실습해본 사진들의 순서이다.
* 자식의 jsp 내용이 모두 포함되어 서블릿 코드로 변환된 것을 볼 수 있고, 변수가 공유되는 것을 확인 할 수 있다.
  1. 자식 jsp 페이지
  2. 부모 jsp 페이지
  3. 변환된 부모 서블릿 파일내 일부 코드
  4. 정적 include 결과

![](/assets/post_images/jsp/children_jsp.PNG)



![](/assets/post_images/jsp/parent_jsp_static.PNG)



![](/assets/post_images/jsp/parent_servlet_static.PNG)



![](/assets/post_images/jsp/static_include_result.PNG)





###`<jsp:include page="URL"></jsp:include>`

* jsp 액션태그로써, 동적 include라고 부른다.
* 부모 jsp 파일과 자식 jsp 파일 각자가 서블릿 자바 파일로 변환되어 컴파일 된 후, html 문서로 실행되어질 시점에서 자식 jsp 파일이 include 되어 웹 브라우저로 보여진다.
* _**즉,**_ 모듈화적인 면에서? 각자가 독립적으로 컴파일되고 include된다.
* 정적 include 와 달리, 하나의 서블릿 자바 파일에 모든 내용이 포함되어 컴파일 되는 것이 아니라서 변수들이 공유 되지 않는다.
* 단, 부모 jsp 파일에서 선언한 변수를 `<jsp:param name="" value=""/>` 를 통해 자식 jsp 파일에서 request.getParameter로 꺼내 사용할 수 있다.



* 다음은 동적 include를 실습해본 사진들이다.
* 자식의 jsp 내용이 포함되는 것이 아니라, 런타임시  RequestDispatcher.include()를 수행하는 코드가 들어가 있는 것을 볼 수 있다.
* 그리고 변수가 공유되지 않으므로, 부모 jsp에서는 str 변수를 resolve할 수 없다는 에러를 볼 수 있다.
  1. 변환된 부모 서블릿 파일내 일부 코드
  2. 부모 jsp 페이지
  3. 동적 include 결과

![](/assets/post_images/jsp/children_jsp.png)



![](/assets/post_images/jsp/parent_jsp_dynamic.png)



![](/assets/post_images/jsp/parent_servlet_dynamic.png)





## `<c:import url="URL"/>`
* jstl을 이용해 jsp 파일을 include 한 방법이다.
* 위에서 언급한 두 가지 방법으로는 할 수 없는 원격지의 jsp 파일을 include 할 수 있다.
* 사실 동적 include 방법을 조금 더 강력하게 만든 방법이라고 한다. 그 중 가장 큰 특징이 원격지 파일 include.
* 변수를 넘겨 주는 방법 또한 `<c:param name="" value=""/>` 으로 비슷하다.






## 정적, 동적 의미?

* 하나의 파일에 모두 포함 시킨 후 컴파일되어 웹 브라우저로 보여지는 부분을 정적이라 표현하고,
* 각각이 컴파일 되어진 것들을 웹 브라우저로 보여질때 include되는 부분을 보고 동적이라고 하는것 같다.



### 정적, 동적 장단점 (feat. 자바 웹 개발 완벽 가이드 141p, 위키북스)

* 정적 include는 하나의 컴파일된 파일이 로드되므로 비교적 속도가 빠를 수 밖에 없다. 그러나 하나의 파일이(구체적으로 _jspServeice 메소드가) 길어지는 단점이 있다. (컴파일 된 자바 메소드의 바이트 코드는 65,534 바이트를 넘길 수 없다고 한다.)
* 동적 include는 이런 바이트 코드 길이에 대해 신경 쓸 필요가 없지만, 페이지 로드시 매번 include 되므로 성능이 떨어질 수 밖에 없다.
* 결국, 상황에 맞게 선택하여 사용해야 하지만, **대부분의 경우 정적 include를 사용하는 것이 좋다고 한다.**

* 얼마나 성능 차이가 나는지는 다음 블로그를 보면 약 38배 차이가 난다고 한다. (feat. http://12bme.tistory.com/135)


