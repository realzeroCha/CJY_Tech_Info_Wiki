# redux-saga

**제너레이터 함수**를 사용하여 정해진 로직에 따라 다른 **action**을 **dispatch** 시키는 규칙을 작성해주는 **middleware** 

⇒ **비동기 작업**을 처리할 때 사용

- **※ 제너레이터란?**
    - `function*`를 사용하여 만든 **제너레이터 함수**를 호출했을 때 반환되는 객체
    - `next`를 통해 순차적으로 요소를 탐색
    - `yield`를 통해 반환하여 코드의 흐름을 정지 혹은 재개
    - `value`와 `done`을 가진 새로운 객체 반환

### **설치**

- `npm i redux-saga`

### 사용방법

- **saga 미들웨어 추가**

```jsx
import { createStore, applyMiddleware } from 'redux';
import createSagaMiddleware from 'redux-saga';

const sagaMiddleware = createSagaMiddleware();
const store = createStore(rootReducer, applyMiddleware(sagaMiddleware));

sagaMiddleware.run(rootSaga);
```

- **이펙트 함수 타입**
    - `takeEvery(action, saga)`: 모든 액션을 감지하고 처리 ⇒ 비동기 호출 중복 가능
    - `takeLatest(action, saga)`: 가장 최신 액션만 처리 ⇒ 이전 요청 취소
    - `call(fn, …arg)`: 비동기 함수 호출 ⇒ **블로킹** 발생
    - `fork(fn, …arg)`: 함수를 비동기적으로 실행하도록 호출 ⇒ **블로킹** 발생 X
    - `put(action)`: **action**을 **dispatch**
    - `delay(ms)`: 행동 지연

### 참고

- [Redux-Saga](https://redux-saga.js.org/)