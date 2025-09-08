# Zustand

전역 상태관리 라이브러리의 일종으로 클라이언트 상태를 관리하는데 유용하다.

### **특징**

- **Flux 패턴**에 맞추어 설계되어 있다.
  [Flux 패턴](../../용어%20및%20개념/Flux%20패턴.md)
- React에 직접적으로 의존하지 않아 자주 바뀌는 상태를 직접 제어하거나 불필요한 리렌더링이 일어나지 않게 제어할 수 있다.
- Redux에 비해 코드가 적고 비교적 간단하여 상대적으로 **러닝커브**가 낮다

### **설치**

- `npm i zustand`

### **사용법**

- **Store 생성**
  ```jsx
  import { create } from "zustand";

  const useStore = create((set) => ({
    value: 0,
    increaseValue: () => set((state) => ({ value: state.value + 1 })),
    resetValue: () => set({ value: 0 }),
  }));
  ```
  **hook** 형태의 **store**를 생성하여 내부에 데이터, 함수, 객체 등을 넣는다.
  ⇒ 단, 상태는 불변적으로 업데이트 되어야한다.
- **Store 사용**

  ```jsx
  function Counter() {
    const bears = useStore((state) => state.value);
    return <h1>value is {value}</h1>;
  }

  function AddButton() {
    const increaseValue = useStore((state) => state.increaseValue);
    return <button onClick={increaseValue}>Add Value</button>;
  }
  ```

  **store**를 불러와 사용한다.

  ⇒ **Redux**와 달리 별도의 **Provider**가 필요하지 않는다.

- **리렌더링 제어**
  ```jsx
  import { useShallow } from "zustand/react/shallow";

  // 필요한 state만 구독하는 방법
  const value = useStore((state) => state.value);

  // shallow를 사용하여 값의 변경이 발생했을 때만 리렌더링 수행하는 방법
  const { name, value } = useStore(
    useShallow((state) => ({ name: state.name, value: state.value }))
  );
  ```
  리렌더링이 필요한 **Store**의 특정 **state**만 구독하여 변경을 감지하는 방법과 `useShallow`를 통해 **state**를 구독하여 값의 변경이 발생했을 때만 **리렌더링**을 수행하는 방법이 있다.
  ```jsx
  const subscribeValue = useStore.subscribe(
    (state) => state.value,
    (value, prevValue) => console.log(value, prevValue)
  );
  ```
  **리렌더링** 없이 **state** 변경을 감지 후 특정 요청을 수행하고 싶다면, `subscribe`를 사용한다.

### **기타**

- **Redux와 차이점**
  - **Hook**을 상태 소비의 주요 수단으로 사용한다.
  - 별도의 **Provider**를 필요로 하지 않는다.
  - **리렌더링** 없이 상태 변경을 수행할 수 있다.
- **Jotai**와 개발자가 같다. (**Jotai**는 **Atomic 패턴**, **Justand**는 **Flux 패턴** 차이)

### **참고**

- [zustand 문서](https://zustand-demo.pmnd.rs/)
- [zustand Github](https://github.com/pmndrs/zustand)
