2019 상반기 업무 리뷰

# The Agenda
- Observer API
- 무한 스크롤
- 스켈레톤 스크린

# Observer API

## 옵저버 패턴
- 한 객체의 상태가 바뀌면 다른 객체들에게 알려주고 자동으로 내용이 갱신되는 방식
- ex. 엑셀 표) 사용자가 스프레드시트 값 변경 → 표, 그래프, 차트 변화. 이때 모든 요소들이 동시에 즉각적으로 반응
<img src="img/observer.png" alt="" width="50%">

## 옵저버
페이지에서 발생하는 어떤 일들을 감시

## setInterval API
일정 시간마다 반복해서 변경사항이 있는지 확인

- 정확한 지연시간을 보장해 주지 않음
- 여러 개의 타이머 사용 시 웹 사이트 성능 저하 원인
- 메모리 절약을 위해 더 이상 필요하지 않을 때 취소시키는 것이 중요

```js
var timeId = setInterval(function() {
    //...
}, 5000); // 5초 마다 실행

clearInterval(timeId);
```

## JS Observers
최신 브라우저에서 제공하는 옵저버 목록

### DOM 정보 관찰
- MutationObserver - 엘리먼트의 변경 사항 감시
- IntersectionObserver - 뷰포트와 엘리먼트의 교차 감시
- ResizeObserver - 엘리먼트의 너비, 높이 변화 감시

### 성능 측정, 코드 상태 확인
- PerformanceObserver - 웹 성능 측정 데이터를 효율적으로 액세스
- ReportingObserver - 웹 사이트의 잠재적인 문제를 발견하고 모니터링



## 이전의 MutationEvents
성능 저하 및 안정성 문제로 DOM 이벤트 표준에서 지원 중단. 일부 브라우저는 여전히 지원

- DOMAttrModified
- DOMAttributeNameChanged
- DOMCharacterDataModified
- DOMElementNameChanged
- DOMNodeInserted
- DOMNodeInsertedIntoDocument
- DOMNodeRemoved
- DOMNodeRemovedFromDocument
- DOMSubtreeModified

## 옵저버 vs. 이벤트

- 옵저버는 비동기 넌블로킹(non-blocking) API
- DOM 변경이 완료 될 때까지 콜백함수가 작동하지 않음
- scroll, resize 비용이 큰 EventListener 대체
- 어떤 옵저버는 사전 계산된 속성을 제공 (직접 계산 불필요)
- 이벤트는 호출 될 때 콜백이 시작되며 발생할 때마다 동기적으로 반응
- 이벤트에서 getClientBoundingRect() 처럼 성능에 비용이 많이 드는 메서드와 속성들을 사용해 직접 계산
- 이벤트는 캡처링, 버블링을 통해 전파, 더 많은 이벤트 발생


## MutationObserver
노드가 새로 추가되거나 제거, 속성, 텍스트 내용 등 DOM 요소의 변경 사항을 감시하는 API

### 사용 방법
MutationObserver 인스턴스 생성. DOM 변경을 감지했을 때 실행 할 콜백함수

```js
var observer = new MutationObserver(callback);
```

### 메서드
- observer.observe(target, config) - 대상 요소 감시 시작 (감시할 DOM 노드, 옵션)
- observer.disconnect() - DOM 변경 사항 감시 중지
- observer.takeRecords() - 보류중인 DOM 변경 사항 목록을 반환, 동시에 레코드 대기열 비움


### 감시 시작
observe 메서드를 사용해 감시 시작. 감시할 대상과 옵션들을 설정 후 옵저버에 전달
```js
// 감시할 대상 선택
var target = document.getElementById('some-element');

// 감시할 옵션
var config = {
    childList: true,
    characterData: true
};

// 감시 시작 (감시 대상과 옵션 전달)
observer.observe(target, config);
```

### 감시 옵션
- 어떤 내용을 감시할 지에 대한 설정 (MutationObserverInit)
- childList, attributes, characterData 속성 중 하나 true. 그렇지 않으면 TypeError 예외 발생
```js
var config = {
    childList: true,
    attributes: true,
    characterData: false,
    subtree: false,
    attributeFilter: ['class', 'src'],
    attributeOldValue: false,
    characterDataOldValue: false
};
```

### MutationObserverInit
<table>
    <colgroup>
    <col style="width:20%">
    <col style="width:10%">
    <col style="width:70%">
    </colgroup>
    <thead>
        <tr>
            <td>속성</td>
            <td>타입</td>
            <td>설명</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>childList</td>
            <td>Boolean</td>
            <td>대상 노드 하위 요소의 추가 및 제거<br>(텍스트 포함)</td>
        </tr>
        <tr>
            <td>attributes</td>
            <td>Boolean</td>
            <td>대상 노드 속성 변경</td>
        </tr>
        <tr>
            <td>characterData</td>
            <td>Boolean</td>
            <td>대상 노드 텍스트 값에 대한 변경 사항</td>
        </tr>
        <tr>
            <td>subtree</td>
            <td>Boolean</td>
            <td>대상 노드 자식 자손까지 모두 감시</td>
        </tr>
        <tr>
            <td>attributeOldValue</td>
            <td>Boolean</td>
            <td>변경 전 속성 값</td>
        </tr>
        <tr>
            <td>characterDataOldValue</td>
            <td>Boolean</td>
            <td>텍스트 (문자 데이터) 이전 값</td>
        </tr>
        <tr>
            <td>attributeFilter</td>
            <td>Array</td>
            <td>지정된 속성만 감시<br>(모든 속성을 관찰 할 필요가 없는 경우)</td>
        </tr>
    </tbody>
</table>

### 콜백 함수
- DOM 변경이 발생 할 때마다 실행되는 함수
- MutationObserverInit을 사용하여 감시 할 내용 설정. 콜백에서 Mutation​Record 객체를 통해 변경 내용 확인
```js
var callback = (mutations, observer) => {
    mutations.forEach(mutation => {
        console.log(mutation); // 변경된 내용 정보
        if (mutation.type === 'childList' && mutation.addedNodes.length) {

        }
    });
};
```

### Mutation​Record
<table>
    <colgroup>
    <col style="width:20%">
    <col style="width:80%">
    </colgroup>
    <thead>
        <tr>
            <td>속성</td>
            <td>설명</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>type</td>
            <td>어떤 종류가 변경되었는지 확인<br>attributes || childList || characterData</td>
        </tr>
        <tr>
            <td>attributeName</td>
            <td>변경된 속성명</td>
        </tr>
        <tr>
            <td>attributeNamespace</td>
            <td>변경된 속성 네임스페이스</td>
        </tr>
        <tr>
            <td>addedNodes</td>
            <td>추가된 노드</td>
        </tr>
        <tr>
            <td>removedNodes</td>
            <td>제거된 노드</td>
        </tr>
        <tr>
            <td>nextSibling</td>
            <td>추가, 삭제 된 노드의 다음 형제</td>
        </tr>
        <tr>
            <td>previousSibling</td>
            <td>추가, 삭제 된 노드의 이전 형제</td>
        </tr>
        <tr>
            <td>oldValue</td>
            <td>속성 또는 데이터 이전 값</td>
        </tr>
        <tr>
            <td>target</td>
            <td>대상 DOM 요소</td>
        </tr>
    </tbody>
</table>

### 감시 중지
모니터링하는 프로세스가 증가하면 증가할수록 부하. 작업이 끝난 후 더이상 감시 할 필요가 없어지면 중단
```js
observer.disconnect();
```

### takeRecords()
감시를 중단 할 때, 아직 보류 중인 모든 변경 내용을 처리
```js
var mutations = observer.takeRecords();
// 아직 처리되지 않은 모든 변경 사항 레코드를 가져와서 처리 후 중단

if (mutations) {
    callback(mutations);
}
// DOM 감시를 중단 하기 위해 disconnect() 호출 하기 전 수행

observer.disconnect();
```

### 다국어 프로젝트
언어 변경 체크를 위해 Mutation Observer API 사용

- 구글 웹 사이트 번역기 사용
- 개발 시 필요한 API 지원이 안되는 문제
- 언어 변경 후, 변역이 완료되면 다국어 처리(i18n), 페이지 새로고침을 위한 callback 함수 필요
- 특정 엘리먼트를 감시하고 있다가 변경 사항이 있으면 이전 텍스트와 현재 텍스트 비교

### placeholder 번역 대응
- 기존 placeholder 값을 임시 DOM 안에 넣은 후 옵저버로 감시
- 번역이 완료 되면 placeholder 속성을 변역된 언어로 변경


### 브라우저 지원
- [Mutation Observer](https://caniuse.bitsofco.de/embed/index.html?feat=mutationobserver&amp;periods=future_1,current,past_1,past_2)
- [MutationObserver Polyfill](https://github.com/megawac/MutationObserver.js)


## IntersectionObserver
뷰포트와 DOM 요소 간의 교차하는 부분의 변화를 비동기적으로 감시하는 API


### Scroll Event
- 스크롤 이벤트는 동기적으로 실행 (메인 스레드에 영향)
- 스크롤 할 때마다 이벤트가 끊임없이 호출
- Debouncing, Throttling을 통해 스크롤 이벤트 함수 호출 빈도 조정
```js
$(window).on('scroll', function() {
  // 1. 각 이미지가 현재 뷰포트에 존재하는지 확인 후
  // 2. 이미지 로드
});
```

### 뷰포트 요소 확인
스크롤 시 해당 요소가 화면에 들어왔는지 위치 확인을 위해 수동으로 계산

<img src="img/scrolltop.png" alt="" width="50%">

> getBoundingClientRect() 요소 크기와 뷰포트에서 요소의 상대적인 위치 반환. 이 함수는 리플로우(레이아웃) 현상이 발생한다는 단점

```js
function isElementInViewport (el) {
  var rect = el.getBoundingClientRect();
  return (
    rect.top >= 0 &&
    rect.left >= 0 &&
    rect.bottom <= (window.innerHeight || document.documentElement.clientHeight) &&
    rect.right <= (window.innerWidth || document.documentElement.clientWidth)
  );
}
```

> 이러한 모든 배치 관련 계산이 메인 쓰레드에서 수행

<img src="img/scroll_inview.png" alt="" width="80%">


## IntersectionObserver
> scroll 이벤트는 여러 브라우저에서 가장 호환성이 좋긴 하지만, 최신 브라우저는 요소 확인을 IntersectionObserver를 통해 더 뛰어난 성능과 효율적인 방식 제공

- 대상 요소 위치를 지속적으로 계산할 필요 없음
- 대상 요소가 뷰포트에 들어오거나 벗어날 때 콜백 제공

### 사용 방법
IntersectionObserver 객체의 생성자는 두 개의 파라미터 사용

- 콜백은 요소가 화면에 교차 할 때마다 실행되는 함수
- 옵션은 다양한 교차 지점을 설정 할 수 있는 사용자 정의 객체

```js
var observer = new IntersectionObserver(callback, options);
```

### 감시 옵션
root, rootMargin, threshold 세 가지 옵션을 통해 교차 지점 설정

```js
var options = {
    root: null,
    rootMargin: '0px 0px 0px 0px',
    threshold: 0
};
```

### root
- 교차 영역의 기준이 될 요소 지정
- 이 속성을 정의하지 않으면 기본으로 브라우저 뷰포트 사용

<img src="img/root.png" alt="" width="80%">

```js
var options = {
    root: null // document.querySelector('.container')
};
```

### rootMargin
- root 요소의 여백
- 뷰포트를 확장 또는 축소
- px 또는 %로 표시, 축약 가능 (css margin과 동일)

<img src="img/rootMargin.png" alt="" width="80%">

```js
var options = {
    rootMargin: '0px 0px 0px 0px'
};
```

### threshold
- 0.0 ~ 1.0 사이의 숫자 또는 이 숫자들로 이루어진 배열
- 콜백이 실행 되어야하는 시점 지정
- 0.5: 타겟이 root 요소 내부에 50% 교차 할 때 콜백 실행

<img src="img/threshold.png" alt="" width="80%">

```js
var options = {
    threshold:  0.5
    // threshold: [0, 0.2, 0.4, 0.6, 0.8, 1.0]
    // 교차 영역이 20% 변경 될 때마다 callback 실행
};
```

### 메서드
- observer.observe(targetElement) - 대상 요소 감시 시작
- observer.unobserve(targetElement) - 특정 요소에 대한 감시 중지
- observer.disconnect() - 다수의 요소를 감시하고 있을 때, 모든 감시 중지
- observer.takeRecords() - 교차하는지 여부에 관계없이 모든 감시 대상 목록 반환

### 감시 시작
옵저버가 해당 요소를 감시 할 수 있도록 observe 메서드에 대상 요소 전달

```js
// 대상 요소는 root 요소의 자식이어야 됨
var target = document.getElementById('some-element');
observer.observe(target);


// 여러 대상을 추적해야하는 경우 각 대상을 개별적으로 추가
var images = document.querySelectorAll('.lazy');
images.forEach((el) => {
    observer.observe(el);
});
```

### 콜백 함수
해당 요소가 화면에 들어오거나 벗어날 때마다 콜백 함수 호출. 콜백 함수는 IntersectionObserverEntry 배열 객체를 받음

```js
var callback = (entries, observer) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            // 타겟이 화면에 보이면
            entry.target.classList.add('box--visible');
        } else {
            // 타겟이 화면을 벗어나면
            entry.target.classList.remove('box--visible');
        }
    });
};
```

### IntersectionObserverEntry
<table>
    <colgroup>
    <col style="width:20%">
    <col style="width:80%">
    </colgroup>
    <thead>
        <tr>
            <td>속성</td>
            <td>설명</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>rootBounds</td>
            <td>root 요소 위치와 크기</td>
        </tr>
        <tr>
            <td>boundingClientRect</td>
            <td>대상 요소 위치와 크기</td>
        </tr>
        <tr>
            <td>intersectionRect</td>
            <td>교차된 영역 정보 (보이는 부분)</td>
        </tr>
        <tr>
            <td>intersectionRatio</td>
            <td>교차하는 영역 비율 (threshold와 같이 0.0 ~ 1.0 값)</td>
        </tr>
        <tr>
            <td>isIntersecting</td>
            <td>대상 요소가 root에 표시되었는지 여부</td>
        </tr>
        <tr>
            <td>target</td>
            <td>대상 DOM 요소</td>
        </tr>
        <tr>
            <td>time</td>
            <td>교차가 발생한 시간</td>
        </tr>
    </tbody>
</table>

<img src="img/callback.png" alt="" width="70%">

> 메인 스레드에 영향을 주지 않으면서 비동기적으로 요소 확인. getBoundingClientRect()를 호출 할 필요 없어 리플로우 현상 방지

<img src="img/observer_inview.png" alt="" width="80%">

### 브라우저 지원
- [IntersectionObserver](https://caniuse.bitsofco.de/embed/index.html?feat=intersectionobserver&amp;periods=future_1,current,past_1,past_2)
- [IntersectionObserver Polyfill](https://github.com/w3c/IntersectionObserver/tree/master/polyfill)
















































