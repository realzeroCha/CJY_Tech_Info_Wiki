# Jotai

전역 상태관리 라이브러리의 일종으로 클라이언트 상태를 관리하는데 유용하다.

### **특징**

- **Atomic 패턴**에 맞추어 설계되어 있다.
  [Atomic 패턴](../../용어%20및%20개념/Atomic%20패턴.md)
- **React 트리** 안에 상태를 저장하고 관리한다는 특징이 있어 **Context**, **useState** 등과 사용 방법이 비슷하다. ⇒ **러닝커브**가 낮다.

### **설치**

- `npm i jotai`

### **사용법**

- **Atom 생성**
  ```jsx
  import { atom } from "jotai";

  const countAtom = atom(0);
  ```
  **atom**을 생성하여 값을 저장하거나 읽고 쓰기가 가능하다.
  **atom**은 객체와 배열을 포함한 다양한 type을 선언할 수 있다.
- **Derived atoms**
  ```jsx
  import { atom } from "jotai";

  const progressAtom = atom((get) => {
    const anime = get(animeAtom);
    return anime.filter((item) => item.watched).length / anime.length;
  });
  ```
  **Derived atoms(파생원자)**는 다른 원자를 읽어 가공한 값을 반환할 수 있다. (**MobX**의 **computed**와 유사)
- **useAtom**
  ```jsx
  const [anime, setAnime] = useAtom(animeAtom);

  const anime = useAtomValue(animeAtom);
  const setAnime = useSetAtom(animeAtom);
  ```
  **useAtom**을 사용하여 기존의 **useState**와 같은 형태로 **atom**을 읽고 쓰는 결합된 hook을 사용할 수 있다.
  해당 **atom**을 사용하는 컴포넌트가 모두 **언마운트**되면 값은 가비지 콜렉터에 수집된다.
  ⇒ **useAtom**을 분리하면 `useAtomValue`가 **get** 역할, `useSetAtom`이 **set** 역할을 수행
  ⇒ `useAtomValue`와 `useSetAtom`은 읽고 쓰는 기능만 가져와 리렌더링을 하지 않는다는 장점이 있다.
- **atomWithStorage**
  ```jsx
  const darkModeAtom = atomWithStorage("darkMode", false);

  const [darkMode, setDarkMode] = useAtom(darkModeAtom);
  ```
  캐시에 저장되는 **atom**을 생성한다. ⇒ **localStorage, sessionStorage, AsyncStorage**에 값을 저장
  ⇒ **AsyncStorage(**React-native**)**를 사용한다면 비동기 값이 오기 때문에 **Promise** 객체를 해결해야한다.
  **atomWithStorage**는 위 storage와 같이 `(key, initialValue)`의 필수값과 옵션 기능을 사용한다.
  - `getItem`: 값을 읽거나 `initialValue`로 돌아간다.
  - `setItem`: 값을 storage에 저장한다. (value에 `RESET`을 넣으면 initialValue로 변경된다.)
  - `removeItem`: 값을 storage에서 삭제한다.
  - `subscribe`: 외부 저장소 업데이트를 구독한다. ⇒ **callback** 수행 가능
  - `getOnInit`: 초기화 시 저장소에서 값을 가져올 것인지에 대한 여부 설정 (default: **false**)

### **기타**

- **Redux**와 같이 최상단에 **Store**를 배치하여 **Provider**와 함께 사용할 수도 있지만 **Atomic 패턴**에서 벗어나 비추천 (**Redux** or **Justand** 등의 **Flux 패턴** 라이브러리를 사용하는 것이 유용하다)
- **Recoil** 역시 Jotai와 같은 **Atomic 패턴**에 맞추어 설계된 상태관리 라이브러리이다.
  ⇒ **Recoil**은 비교적 최신 라이브러리이기 때문에 Jotai에 비해 정보가 부족하고 기능 보조 라이브러리가 적다. (~~Recoil 담당자가 퇴사해서 추후 업데이트 가능성이 많이 낮아졌다.)~~

### **참고**

- [Jotai 문서](https://jotai.org/)
