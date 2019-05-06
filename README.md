
상하 스크롤 시 데이터 append / prepend

                        스크롤 시 append 방식

                        일정 dom 수를 유지
                        하지만 가져온 데이터들은 계속 쌓이는 방식
                        상품 클릭 시 가지고 있던 모든 데이터를(인피니티스크롤 정보) Web Storage에 저장
                        뒤로가기 시 인피니티 스크롤 정보를 바탕으로 다시 그려줌

                        모바일 작업 시 PC와는 다르게
                        상품유닛 뷰 타입도 다양
                        한번에 더 많은 상품을 가져오다 보니 이미지를 한번에 요청하고 화며에 다 그려주기까지
                        시간이 오래 걸림
                        일단 빈 이미지로 로딩 후 화면에 보이는 부분만 이미지를 가져오도록
                        레이지 로딩 적용
                        기존 스크롤값을 이용한 레이지 로딩 대신
                        인터렉션 옵저버 API를 사용



                        500개에서 1000개 상품 수 증가
                        처음에는 크게 이슈가 없으나 페이지가 뒤로 갈수록 저장할 데이터는 증가

                        결국 저정할 정보들이 증가하게 되면
                        해당 용량 초과 발생

                        그래서 상품 클릭 시 전체 데이터를 저장하는것이 아니라
                        클릭한 페이지와 이전 다음 페이지의 데이터만 저장

                        뒤로가기 후 스크롤 시 해당 페이지의 데이터가 없다면
                        위로 스크롤 시 prepend 데이터 요청 후 추가
                        아래로 스크롤 시 append 데이터 요청 후 추가

                        다시 데이터를 요청후 가져오도록 기능 수정



# Skeleton Screen

스켈레톤 스크린(skeleton screens)


<img src="ssg_loding.png" alt="">
로딩 인디케이터
로딩 인디케이터는 사용자에게 기다려 달라는 의미이지만 텅빈 화면을 오래 봐야 하는 단점

## 빈 레이아웃을 먼저 보여줘라.
페이지의 뼈대에 해당하는 레이아웃 (Skeleton Screens)을 먼저 보여주고 데이터를 받는 대로 레이아웃을 부분적으로 채워가는 방식




스켈레톤스크린? 해골화면? 하면서 의아하실 것같습니다.스켈레톤 스크린은 유저가 로딩시간을 기다릴 동안 "덜 지루하게" "좀 더 빨라 보이도록"하는 기법들 중 하나 입니다.

 더 자세히 말씀드리면 이를 통해서 빈화면이나 로딩마크가 아닌 내가 받게될 컨텐츠(정보)가 "이런 모양이고" "이정도 분량"이고 "여기까지 진행되었어"를 사용자에게 보여주는거죠. 
 요즘은 인터넷이 빠른 수준이 아니라 사용자와 실시간으로 소통을 할 수 있을 정도로 빨라졌습니다. 속도가 빨라진 만큼 사용자들이 서비스로부터 피드백이 오기까지 기다리는 시간도 짧아지고, 라이프스타일 자체도 굉장히 속도가 빨라졌다고 봅니다. 그렇기 때문에 현재까지 사용하고 있는 로딩표시나, 로딩이 몇퍼센트 되었다라는 로딩마크만으로는 지루해 하고 있는 사용자를 달래기엔 부족해다고 봅니다. 그렇기 때문에 로딩마크 뿐 아니라, 현재 진행 과정을 보여줌으로써 "진행되고 있음" 과 "지금의 상황"을 스켈레톤 스크린으로 좀 더 사용자가 쾌적한 경험을 할 수 있도록 개선하는 기법입니다. 


스켈레톤 스크린 정의
스켈레톤 화면은 텍스트 및 이미지와 같은 콘텐츠가 점차적으로 채워지는 빈 페이지
일반적으로 자리 표시 자라고하는 회색 또는 중립적 인 채워진 모양은 클릭 유도 문안 또는 링크와의 사용자 상호 작용시 즉시 사용자를 만나게됩니다. 
그런 다음 자리 표시 자 (골격의 소위 "뼈")가 실제 사이트 내용으로 대체되고 환상이 완료됩니다. 이것이 바로 스켈레톤 스크린이하는 일입니다


<img src="facebook.png">
Facebook의 뉴스 피드로드 상태

<img src="youtebe.png">
YouTube의 홈 화면로드 상태



# MutationObserver API

나는 최근에 JavaScript MutationObserver API를 사용하여 매우 인상적이었습니다. 나는 이미 코드를 사용하여 코드를 정리할 수있는 모든 장소를 이미 고려하고 있습니다. 

MDN 은 MutationObserver 인터페이스를 다음과 같이 설명합니다 :
MutationObserver 인터페이스는 DOM 트리에 대한 변경 사항을 감시하는 기능을 제공합니다.

지원범위
지원도 좋습니다 . 바탕 화면의 IE11 플러스 초록색 브라우저로 돌아 가기. 모바일에서는 Android 4.4> 및 iOS6입니다.
https://caniuse.com/#feat=mutationobserver

```js
// observer instance 
var observer = new MutationObserver(function(mutations) { 
    mutations.forEach(function(mutation) { 
    console.log(mutation); 

    // include your code for reacting to each mutation here 

    });  
}); 

// target node 
var target = document.getElementById('my-id'); 

// configuration 
var config = { attributes: true, childList: true, characterData: true }; 

// start observer 
observer.observe(target, config); 

// stop observer -- use this when you want to stop observing 
// observer.disconnect(); 
```

<img id="some" src="">

```js
var target = document.querySelector('#some');

// create an observer instance
var observer = new MutationObserver(function(mutations) {
  mutations.forEach(function(mutation) {
    console.log(mutation.type);
    observer.disconnect();
    mutation.target.src = "http://img.naver.net/static/www/u/2013/0731/nmms_224940510.gif";
    mutation.target.onload = function() {
      observer.observe(mutation.target,config);
    };
  });
});

// configuration of the observer:
var config = { attributes: true, childList: true, characterData: true };

// pass in the target node, as well as the observer options
observer.observe(target, config);

target.src ="http://icon.daumcdn.net/w/icon/1312/19/152729032.png";
```

이렇게하면 daum 이미지를 src 에 넣더라도 곧바로 naver 로 대체되어 보여진다(daum 이미지는 순간으로도 안보이고 곧바로 naver 이미지가 보임)
단, src 를 observe 하고 있었는데, src 를 또 바꾸면 무한호출이 이루어진다. 그렇기 때문에 일단 바꾸기전에 observe 를 끄고
바꾸고 난다음에 로딩이 완료되면 다시 켜도록 해야한다.







# Intersection Observer API


2017년은 브라우저에서 내장된 도구들이 굉장히 좋았던 해였다. 
이 도구들은 많은 수고를 들이지 않고 코드베이스의 스타일과 품질을 개선하는데에 도움을 주었다. 
요즘 웹은 옵저버 인터페이스(또는 "옵저버(Observers)"라고 부른다)의 잘 정의된 접근 방식에 기반해 산발적인 해결 방법에서 벗어나려는 것처럼 보인다. 
지원이 잘 되고있는 MutationObserver는 모던 브라우저에 빠르게 채택된 몇 가지 옵저버들을 새 가족으로 맞이하게 되었다.

IntersectionObserver
PerformanceObserver (Performance Timeline Level 2 명세의 한 부분)
FetchObserver 옵저버는 현재 개발중이며, 우리를 네트워크 프록시의 영역으로 안내한다. 
하지만 오늘은 프론트엔드 영역에 대해 더 많이 이야기하고 싶다.

IntersectionObserver와 PerformanceObserver는 옵저버 가족의 새로운 멤버가 되었다.


PerformanceObserver와 IntersectionObserver는 프론트엔드 개발자들이 그들의 프로젝트 성능을 개선하는데에 여러 부분에서 도움을 준다. 
전자(PerformanceObserver)는 실제 사용자 모니터링에 대한 도구를 제공하며, 
후자(IntersectionObserver)는 도구로 사용되면서 평가 가능한 성능 개선을 제공한다. 



## 옵저버 vs. 이벤트
이름에서 알 수 있듯이, 옵저버(Observer)는 페이지 컨텍스트 안에서 일어나는 일을 관찰하기 위한 것이다. 
옵저버는 DOM 변경처럼 페이지에서 발생하는 어떤 일들을 감시할 수 있다. 
또한 페이지의 생명주기 이벤트(lifecycle events)를 감시할 수 있다. 
옵저버는 어떤 콜백 함수를 실행할 수도 있다. 


## 기존 scroll 이벤트의 문제
웹사이트를 개발할 때 특정 위치에 도달했을 때 어떤 액션을 취해야 한다면 어떻게 구현할 수 있을까요? 보통 addEventListener()의 scroll 이벤트가 먼저 떠오릅니다. document에 스크롤 이벤트를 등록하고, 특정 지점을 관찰하며 엘리먼트가 위치에 도달했을 때 실행할 콜백함수를 등록하는 것이죠.

scroll 이벤트는 단시간에 수백번, 수천번 호출될 수 있고 동기적으로 실행되기 때문에 메인 스레드(Main Thread) 영향을 줍니다. 
한 페이지 내에 여러 scroll 이벤트(무한 스크롤, 광고 배너, 애니메이션 등)가 등록되어 있을 경우, 각 엘리먼트마다 이벤트가 등록되어 있기 때문에 사용자가 스크롤할 때마다 이를 감지하는 이벤트가 끊임없이 호출됩니다. (디바운싱(Debouncing)과 쓰로틀링(Throttling)을 통해 이러한 문제를 개선시킬 수도 있습니다.) 

그리고 특정 지점을 관찰하기 위해서는 getBoundingClientRect() 함수를 사용해야 하는데, 이 함수는 리플로우(reflow) 현상이 발생한다는 단점

리플로우(reflow): 리플로우는 브라우저가 웹 페이지의 일부 또는 전체를 다시 그려야하는 경우 발생



 IntersectionObserver를 활용하면 기존에 사용하고 있던 scroll 이벤트 중 IntersectionObserver로 구현할 수 있는 경우에는 기존 코드 대비 성능을 개선할 수 있고, 그 외에도 웹에 게시된 광고의 수익을 활용하는 등 다양한 영역에 활용할 수 있을 것 같습니다. 


## Intersection Observer API의 등장
Intersection Observer API(교차 관찰자 API)를 사용하면 위와 같은 문제를 해결
비동기적으로 실행되기 때문에 메인 스레드에 영향을 주지 않으면서 변경 사항을 관찰
또한 IntersectionObserverEntry의 속성을 활용하면 getBoundingClientRect()를 호출한 것과 같은 결과를 알 수 있기 때문에 따로 getBoundingClientRect() 함수를 호출할 필요가 없어 리플로우 현상을 방지



```
const options = {
    root: null,
    rootMargin: '0px 0px 0px 0px',
    threshold: 1.0
 };
// 기본구조는 콜백함수와 옵션
const observer = new IntersectionObserver(callback, options);

```

## callback
타겟 엘리먼트가 교차되었을 때 실행할 함수

## options

### root
교차 영역의 기준이 될 root 엘리먼트. 
observe의 대상으로 등록할 엘리먼트는 반드시 root의 하위 엘리먼트

root속성 표시 영역으로 설정 될 요소를 정의
이 속성을 정의하지 않으면 자동으로 브라우저 뷰포트가 사용

요소를 타겟팅하고 높이가 자동인 경우 모든 요소가 표시되도록 설정
따라서 요소를 설정하면 자동이 아닌 높이를 설정하십시오. 그렇지 않으면 예상 한 것처럼 작동하지 않습니다. 

### rootMargin
root 엘리먼트의 마진값. 
css에서 margin을 사용하는 방법으로 선언할 수 있고, 축약도 가능하다. px과 %로 표현할 수 있습니다. rootMargin 값에 따라 교차 영역이 확장 또는 축소된다.

### threshold
이 값이 0이면 요소의 1 픽셀이 root 요소 안에있을 때 콜백을 실행
값이 1.0이면 root요소 내부에 100 % 콜백이 트리거
엘리먼트가 root엘리먼트 내에서 50% 일 때 콜백이 호출되기를 원한다면 , 값 0.5

0.0부터 1.0 사이의 숫자 혹은 이 숫자들로 이루어진 배열로, 
타겟 엘리먼트에 대한 교차 영역 비율을 의미
0.0의 경우 타겟 엘리먼트가 교차영역에 진입했을 시점에 observer를 실행하는 것을 의미 
1.0의 경우 타켓 엘리먼트 전체가 교차영역에 들어왔을 때 observer를 실행하는 것을 의미


## Methods

* IntersectionObserver.observe(targetElement)
타겟 엘리먼트에 대한 IntersectionObserver를 등록할 때(관찰을 시작할 때) 사용

* IntersectionObserver.unobserve(targetElement)
타겟 엘리먼트에 대한 관찰을 멈추고 싶을 때 사용하면 됩니다. 
예를 들어 Lazy-loading(지연 로딩)을 할 때는 한 번 처리를 한 후에는 관찰을 멈춰도 됩니다. 

* IntersectionObserver.disconnect()
다수의 엘리먼트를 관찰하고 있을 때, 이에 대한 모든 관찰을 멈추고 싶을 때 사용

* IntersectionObserver.takerecords()
IntersectionObserverEntry 객체의 배열을 리턴


## Properties
IntersectionObserverEntry 객체

위에서 IntersectionObserver에 대해 설명했을 때 
IntersectionObserver에서 반환하는 callback은 IntersectionObserverEntry 객체의 배열을 반환한다고 했는데요. 
IntersectionObserver를 사용할 때 반환되는 이 객체의 정보는 어떤 동작을 등록하거나 할 때 유용하게 사용할 수 있습니다.

### Properties
사각형의 크기를 반환하는 속성

* IntersectionObserverEntry.boundingClientRect: 타겟 엘리먼트의 정보를 반환

* IntersectionObserverEntry.rootBounds: root 엘리먼트의 정보를 반환 (root+ rootMargin) 

* IntersectionObserverEntry.intersectionRect: 교차된 영역의 정보를 반환



* IntersectionObserverEntry.intersectionRatio: 
IntersectionObserver 생성자의 options의 threshold와 비슷합니다. 교차 영역에 타겟 엘리먼트가 얼마나 교차되어 있는지(비율)에 대한 정보를 반환합니다. threshold와 같이 0.0부터 1.0 사이의 값을 반환합니다.

* IntersectionObserverEntry.isIntersecting: 타겟 엘리먼트가 교차 영역에 있는 동안 true를 반환하고, 그 외의 경우 false를 반환합니다.

* IntersectionObserverEntry.target: 타겟 엘리먼트를 반환

* IntersectionObserverEntry.time: 교차가 기록된 시간 반환

<img src="intersectionobserverentry.png" alt="">


#Lazy-Loading 예제

## Intersection observer 사용
이전에 지연 로딩 코드를 작성한 적이 있다면 scroll이나 resize와 같은 이벤트 핸들러를 이용하여 작업했을 것입니다. 
이러한 접근 방식이 여러 브라우저에서 가장 호환성이 좋긴 하지만, 현대의 브라우저는 Intersection observer API를 통해 요소 확인 작업을 수행하는 더욱 우수하고 효율적인 방식을 제공





Observer | Javascipt Observer 자료 정리

# IntersectionObserver API

* https://w3c.github.io/IntersectionObserver
* https://github.com/adamrasheed/IntersectionObserver
polyfill
* https://github.com/adamrasheed/IntersectionObserver/tree/master/polyfill





https://github.com/codepink/codepink.github.com/wiki/%EB%84%88%EB%8A%94-%EB%82%98%EB%A5%BC-%EB%B3%B8%EB%8B%A4:-%EC%A7%80%EC%97%B0-%EB%B0%A9%EB%B2%95,-%EB%A0%88%EC%9D%B4%EC%A7%80-%EB%A1%9C%EB%93%9C%EC%99%80-IntersectionObserver%EC%9D%98-%EB%8F%99%EC%9E%91

https://tech.lezhin.com/2017/07/13/intersectionobserver-overview




Intersection Observer API와 lozad.js 로 이미지 지연 로딩하기

WebIntersection Observer API
* https://rhostem.github.io/posts/2017-10-21-lazy-image-loading-with-lodzad-js/


이미지가 많은 웹사이트에서는 브라우저 화면상에 표시되지 않은 영역의 이미지의 로딩을 의도적으로 지연시켜서 네트워크 트래픽과 성능에 도움을 주는 방법을 많이 사용한다.

그런데 지연 로딩(lazy loading)이라고 불리는 이 기법을 구현하는 전통적인 방식에는 문제가 있다. 브라우저 스크롤 이벤트에 리스너를 붙여서 현재 스크롤의 위치와 getBoundingClientRect같은 API를 사용해서 얻은 엘레멘트의 위칫값을 주기적으로 비교해야 하기 때문이다.

최근 브라우저에 추가된 API인 Intersection Observer API를 사용하면 이처럼 리소스 소모가 많은 방식을 대체할 수 있다.

Intersection Observer API
Intersection Observer API는 이름 그대로 브라우저 뷰포트와 엘레멘트의 교차점(intersection)과 관련된 기능이다. 뷰포트에 엘레멘트가 표시되면 옵저버는 이벤트를 발생시키기 때문에 개발자는 콜백 함수에서 원하는 작업을 수행할 수 있다.

사용법은 그렇게 복잡하지 않다. 옵션과 함께 옵저버 인스턴스를 생성하고 탐지할 엘레멘트를 지정하면 된다. 상세한 사용법은 MDN의 API 문서를 살펴보면 알 수 있다.

const options = {
  root: document.querySelector('#scrollArea'), // null은 브라우저 전체 영역
  rootMargin: '10% 10px', // 요소가 감지되는 영역을 추가하거나 줄일 수 있다. CSS margin 같은 속성에 사용하는 축약형처럼 작성할 수 있다.
  threshold: 0, // 콜백을 실행시킬 지점. 0은 보이자마자, 1은 모두 표시된 후
}

const callback = (entries, observer) => {
  entries.forEach(entry => {
    // entry는 탐지된 엘레멘트의 정보를 가지고 있다.
    // ex)
    //    entry.isIntersecting,
    //    entry.boundingClientRect
  })
}

const observer = new IntersectionObserver(callback, options); // 옵저버 인스턴스 생성

const target = document.querySelector('#listItem'); // 탐지할 엘레멘트

observer.observe(target);
lozad.js로 간단하게 Intersection API 사용하기
lozad.js
lozad.js는 이미지 지연 로딩에 특화된 Intersection API 구현 사례라고 할 수 있다. 복잡한 설정 필요 없이 이미지 태그에 라이브러리가 요구하는 속성을 추가한 후 옵저버 인스턴스를 생성하면 이미지 지연 로딩이 간단히 구현된다.

image 태그에는  src 속성 대신  data-src에 이미지의 경로를 할당하고 lozad라는 이름의 클래스 이름을 추가해야 한다. (클래스명은 인스턴스 옵션으로 바꿀 수 있다)

<img class="lozad" data-src="image.png" />
const observer = lozad(); // 옵션을 지정하지 않으면 lozad라는 클래스가 붙은 이미지 태그를 지연 로딩한다.
observer.observe();
이것이 전부다. lozad.js는 앞서 설명한 Intersection Observer를 사용하는 과정에서 타겟 요소를 설정하고 이미지 불러오는 로직을 자동으로 처리해주기에 지연 로딩을 무척 간단히 구현할 수 있다. 물론 Intersection API에서 제공하는 rootMargin, threshold, 콜백 함수도 사용할 수 있다.

예제
lozad.js를 사용해 간단히 만들어 본 예제 페이지에서는 이미지가 로드된 후 콜백 함수에서 페이드인 효과를 추가하고, 토스트 메시지를 표시하도록 했다.

https://rhostem.github.io/lozad-example

polyfill 설치
Intersection Observer의 브라우저 지원 현황
http://caniuse.com/#feat=intersectionobserver

2017년 10월 현재 크롬 최신 버전에서는 사용할 수 있지만 사파리 브라우저는 데스크탑, 모바일 모두 지원하지 않는다. 많은 웹 브라우저가 지원하지 않기 때문에 polyfill 설치가 필수적이다.

polyfill 설치를 위해서는 스크립트 직접 추가하거나, NPM 모듈을 설치한 후 추가하는 방법이 있다.

<!-- 메인 스크립트보다 먼저 불러와야 한다. -->
<script src="path/to/intersection-observer.js"></script>
// 가능하면 entry 파일에서 다른 모듈보다 먼저 불러와야 한다.
require('intersection-observer');














