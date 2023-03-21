# React 18 - concurrent render

브라우저는 HTML,CSS를 파싱하고 자바스크립트를 실행하며 랜더트리를 구축하고 그려내는 작업까지 싱글 스레드로서 한번에 하나의 작업만을 수행합니다.

메인 스레드가 자바스크립트 엔진에게 권한을 넘겨주어 자바스크립트 파싱을 하게 된다면 해당 작업이 끝날 때 까지 멈출 수 없으며 리액트 렌더링 과정도 이와 동일하게 진행됩니다.

그러므로 연산이 복잡하거나 오래 걸리는 작업이 진행될 경우 메인 스레드가 블록되어 사용자와의 상호작용에 방해가 될 수 있으며 이는 좋지 않은 사용자 경험을 줄 수 있습니다.<br>
리액트 팀은 이런 문제를 `concurrent render`를 통해 해결하고자 했습니다.

concurrent, 동시성이란 여러 작업을 작은 단위로 나누어 우선순위를 정하고 그에 따라 작업을 번갈아 수행하는 방법입니다.<br>
실제로 작업이 동시에 실행되는것은 아니지만, 빠른 작업 전환을 통해 동시에 진행 되는 것 처럼 보이게 할 수 있습니다.

concurrent render는 사용자와의 상호작용이 있을경우 이를 우선 처리하여 화면이 멈춰있는 경험을 하지 않도록 일정 시간동안 현재 UI와 기능들을 유지하고 다음 렌더링을 동시에 준비하는 방법으로 문제를 해결합니다.

자바스크립트에는 병렬 스레드나 스케줄러가 없지만 react는 18버전에서 fiber 라는 엔진을 개선하여 자체적인 스케줄러를 가지고있으며, 이를 통해 작업의 우선순위를 정하고 우선순위가 높은 작업이 들어오면 먼저 처리하는 기능이 구현되었습니다.

<br>

## useTransition

```javascript
const [isPending, startTransition] = useTransition();
```

useTransition는 UI를 차단하지 않고 상태를 업데이트할 수 있는 React Hook입니다.

- startTransition() : 리액트에 어떤 상태변화를 지연하고 싶은지 지정할 수 있는 함수
- isPending : 트랜지션이 진행중인지 알 수 있다

useTransition을 사용하면 특정 상태 업데이트의 `우선순위가 더 낮다는 것`을 React에 알릴 수 있습니다.<br>
더 우선순위가 높은 업데이트가 있을 경우 우선순위가 낮은 업데이트를 지연시키고 이전 값을 보여줍니다.

<br>

## useDeferredValue

useDeferredValue는 우선순위가 낮은 부분의 리렌더링을 막으면서 하위 컴포넌트나 상태의 업데이트를 지연시킬 수 있습니다.

값의 업데이트 우선순위를 지정하여 우선순위가 높은 작업을 실행하는동안 이전 값을 계속 갖고 있으면서 업데이트를 지연시킵니다.

```javascript
const [inputValue, setInputValue] = useState('');
const deferredValue = useDeferredValue(inputValue);

const boxes = useMemo(() => {
  return (
    ...
  );
}, [deferredValue]);
```

inputValue는 빠르게 변하지만 deferredValue의 값을 바로 변경되지 않습니다.

우선순위가 높은 value 값만 먼저 업데이트하고 우선순위가 낮은 deferredValue 값은 업데이트가 지연됩니다.
