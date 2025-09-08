# styled-components

**JavaScript**를 사용한 **css 스타일 작성 라이브러리**이다.

### **특징**

- JavaScript내에서 직접 스타일을 작성할 수 있는 **CSS-in-JS** 기반으로 상태에 따라 스타일 변경이 쉽다.
- **CSS**가 컴포넌트 범위로 한정되어 **전역 네임스페이스** 오염을 방지한다.
- **스타일**이 적용된 컴포넌트를 생성하여 독립적인 스타일이 가능하다.

### 설치

`npm i styled-components`

### **사용방법**

```jsx
import styled from "styled-components";

const Wrapper = styled.div`
	padding: 10px;
	background-color: '#fff';
`;

return (
	<Wrapper>
		<h1>Main Page</h1>
	</Wrapper>
);
```

`Wrapper`라는 **div**에 직접 **padding**과 **background-color** 스타일을 주어 독자적인 **div**를 생성한다.

```jsx
const ColorTitle = styled.button<{ $inputColor?: string }>`
	padding: 4px;
	font-size: 20px;
  color: ${(props) => props.$inputColor || '#000'};
`;

return (
	<div>
		<ColorTitle $inputColor="red">
			Hello World!
		</ColorTitle>
	</div>
);
```

`props`를 받아 동적인 스타일을 구현할 수 있다.

```jsx
const Button = styled.button.attrs(props => ({
	disabled: props.isDisabled
}))`
	background: ${props => (props.isDisabled ? '#000' : '#555')};
`;
```

`attrs()`는 스타일을 `inline` 속성으로 넣어 컴포넌트의 기본 속성을 설정하거나 스타일을 동적으로 추가할 때 사용한다.

주로 props 값이 매순간 변경되는 경우 스타일이 계속 변경되는 것을 방지하기 위해 사용한다.

### 참고

[styled-component 문서](https://styled-components.com/docs)