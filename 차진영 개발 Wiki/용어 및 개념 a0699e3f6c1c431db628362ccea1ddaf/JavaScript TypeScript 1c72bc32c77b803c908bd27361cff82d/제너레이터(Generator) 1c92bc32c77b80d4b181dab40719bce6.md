# 제너레이터(Generator)

**제너레이터(Generator)**는 실행을 중간에 멈췄다가 시작할 수 있는 함수

### 특징

- 일반 함수처럼 한 번에 실행되지 않고 **필요할 때마다 실행** (**Lazy Execution**)
- **일시 중지** 및 **재개**가 가능하여 **대량 데이터 로딩 제어**가 가능
- `async`/`await` 없이 `yield`를 통해 **비동기 흐름 제어**가 가능

### 기본 문법

```jsx
function* getFruits() {
	yield "apple";
	return "grape";
	yield "banana";
}

const gen = getFruits();

console.log(gen.next()); // apple
console.log(gen.next()); // grape
console.log(gen.next()); // undefined
```

- `function*`를 사용해 선언
- `yield`로 실행을 멈추고 값을 반환 ⇒ **비동기 작업 가능**
- `.next()` 메서드로 실행 재개
- `return`을 만나면 제너레이터가 **종료** ⇒ `return` 이후의 `yield`는 실행되지 않음

### 사용 예시

- **비동기 처리**

```jsx
function* fetchData() {
	yield new Promise((resolve) => setTimeout(() => resolve("success"), 1000));
}

const generatedFetch = fetchData();

generatedFetch.next().value.then((data) => console.log(data));
```

⇒ `async`/`await` 없이 **비동기 작업 처리**

- **무한 반복**

```jsx
function* infiniteCounter() {
	let i = 0;
	while (true) {
		yield i++;
	}
}

const counter = infiniteCounter();

console.log(counter.next().value); // 0
console.log(counter.next().value); // 1
console.log(counter.next().value); // 2
```

⇒ `yield`를 통해 필요할 때마다 실행하는 **무한루프** 생성

- **이터러블 생성**

```jsx
const iterableObject = {
*[[Symbol.iterator](%EC%A0%9C%EB%84%88%EB%A0%88%EC%9D%B4%ED%84%B0(Generator)%201c92bc32c77b80d4b181dab40719bce6.md)]() {
		yield "1st";
		yield "2nd";
		yield "3rd";
	},
};

for (const value of iterableObject) {
	console.log(value);
}
```

⇒ `Symbol.iterator` 객체 생성 가능