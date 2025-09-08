# const arrow / function의 차이

|  | **const arrow** | **function** |
| --- | --- | --- |
| **문법** | `const name = (params) ⇒ { … }` | `function name (params) { … }` |
| `this` | 정의된 context의 `this` | 호출된 객체를 참조하는 자신만의 `this` |
| `arguments` | 없음 | `arguments` 객체 가짐 |
| **생성자 함수** | 사용 불가 | 사용 가능 (`new` 사용) |
| **호이스팅** | 불가능 | 가능 |
| **사용 용도** | 짧고 간결한 콜백 함수 | 메서드 정의, 복잡한 함수 |

`arguments`: 배열 형태로 나타낸 함수의 **parameter**

- `map`, `forEach` 등의 배열 메서드 사용 불가
- ex) `function Name (a, b, c) { … }` 의 `arguments = [a, b, c]`

**호이스팅**: 변수 혹은 함수 선언을 코드의 **최상단**으로 올리는 것과 같이 동작하는 특성

- 변수 혹은 함수를 정의하기 전에 호출하는 것이 가능 ⇒ 오류 발생 가능성