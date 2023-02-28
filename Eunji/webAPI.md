# Web API

## Intersection Observer

> - 페이지가 스크롤 되는 도중에 발생하는 이미지나 다른 컨텐츠의 지연 로딩.
> - 스크롤 시에, 더 많은 컨텐츠가 로드 및 렌더링되어 사용자가 페이지를 이동하지 않아도 되게 하는 infinite-scroll 을 구현.
> - 광고 수익을 계산하기 위한 용도로 광고의 가시성 보고.
> - 사용자에게 결과가 표시되는 여부에 따라 작업이나 애니메이션을 수행할 지 여부를 결정.

브라우저에서 특정 요소가 다른 요소와 교차되는지 여부를 확인하는 데 사용합니다.

페이지가 로드될 때 이벤트를 발생시키고 요소가 화면에 표시되거나 사라질 때 이벤트를 발생시키므로 페이지 로딩 시간을 단축하고 사용자 경험을 개선하는데 도움이 됩니다.

브라우저에서는 이벤트 루프를 사용하여 스크롤 이벤트를 처리하기 때문에 페이지가 스크롤 될 때마다 스크롤 이벤트가 이벤트 큐에 쌓이게 된다면 debounce나 throttle을 사용하더라도 이벤트 큐에 부하가 생길 수 있습니다.

Intersection Observer는 교차 상태를 비동기적으로 감지한 뒤 이벤트 루프를 통해 콜백 함수를 호출하기 때문에 브라우저에서 효율적으로 동작합니다.

### 사용방법

```javascript
const observerCallback = useCallback(
  (entries: IntersectionObserverEntry[]) => {
    const [entry] = entries;

    if (data) {
      if (entry.isIntersecting) {
        onFetchNext();
      }
    }
  },
  [data, onFetchNext]
);

let options = {
  root: document.querySelector('#scrollArea'),
  rootMargin: '0px',
  threshold: 1.0,
};

let observer = new IntersectionObserver(observerCallback, options);
```

- `root` : 대상 객체의 가시성을 확인할 때 사용되는 뷰포트 요소
- `rootMargin` : root 가 가진 여백
- `threshold` : 가시성 퍼센티지를 나타내는 단일 숫자 혹은 숫자 배열<br>
  - 0.0 - 교차영역에 진입한 시점에 observer 실행
  - 1.0 - 타겟 엘리먼트 전체가 교차영역에 진입했을 때 observer 실행

<br>

observer는 IntersectionObserverEntry 객체의 배열을 반환합니다.
엔트리를 조회해 isIntersecting 값이 true일 경우 관찰하고 있는 대상이 root와 교차한 상태이므로 callback함수를 실행하도록 로직을 작성해줍니다.

intersection observer를 활용해 무한 스크롤을 구현했으며 이때 rootMargin과 threshold값을 활용하여 스크롤이 끝에 도달하기 전에 미리 다음 페이지를 fetch할 수 있어 사용자가 페이지를 탐색 할 때 스크롤이 멈추는 경험을 하지 않도록 할 수 있었습니다.

또한 react-native의 flatList처럼 모바일 웹에 최적화 되어있는 공통 리스트 컴포넌트로 모듈화 시키며 초기값을 받아오거나 다음 데이터를 fetching하고 있을때는 skeleton ui를 보여주고, 리스트 중간에 광고 컨텐츠 또는 다른 유형의 데이터를 보여줄 수 있는 작업등을 진행해 개발자들의 업무의 효율성을 높일 수 있었습니다.

<br>

---

<br>

## Scroll

<br>

- scrollHeight : 해당 요소의 콘텐츠 전체 높이<br>
  내부 스크롤이 발생할 경우에도 보이지 않는 전체 콘텐츠의 높이를 반환하며 padding, border를 포함한 높이

- clientHeight : 해당 요소의 내부 높이<br>
  padding을 포함한 내부 콘텐츠의 높이를 반환

- offsetHeight : 해당 요소의 외부 높이<br>
  paddig, border, scrollbar를 포함한 내부 컨텐츠 전체 높이를 반환

- offsetTop : 해당 요소의 위쪽 여백과 부모 요소의 위쪽 여백 사이의 거리

스크롤한 위치와 브라우저에 표시된 높이를 더한 값을 엘리먼트 또는 페이지 전체 높이값과 비교해 스크롤의 끝에 도달했는지 판단할 수 있습니다.

또한 스크롤 메서드를 활용해 원하는 지점의 높이를 계산하여 이동하는 로직을 구현할 수 있습니다.

### 스크롤 메서드

- scroll(), scrollTo() - 엘리먼트의 절대적 위치를 기준으로 스크롤 발생<br>
  x, y좌표를 인자로 넘길 수 있습니다.<br>
- scrollBy() - 엘리먼트의 상대적 위치를 기준으로 스크롤 발생<br>
  x, y좌표를 인자로 넘길 수 있습니다.<br>
- scrollIntoView() - 특정 요소가 보이도록 스크롤<br>
  { behavior, block, inline }값을 인자로 넘길 수 있습니다.
- scrollTop - 값을 변경하여 스크롤의 위치를 변경할 수 있습니다.

<br>
