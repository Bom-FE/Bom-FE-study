# Unstated-next를 활용한 상태관리 및 관심사 분리

효율적인 상태관리 코드와 관심사 분리 측면에서 적합한 라이브러리에 대해 조사 해봤습니다.

### 상태관리 라이브러리 후보
- Redux
- Unstated-next  

이중 최종적으론 Unstated-next를 선택했고 왜 선택했는지에 대한 이유를 작성하고자 한다.

## Unstated-next란 ?
> React Context API 위에서 동작하는 상태관리 라이브러리이며 도큐먼트에선 이렇게 소개하고 있다.  
> - **React Hooks** use them for all your state management.
> - **~200 bytes** min+gz.  
> - **Familiar API** just use React as intended.  
> - **Minimal API** it takes 5 minutes to learn.  
> - **Written in TypeScript** and will make it easier for you to type your React code.  
> 
> But, the most important question: Is this better than Redux? Well...     
>   
> 
> - **It's smaller.** It's 40x smaller.  
> - **It's faster.** Componentize the problem of performance.  
> - **It's easier to learn.** You already will have to know React Hooks & Context, just use them, they rock.  
> - **It's easier to integrate.** Integrate one component at a time, and easily integrate with every React library.  
> - **It's easier to test.** Testing reducers is a waste of your time, make it easier to test your React components.  
> - **It's easier to typecheck.** Designed to make most of your types inferable.  
> - **It's minimal.** It's just React.  
> 
> 직역 하면 **리액트 상태관리 라이브러리에 대해 다시는 생각하지 않게되는 200Bytes 수준의 경량화된 용량을 가지고 쉽고 빠르다** 라고 소개하고 있다.

### 🚀 사용방법
```javascript
import React from "react";
import { createContainer } from "unstated-next";

function useCounter(initialState = 0) {
  const [count, setCount] = useState(initialState);

  const increment = () => {
    setCount(count + 1);
  };

  return { count, increment };
}

const CounterContainer = createContainer(useCounter);

function MyComponent() {
  const counter = CounterContainer.useContainer();

  return (
    <div>
      <h1>Count: {counter.count}</h1>
      <button onClick={counter.increment}>Increment</button>
    </div>
  );
}

function App() {
  return (
    <CounterContainer.Provider>
      <MyComponent />
    </CounterContainer.Provider>
  );
}

export default App;
```
위 코드에서는 useCounter 함수를 만들어서 count의 상태와 increment 함수를 반환하고 있다.  
이후 createContainer 함수를 활용하여 CounterContainer 컨테이너를 한다.  
MyComponent 컴포넌트 에서는 CounterContainer.useContainer를 사용하여 count와 increment 함수를 가져와서 사용하는 형태이다.  


### 왜 Unstated-next 였는가
Redux의 단점
1. 보일러플레이트 코드가 많음
2. 단 한개의 스토어에서 관리되는 Redux 특성 상 상태를 일관성 있게 유지할순 있지만 애플리케이션 규모가 커지면 스토어 내부 상태관리 또한 러닝커브가 증가함
3. 불변성 유지에 대한 부담

유지보수, DX를 고려했을때 **Redux**를 채택하지 않은 이유이다.  

반면 unstated-next는 이러한 단점들을 충분히 보완할수 있다.
1. hook과 유사한 방식으로 사용
2. 내부적으로 immer를 사용하여 상태를 업데이트 하기에 불변성이 보장됨
3. 상태관련 로직을 컨테이너 형식으로 관리하므로 복잡한 상태관리 패턴도 쉽게 관리 가능
4. useState, useEffect 기반으로 만들어졌기에 상태변화 예측, 디버깅 편리
5. 도메인별로 필요한 상태만을 관리하기가 용이


반면 단점도 있다.  
unstated-next는 리덕스에 비해서 DevTools와 같은 개발자도구가 따러 없으며 비교적 신생 라이브러리 이기 때문에 생태계가 비교적 크지 않아 자료가 많지 않다는 단점도 있다.
그리고 Context API 기반으로 작동하여 컴포넌트 상태가 업데이트될 때마다 Context Provider를 찾아서 상태를 전달해야 하기에 Store가 변경될때만 컴포넌트가 업데이트 되는 Redux에 비해서 경우에 따라 성능이 뒤쳐질수도 있다.

하지만 이런 단점들 보다도 도메인별 컨테이너 형식으로 상태관리 로직을 집중적으로 작성 가능, custom hook과 유사한 형식의 사용방식 덕분에 러닝커브가 매우 적음, 페이지별 상태관리가 가능한 점 등
DX가 Redux에 비해서 뛰어났기에 unstated-next를 선택했다.