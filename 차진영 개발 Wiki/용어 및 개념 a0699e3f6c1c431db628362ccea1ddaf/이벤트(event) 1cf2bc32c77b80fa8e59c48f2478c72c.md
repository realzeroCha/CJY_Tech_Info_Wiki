# 이벤트(event)

**이벤트(event)**는 사용자의 **클릭**, **입력**, **마우스 움직임**, **폼 제출** 등의 동작을 감지하고 처리하는 메커니즘

⇒ **버블링(bubbling)**, **캡처링(capturing)**, **위임(delegation)** 등의 방식으로 전파

## 이벤트 전파(Event Propagation)

이벤트가 발생하면 특정 요소에서 **DOM 트리**를 따라 위 또는 아래로 전파

⇒ **캡처링** ⇒ **타겟** ⇒ **버블링**의 3단계로 진행

### **이벤트 전파 단계**

1. **캡처링**
- 이벤트가 **최상위 요소(document)**에서 타겟 요소까지 내려가는 과정
- **이벤트 리스너**를 실행할 수 있음
1. **타겟**
- 이벤트가 **타겟**에서 실행됨
1. **버블링**
- 이벤트가 타겟 요소에서 시작해 상위 요소로 전파되는 과정
- 기본적으로 **이벤트 리스너**가 실행되는 단계

## 버블링 (Bubbling)

이벤트가 **하위 요소에서 발행**한 후, **상위 요소로 전파**되는 현상

```html
<div id="parent">
<button id="child">버튼</button>
</div>
<script>
	document.getElementById("parent").addEventListener("click", function () {
		console.log("부모 요소 클릭");
	});
	document.getElementById("child").addEventListener("click", function () {
		console.log("버튼 클릭");
	});
</script>
```

⇒ “버튼 클릭” ⇒ “부모 요소 클릭” 순으로 실행

**버블링을 방지하는 방법**: `event.stopPropagation()`

## 캡처링 (Capturing)

이벤트가 **최상위 요소**부터 **타겟 요소**까지 내려오는 과정에서 실행되는 방식

- `useCapture` 옵션을 **true**로 설정할 경우 위 단계에서 **이벤트 리스너** 실행
    
    ⇒ **부모의 이벤트**가 먼저 실행되고, 이후 **자식 요소 이벤트** 실행
    

## 이벤트 위임 (Delegation)

**부모 요소**에 **이벤트 리스너**를 설정하여 **하위 요소**의 이벤트를 처리하는 기법

```html
<ul id="list">
	<li>Item 1</li>
	<li>Item 2</li>
	<li>Item 3</li>
</ul>
<script>
	document.getElementById("list").addEventListener("click", function (event) {
		if (event.target.tagName === "LI") {
			console.log(event.target.innerText + " 클릭");
	}
	});
</script>
```

⇒ 클릭한 아이템만 감지하여 이벤트 실행

### 장점

- 새로운 아이템을 추가하더라도 별도의 **이벤트 리스너**를 추가할 필요 없음
- 개별 리스너를 추가하는 것보다 **효율적**이고 **성능 최적화**

## 기타

- `event.stopPropagation()`: **버블링** or **캡처링**을 중단하여 이벤트 전파 방지
- `event.preventDefault()`: **기본 이벤트 동작**(클릭, 입력 등) 방지