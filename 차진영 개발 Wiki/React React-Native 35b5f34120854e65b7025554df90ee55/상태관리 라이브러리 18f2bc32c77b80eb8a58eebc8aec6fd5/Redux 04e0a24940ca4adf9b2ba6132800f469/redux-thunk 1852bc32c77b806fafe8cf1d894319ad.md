# redux-thunk

**action** 생성자에서 함수를 반환하여 함수 형태의 **action**을 **dispatch**해주는 **middleware** 

⇒ **비동기 작업**을 처리할 때 사용

### **설치**

- `npm i redux-thunk`

### 사용방법

- 함수를 **dispatch** 할 때, 해당 함수는 `dispatch`와 `getState`를 파라미터로 가질 수 있음

- **redux-toolkit**을 사용하는 방법

```jsx
import { createAsyncThunk, createSlice } from "@reduxjs/toolkit";

export const getAPI = createAsyncThunk("GET_API", async () => {
  const response = await axios.get("http://localhost:3000/api");
  return response.data;
});
```

- **redux-toolkit**을 사용하지 않는 방법

```jsx
import { applyMiddleware, createStore } from 'redux';
import { thunk } from 'redux-thunk';

const store = createStore(rootReducer, applyMiddleware(thunk));
```

### 참고

- [Redux-Thunk](https://github.com/reduxjs/redux-thunk)