# Web Broswer의 작동 원리

## Rendering Engine

현대 브라우저들의 렌더링 엔진은 다양하다. gecko기반의 spider monkey를 사용하는 firefox, webkit - blink기반의 V8을 사용하는 chrome, webkit을 사용하는 safari 등 상당히 종류가 많다.

하지만 모든 브라우저는 크게 비슷한 flow로 동작하며 다음의 그림은 그 뼈대인 main flow다.

![mainflow](https://user-images.githubusercontent.com/24724691/62412557-96296180-b63f-11e9-9f0c-fe14a3914629.png)

1. DOM Tree 구축을 위한 HTML parsing
2. Render Tree 구축
3. Render Tree의 배치
4. paint

모든 브라우저는 위의 과정을 거쳐 화면에 그린다.

모든 작업은 UX를 위해 점진적으로 진행된다.

내용을 최대한 빠르게 paint하기 위해 모든 HTML이 parsing 되기를 기다리지는 않고 layout과 paint 일부를 먼저 처리한다.

중요한 것은 위와 같은 flow로 작동한다는 점이다.

### 예시

HTML5rocks라는 구글이 했던 프로젝트의 [How Browsers Work: Behind the scenes of modern web browsers](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)의 유명한 그림이다.

- Webkit Engine 기반

![Webkit](https://user-images.githubusercontent.com/24724691/62412567-bf49f200-b63f-11e9-9ed4-ec8215d04a7d.png)

- Gecko Engine 기반

![Gecko](https://user-images.githubusercontent.com/24724691/62412568-bf49f200-b63f-11e9-86a9-743f6d911e54.png)

이미 너무 예전 그림이며 현대에는 더욱더 복잡한 과정을 거친다. 고전적인 rendering 과정이지만 두 엔진은 용어가 조금 다를 뿐 main flow는 비슷하게 작동함을 알 수 있다.

## Critical Rendering Path(CRP)

위처럼 브라우저에서 화면이 그려지기까지의 주요한 과정을 Critical Rendering Path라고 한다.

1. HTML 마크업을 처리하고 DOM 트리를 빌드한다.
2. CSS 마크업을 처리하고 CSSOM 트리를 빌드한다.
3. DOM 및 CSSOM을 결합하여 Rendering 트리를 형성한다.
4. Rendering 트리에서 레이아웃을 실행하여 각 노드의 기하학적 형태(화면의 위치)를 계산한다.
5. 개별 노드를 화면에 paint한다.

이 과정을 정리한 유명한 글은 Google Developers의 [주요 렌더링 경로](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/?hl=ko)가 있다.

webkit과 gecko, 다른 모든 렌더링 엔진은 이 Critical Rendering Path를 따른다.

### HTML 파싱 / DOM Tree 빌드

컴파일러는 Source Code를 기계어로 변환한다.

![compilation](https://user-images.githubusercontent.com/24724691/62412956-4731fb00-b644-11e9-8614-977ad4a5cc73.png)

위 그림은 그 변환 과정 중 하나인 parsing 과정이다.

문서는 lexer(tokenizer)와 parser(syntax 분석)가 함께 작업하여 tree를 만든다.

브라우저도 마찬가지다.

브라우저는 HTML 문서를 파싱해 DOM Tree를 만든다.
모든 HTML 태그에는 노드가 있고 각각의 노드는 Tree형태로 구현된다.

![Domtree](https://user-images.githubusercontent.com/24724691/62413000-fd95e000-b644-11e9-9fd0-059f49f6cf48.png)

### CSSOM Tree 빌드

HTML 파싱 중 CSS 링크를 만나면 리소스를 받아온다.
같은 프로세스로 CSS도 Tree형태로 만들어진다.

이를 CSSOM(CSS Object Model)이라고 부른다.

![CSSOM](https://user-images.githubusercontent.com/24724691/62413025-52395b00-b645-11e9-9c88-3c3dafedbe46.png)

#### script and css in HTML parsing

HTML parsing과정에서 코드를 읽는 중 script나 css만난 경우 어떻게 처리될까?

- script

  script는 기본적으로 parsing을 중단(block)시킨다.
  script가 외부에 있다면 네트워크 과정을 기다린다.
  이는 HTML4, 5의 spec에도 명시되어 있다.
  이러한 중단을 막고자 4에서는 defer, 5에서는 async라는 옵션이 추가되었다.

  최근에는 외부 script의 parsing은 main parser가 하지 않으며 별도의 쓰레드에서 작업한다.

- css

  이론적으로 css는 dom tree를 수정하지 않기 때문에 block하지 않는다. 하지만 script가 css 정보를 이용해야 하는 경우가 있다. 이 경우 브라우저 엔진이 최적화 작업을 진행하여 문제가 될 경우만 block한다.

### Render Tree 생성

DOM Tree와 CSSOM를 결합하여 Render Tree를 만든다.

![render-tree](https://user-images.githubusercontent.com/24724691/62413187-ee646180-b647-11e9-960f-06d2a85cdcff.png)

![render-tree2](https://user-images.githubusercontent.com/24724691/62413188-ef958e80-b647-11e9-9975-b6bf1c5a8c09.png)

위 그림들은 Render Tree를 추상화 한 그림이다.

Render Tree는 DOM Tree에 있는 것 중에 실제 보이는 것들로만 구성한다.

**ex) style='display : none;'은 Render Tree에서 제외된다. Header 역시 제외된다.**

### Layout(Reflow)

Render Tree가 만들어진 뒤 기기의 viewport를 기준으로 노드들의 정확한 위치와 크기를 계산하는 과정이다.

위치와 관련된 속성(position, width, height 등)들을 계산한다.

**ex) width:100%인 상태에서 브라우저를 resize하면, Render Tree는 변경되지 않고 Layout 이후 과정만 다시 거치게 된다.**

### Paint

실제 웹페이지를 화면에 그리는 작업이다.

렌더러의 "paint" 메서드가 호출된다.

**색이 바뀐다거나 노드의 스타일이 바뀌는 것으로는 Layout 과정을 거치지 않고 Paint만 일어난다.**

#### modern browser

최근의 브라우저들은 Critical Rendering Path에 몇 가지 과정이 추가된다.

1. layout 과정 이후 Update Layer Tree라는 단계가 생겼다.

   Render Object는 layout 이후 정해진 기준에 따라 layer를 나누게 된다.

   여기에서 layer는 포토샵의 layer와 비슷한 개념이다.
   [Layer Model](https://github.com/Im-D/Dev-Docs/blob/master/Browser/Layer_Model.md)로 글을 대체한다.

   Layer는 Render Layer와 Graphics Layer 두 종류가 있는데 이 두 기준으로 Layer Tree를 만든다.

2. 각각의 layer는 paint 과정을 거친 후 composite layers이라는 새로운 과정을 거친다.

   layer들을 합성하여 하나의 bitmap으로 만든 뒤 최종 page를 만든다.

#### :pray: Reference

- [edwidth - 부스트코스 웹 백엔드](https://www.edwith.org/boostcourse-web-be/lecture/58944/)
- [How Browsers Work: Behind the scenes of modern web browsers](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)
- [Naver D2 - 브라우저의 작동 원리(위 글 번역)](http://d2.naver.com/helloworld/59361)
- [critical rendering path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/?hl=ko)
- [웹 성능 최적화에 필요한 브라우저의 모든 것](https://tv.naver.com/v/4578425)
