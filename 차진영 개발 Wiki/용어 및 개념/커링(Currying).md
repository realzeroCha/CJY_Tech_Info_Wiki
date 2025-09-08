# 커링(Currying)

**커링(Currying)**은 **다수의 인자를 받는 함수**를 **하나의 인자만 받는 함수**들의 연속으로 변환하는 기법

### 커링의 장점

- 부분적으로 **인자**를 미리 설정하여 재사용 가능
- 여러 개의 함수를 체인처럼 연결하여 호출하는 **함수 체이닝**이 가능
- 각 함수가 한가지 책임만 갖게되어 **가독성 향상**

```jsx
function curry(fn) {
	return function curried(...args) {
		if (args.length >= fn.length) {
			return fn(...args);
		} else {
			return function(...newArgs) {
				return curried(...args, ...newArgs);
			}
		}
	}
}

function sum(a, b, c) {
	return a + b + c;
}

const curriedSum = curry(sum);

console.log(curriedSum(1)(2)(3));
```