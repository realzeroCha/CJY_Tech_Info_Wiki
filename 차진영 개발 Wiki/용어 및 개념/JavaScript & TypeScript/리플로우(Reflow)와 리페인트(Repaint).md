# 리플로우(Reflow)와 리페인트(Repaint)

### 리플로우(Reflow)

- **레이아웃 변경**이 발생하는 경우
    - **DOM 요소 크기 변경**
        
        ⇒ ex) width, height, padding, margin, border
        
    - **요소 위치 변경**
        
        ⇒ ex) top, left, position
        
    - **화면 크기** **변경**
    - **새로운 DOM 요소 추가 / 제거**
- 브라우저가 모든 요소의 **위치**와 **크기**를 다시 계산
- 비용이 많이 들어 **성능 저하의 원인**

### 리페인트(Repaint)

- **레이아웃을 다시 계산할 필요가 없는 변경**이 발생하는 경우
    - ex) background-color, color, border-color, box-shadow
- **리플로우보다 가벼움**

⇒ **리플로우**가 발생하면 **리페인트**도 발생 ⇒ **리페인트**는 **리플로우** 없이 발생 가능

### 리플로우 & 리페인트 최소화 방법

- 스타일 변경을 한번에 적용 (**Batching**)

```jsx
const box = document.getElementById("wrap");

wrap.style.width = "300px";
wrap.style.height = "200px";
wrap.style.margin = "10px";

box.classList.add("style");
```

위 방법처럼 하나의 **클래스**를 추가하여 한꺼번에 변경하면 **리플로우**가 1회만 발생하여 효율적

- `position: absolute` or `fixed` 사용

```css
.wrap {
	position: fixed;
	top: 0;
	left: 0;
	width: 100px;
	height: 100px;
}
```

`position: static`에 비해 리플로우 영향을 덜 받아 성능 최적화

- `visibility: hidden` 사용

```css
.wrap {
	visibility: hidden;
}
```

`display: none`과 비슷하게 **요소를 숨기는 역할**

⇒ `display: none`은 **리플로우** 발생, `visibility: hidden`은 **리페인트**만 발생한다는 차이가 있음

`display: none`은 아예 화면에서 제거된 것 처럼 동작 ⇒ 요소가 차지하던 공간도 사라짐

ex) **모달 창** 숨기기, **선택되지 않은 탭**의 콘텐츠 숨기기

`visibility: hidden`은 보이지 않지만 공간은 그대로 유지 ⇒ **이벤트는 발생하지 않음**

ex) **툴팁, 드롭다운 메뉴** 등을 숨길 때, 애니메이션 적용

- **애니메이션 최적화**

```css
.wrap {
	transform: translate(-50%, -50%);
	transition: transform 1s ease;
}

.wrap {
	opacity: 0;
}
```

`transform`과 `opacity` 애니메이션은 **리플로우** 없이 적용 가능

⇒ `top`, `left` 등을 변경하는 애니메이션은 **리플로우** 발생