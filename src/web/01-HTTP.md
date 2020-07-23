# HTTP

대부분의 개발자들이 애플리케이션을 개발하다보면 자연스럽게 HTTP를 경험하게 된다. 다만, 너무 당연히 사용되고 있는 것이다보니 이를 잘 모르고 넘어가는 경우가 많다.

## Protocol

우선 `Protocol`이란 용어를 알고 넘어갈 필요가 있다. `HTTP`를 많이 들어봤지만 "그래서 이게 뭔데?" 라는 생각이 들 수 있다. `HTTP`는 `Protocol`의 일종이며, `Protocol`은 컴퓨터나 원거리 통신 장비 사이에 메세지를 주고 받는 양식과 규칙의 체계이다.

송신자와 수신자 사이에 "메세지 구조는 이런식으로하고, 그건 이런 의미로 약속하고 이런 방식으로 보내기로 하자." 등을 약속한 것이다. 예를 들면, 특정 기업에 이력서를 보낼 때 해당 기업의 이력서 양식에 맞춰 지원을 하는 것이 있을 수 있다.

즉, `Protocol`은 특정 데이터를 주고 받기 위한 약속이며 이를 통해 클라이언트와 서버 사이에 데이터를 주고 받을 수 있는 것이다.

## HTTP(HyperText Transfet Protocol)?

팀 버너스리(Tim Berners-Lee)와 그가 속한 팀은 CERN에서 HTML뿐만 아니라 웹 브라우저 및 웹 브라우저 관련 기술과 HTTP를 발명했다.

`HTTP`는 **HyperText Transfer Protocol**의 약자이며 말 그대로 하이퍼텍스트 문서를 교환하기 위해 사용된 Protocol이다.

`HTTP`의 큰 특징은 다음과 같다.

### 단방향적 통신 프로토콜

`HTTP Protocol`의 가장 큰 특징은 클라이언트가 요청(Request)를 보내면 서버가 응답(Repsonse)하는 단방향적 통신 프로토콜이라는 것이다. 그렇기 때문에 클라이언트가 Request를 서버로 보내지 않으면 서버는 어떠한 데이터도 넘겨주지 않는다.

아마, 앱이나 웹을 개발하다보면 접하게 될 가장 익숙한 패턴의 프로토콜일 것이다.

### Stateless Protocol

`HTTP Protocol`은 상태를 저장하지 않는다. 이러한 특성 때문에 `HTTP Protocol`을 `Stateless Protocol`이라고 한다.

클라이언트에서 요청을 보내면 서버에서는 그에 대한 응답을 클라이언트로 전송하게 된다. 이 때, 응답을 받는 시점에 어떠한 정보도 남기지 않는다. 즉, 클라이언트에서 서버로 요청을 보낼 때마다 각각의 접속은 독립적인 트랜잭션으로 취급된다.

각각의 접속이 독립적인 트랜잭션이라는 것은 많은 사용자가 웹 서비스를 사용하더라도 접속 유지는 최소한으로 할 수 있기 때문에 서버에 동시 접속할 수 있는 Connection의 수보다 더 많은 요청을 처리할 수 있게 된다. 하지만, 클라이언트의 상태를 저장하고 있지 않기 때문에 생기는 문제가 있다.

예를 들어, 클라이언트가 로그인 과정에서 인증 요청을 보내고 그에 대한 응답으로 로그인에 성공했다고 가정하자. `HTTP Protocol`은 이와 관련된 정보를 저장하고 있지 않기 때문에 해당 클라이언트가 인증을 완료한 클라이언트인지 구분할 방법이 없다. 따라서 접속할 때마다 재인증을 해야하는 문제가 생긴다.

`Cookie`와 `Session`을 이용하여 이러한 `Stateless`문제를 보완할 수 있다. 이에 대한 자세한 내용은 [다음](https://github.com/Im-D/Dev-Docs/blob/master/Network/Cookie%EC%99%80%20Session%20%EA%B7%B8%EB%A6%AC%EA%B3%A0%20Redis.md#cookie%EC%99%80-session)을 참고하면 좋을 것 같다.
 

 ## HTTPS와 SSL

 만약 서버와 클라이언트간의 데이터를 누군가가 중간에서 가로챘다고 해보자. 그 데이터를 가로채 변조한다거나 악의적인 감청이 일어난다면 데이터는 단순 텍스트이기 때문에 문제가 생길 것이다.
예를 들어, 로그인을 위해서 서버로 비밀번호를 전송할 때 악의적인 감청이나 데이터의 변조등이 일어날 수 있다는 것이다.

이를 보완한 것이 HTTPS고 이를 이해하기 위해선 약간의 암호화 기법 지식이 필요하다.

### 비대칭키 암호화 기법(공개키 암호화)

---

비대칭키 암호화 기법은 암호화와 복호화를 하기 위한 키가 다르다는 것이다. 즉 암호화를 하는 방법과 복호화를 하는 방법이 다르기 때문에 중간에 암호화된 데이터가 탈취된다고 해도 복호화를 할 방법이 없다. 

![https_asymmetric_key](https://user-images.githubusercontent.com/24209005/88266818-d1b6b300-cd0a-11ea-9792-e53c9b988c3f.png)

하지만 여기서 생각해봐야 할 것이 있는데 **'서버로 전송된 데이터가 클라이언트에서 전송한 것인지, 안전한 데이터라고 확신할 수 있을까?'** 라는 것이다.

이를 위해 우리는 **SSL 인증서**를 사용한다. 서버로 전송된 데이터가 안전한 데이터라는 것을 SSL 인증서를 통해 확인하는 것이다. 그리고 이 인증서를 발급해주는 민간 기관을 **CA**라고 하고 이 **CA 리스트를 브라우저는 내부적으로 가지고 있다.**

즉, 비대칭키 암호화 기법을 이용해 암호화된 데이터를 서버로 전송하는 과정에서 SSL 인증을 받은 데이터만 서버에서는 신뢰하는 것이다.

하지만 이 방식은 느릴 수 있기 때문에 대칭키 암호화와 비대칭키 암호화를 같이 사용하기도 한다.

---

#### 🙏 Reference

- [edwidth - 부스트코스 웹 백엔드](https://www.edwith.org/boostcourse-web-be/lecture/58942/)
- [Im-D/Dev-Docs - HTTP와 WebSocket](https://github.com/im-d-team/Dev-Docs/blob/master/Network/HTTP%20vs%20WebSocket.md)
- [Im-D/Dev-Docs - HTTP와 SSL](https://github.com/im-d-team/Dev-Docs/blob/master/Security/HTTPS%EC%99%80%20SSL.md)
- [비둘기로 설명하는 HTTPS(HTTPS explained with carrier pigeons)](https://www.vobour.com/%EB%B9%84%EB%91%98%EA%B8%B0%EB%A1%9C-%EC%84%A4%EB%AA%85%ED%95%98%EB%8A%94-https-https-explained-with-car)
- [무하프로젝트 - HTTP, HTTPS 프로토콜이란? ](http://mohwaproject.tistory.com/entry/HTTPS%EB%9E%80)
- [HTTPS와 SSL 인증서](https://opentutorials.org/course/228/4894)
