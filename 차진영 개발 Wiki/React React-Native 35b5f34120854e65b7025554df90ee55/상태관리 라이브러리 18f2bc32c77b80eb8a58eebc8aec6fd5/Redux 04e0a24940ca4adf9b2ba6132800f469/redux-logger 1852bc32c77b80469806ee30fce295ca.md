# redux-logger

**action** 정보를 콘솔에 출력해주는 **middleware**

### **설치**

- `npm i redux-logger`

### 사용방법

- **redux-toolkit**을 사용하는 방법

```jsx
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from '../modules/counterSlice';
import logger from 'redux-logger';

export default configureStore({
  reducer: {
    counter: counterReducer,
  },
  middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(logger),
});
```

- **redux-toolkit**을 사용하지 않는 방법

```jsx
import { applyMiddleware, createStore } from 'redux';
import { createLogger } from 'redux-logger';

const logger = createLogger();
const store = createStore(rootReducer, applyMiddleware(logger));
```

### 참고

- [LogRocket/redux-logger](https://github.com/LogRocket/redux-logger)