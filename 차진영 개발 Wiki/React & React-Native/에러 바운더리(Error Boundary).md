# 에러 바운더리(Error Boundary)

**React 컴포넌트**에서 발생하는 **JavaScript** **오류**를 감지하고, UI 전체가 깨지는 것을 방지하는 기능

⇒ **자식 컴포넌트**에서 발생한 오류로 인한 **크래시**를 방지하지 위해 **대체 UI**를 보여주는 역할 수행

**⇒ 클래스 컴포넌트**에서만 구현 가능 (`componentDidCatch` 메서드 사용)

### 에러 바운더리 구현방법

```tsx
import React, { Component } from "react";

class ErrorBoundary extends Component {
	state = { hasError: false };

	static getDerivedStateFromError(error) {
		return { hasError: true };
	}

	componentDidCatch(error, info) {
		console.error("Error caught by ErrorBoundary:", error, info);
	}

	render() {
		if (this.state.hasError) {
			return <h2>⚠️ 오류가 발생했습니다.</h2>;
		}
		return this.props.children;
	}
}

export default ErrorBoundary;
```

`componentDidCatch`와 `getDerivedStateFromError` 생명주기 메서드를 사용하여 구현

```tsx
import React from "react";
import ErrorBoundary from "./ErrorBoundary";
import ErrorComponent from "./ErrorComponent";

function ErrorBoundaryTester() {
	return (
		<ErrorBoundary>
			<ErrorComponent/>
		</ErrorBoundary>
	);
}
```

위 에러 바운더리 클래스를 적용하여 `ErrorComponent`에 오류가 발생해도 앱이 **크래시**되는 것을 방지

### 에러 바운더리의 한계

- **자식 컴포넌트**에서 발생한 오류만 감지 ⇒ **자신의 내부**에서 발생한 오류는 감지 불가
- **비동기 혹은 이벤트 핸들러**에서 발생한 오류 감지 불가 ⇒ `try-catch` 로 처리
- **함수형 컴포넌트**에서 사용불가
    
    ⇒ **클래스형 컴포넌트**의 에러 바운더리를 감싸서 사용 가능
    
    ⇒ `react-error-boundary`와 같은 외부 라이브러리 활용 가능