# shouldComponentUpdate

**클래스 컴포넌트**에서 리렌더링을 방지하거나 성능을 최적화할 때 사용하는 **생명주기 메서드**

- 기본적으로 **React**는 `state`나 `props`가 변경되면 컴포넌트 리렌더링
    
    ⇒ `shouldComponentUpdate`를 활용하여 **불필요한 리렌더링 방지**
    

```tsx
class MyComponent extends React.Component {
	shouldComponentUpdate(nextProps, nextState) {
		if (this.props.value !== nextProps.value) {
			return true;
		}
		return false;
	}

	render() {
		return <div>{this.props.value}</div>;
	}
}
```

⇒ `props.value` 값을 비교하여 변경되었을 때만 리렌더링 수행

### shouldComponentUpdate 사용 시기

- **렌더링 비용**이 클 때
- 리스트나 테이블 등이 많아 **성능 최적화**가 필요할 때
- **자식 컴포넌트**의 **불필요한 렌더링 방지**가 필요할 때

### shouldComponentUpdate를 사용하면 안되는 경우

- **불변성**을 지키지 않는 경우
    
    ⇒ 객체의 **얕은 비교**를 수행하므로 객체를 **직접 변경**하면 정상적으로 작동하지 않음
    
    `return JSON.stringify(this.props.data) !== JSON.stringify(nextProps.data);`
    
    위와 같이 객체를 **깊은 비교**로 수행해야 정상 동작
    

### shouldComponentUpdate를 대신할 수 있는 방법

1. `PureComponent`
    
    **React**의 `PureComponent`는 `shouldComponentUpdate`를 자동으로 구현하여 얕은 비교 수행
    
    ```tsx
    class MyComponent extends React.PureComponent {
    	render() {
    		return <div>{this.props.value}</div>;
    	}
    }
    ```
    
    ⇒ **객체나 배열**이 깊은 구조일 경우 `PureComponent`가 비효율적일 수 있음
    

1. `React.memo`
    
    **함수형 컴포넌트**에서 `shouldComponentUpdate` 대신 사용
    
    ```tsx
    const MyComponent = React.memo(({ value }) => {
    	return <div>{value}</div>;
    });
    ```
    
    ⇒ `props`가 변경되지 않으면 **리렌더링**을 수행하지 않음