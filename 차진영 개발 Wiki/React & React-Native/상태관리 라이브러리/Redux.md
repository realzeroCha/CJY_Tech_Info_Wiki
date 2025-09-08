# Redux

전역 상태관리 라이브러리의 일종으로 클라이언트 상태를 관리하는데 유용하다.

### **특징**

- **Flux 패턴**에 맞추어 설계되어 있다.
  [Flux 패턴](..%2F..%2F%EC%9A%A9%EC%96%B4%2520%EB%B0%8F%2520%EA%B0%9C%EB%85%90%2FFlux%2520%ED%8C%A8%ED%84%B4.md)
- **React Context** 기반의 라이브러리로 최상단에 **Provider**를 위치하고 있다.

### **설치**

- 안정 버전 설치: `npm i redux`
- Redux 코어와 필수적인 패키지 함께 설치: `npm i @reduxjs/toolkit`
- 보조 패키지

<aside>
💡 React 바인딩 도구: `npm i react-redux`

devTools: `npm i --save-dev redux-devtools`

</aside>

- redux 템플릿이 포함된 새 프로젝트 생성: `npx create-react-app my-app --template redux`
  ⇒ Typescript 프로젝트를 생성할 경우 `redux-typescript`로 변경

⇒ 주로 `npm i @reduxjs/toolkit react-redux`를 입력하여 설치

### **초기 설정**

- **store** 생성(**store.js**)

```jsx
import { configureStore } from "@reduxjs/toolkit";

export default configureStore({
  reducer: {},
});
```

- `Provider` 생성 및 store 등록 (**App.js**)

```jsx
import store from "./store";
import { Provider } from "react-redux";

function App() {
  return (
    <Provider store={store}>
      <Todos />
    </Provider>
  );
}
```

위의 store.js에서 선언한 store를 **Provider**의 props로 등록하여 store를 사용한다.

### **사용방법**

- store에 등록할 상태 **Slice** 추가

  ```jsx
  import { createSlice } from "@reduxjs/toolkit";

  export const counterSlice = createSlice({
    name: "counter",
    initialState: {
      value: 0,
    },
    reducers: {
      increment: (state) => {
        state.value += 1;
      },
      decrement: (state) => {
        state.value -= 1;
      },
      incrementByAmount: (state, action) => {
        state.value += action.payload;
      },
    },
  });

  export const { increment, decrement, incrementByAmount } =
    counterSlice.actions;

  export default counterSlice.reducer;
  ```

  `createSlice`를 통해 redux의 새로운 상태를 추가한다.
  `name`과 default 값인 `initialState`를 입력하고 `reducers`를 통해 상태를 변경할 **action**을 추가한다.

- 선언한 **Slice**를 **store.js**의 **reducer**에 추가한다.

  ```jsx
  import { configureStore } from "@reduxjs/toolkit";
  import counterReducer from "../counterSlice";

  export default configureStore({
    reducer: {
      counter: counterReducer,
    },
  });
  ```

- **Slice** 상태 **사용** 및 **action** 수행

  ```jsx
  import { useSelector, useDispatch } from "react-redux";
  import { decrement, increment } from "./counterSlice";

  const count = useSelector((state) => state.counter.value);
  const dispatch = useDispatch();

  const increaseCount = () => {
    dispatch(increment());
  };
  ```

  - `useSelector`: **store**에 등록된 **state**를 읽는 hook
    ⇒ 데이터를 즉시 가공하여 state의 원하는 값만 추출할 수 있다.

    ```jsx
    const allUsers = useSelector((state) => state.users);

    const selectedUser = useSelector((state, userId) =>
      state.users.find((user) => user.id === userId)
    );
    ```

  - `useDispatch`: **state** 변경을 수행할 slice의 **action**을 전송하는 hook
    ⇒ 위의 `dispatch(increment())`와 같이 slice의 action을 import하여 사용한다.

### middleware

액션을 **dispatch**하는 순간과 **reducer**에 도달하는 순간 사이에 처리하는 함수

⇒ 기존의 **미들웨어**는 OS와 SW 중간에서 수행하는 SW를 의미

```jsx
// 기본 템플릿
const middleware = (store) => (next) => (action) => {
  // 처리 로직
};

// 일반 함수 형식
const middleware = function middleware(store) {
  return function (next) {
    return function (action) {
      // 처리 로직
    };
  };
};
```

- **Redux**는 동기적으로 작동하여 과정을 딜레이 시키거나 비동기 작업을 추가할 수 없다.

  ⇒ **middleware**를 통해 비동기 작업을 처리

- **middleware 매개변수**

  - `store`: redux **store** 인스턴스
    ⇒ `dispatch`, `getState`, `subcribe` 내장 함수 존재
  - `next`: **action**을 다음 **middleware**에 전달하는 함수
    ⇒ `next(action)`을 호출하면 다음 **middleware** 혹은 **reducer**에 **action**을 넘김

- **middleware** 내부에서 `store.dispatch`를 사용하면 첫번째 **middleware**부터 다시 처리

**대표적인 middleware**

[redux-logger](Redux/redux-logger.md)

[redux-thunk](Redux/redux-thunk.md)

[redux-saga](Redux/redux-saga.md)

⇒ 간단한 비동기 함수 요청은 **redux-thunk**, 복잡한 비동기 플로우에는 **redux-saga**가 유용

### 참고

- [리덕스(Redux)](https://ko.redux.js.org/)
- [Redux Toolkit](https://redux-toolkit.js.org/)
