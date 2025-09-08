# emotion

**JavaScript**를 사용한 **css 스타일 작성 라이브러리**이다.

### **특징**

- JavaScript내에서 직접 스타일을 작성할 수 있는 **CSS-in-JS** 기반으로 상태에 따라 스타일 변경이 쉽다.
    
    [**CSS-in-JS**](../%EC%9A%A9%EC%96%B4%20%EB%B0%8F%20%EA%B0%9C%EB%85%90%20a0699e3f6c1c431db628362ccea1ddaf/JavaScript%20TypeScript%201c72bc32c77b803c908bd27361cff82d/CSS-in-JS%20e8455c5af30e42119214a5bcc56833eb.md) 
    
- `styled`, `css`를 모두 사용 가능하다.

### **설치**

- **프레임워크** 종류와 상관 없이 css 사용
    
    `npm i @emotion/css`
    
- **React**에서 사용
    
    `npm i @emotion/react`
    
- **styled-Component**와 유사한 기능을 수행하고 싶을 때 사용
    
    `npm i @emoiton/styled`
    
- **권장 플러그인**
    
    `npm i @emotion/babel-plugin`
    

⇒ **React**에서 주로 필요 패키지를 모두 설치해주는 `@emotion/react package`를 통해 설치

### **사용방법 (@emotion/react 기준)**

```jsx
import { css, jsx } from '@emotion/react'

// string
const color = '#000';

<div
css={css`
	padding: 20px;
	border-radius: 4px;
	background-color: `${color}`;	 
`}>
	test
</div>
```

```jsx
import { css, jsx } from '@emotion/react'

// object
const boxStyle = css({
	padding: 20,
	borderRadius: 4,
	backgroundColor: '#000'
});

<div
css={boxCss}>
	test
</div>
```

`css` 함수에 **string** 혹은 **object** type으로 스타일하고자 하는 css를 제공한다.

```jsx
export const boxLine = css({
	borderWidth: 1;
	borderColor: 'red';
});
```

css에 제공될 데이터는 변수화 하거나 외부에서 가져올 수 있다.

```jsx
<div
css={[boxCss, boxLine]}>
	test
</div>
```

css에 제공될 데이터는 중첩해서 사용할 수 있다.

### 참고

- [emotion 문서](https://emotion.sh/docs/introduction)