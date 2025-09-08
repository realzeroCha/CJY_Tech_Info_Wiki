# Tailwind CSS

**Utility-First** 접근 방식을 지향하는 **CSS 프레임워크**이다.

- **Utility-First**란?
    
    재사용 가능한 작은 **CSS 클래스를** 활용하여 스타일을 적용하는 방법
    
    - 사전에 미리 정의된 클래스를 통해 **신속한 스타일 작성**이 가능하다.
    - 스타일의 **일관성**이 유지된다.
    - **불필요한 CSS**를 줄일 수 있다.
- 다수의 스타일을 사용할 경우 코드가 난잡할 수 있다.

### 설치

`npm install -D tailwindcss`

**tailwind.config.js** 파일 생성: `npx tailwindcss init`

### **사용방법**

```jsx
<button class="flex items-center px-4 py-3 text-white bg-blue-500 hover:bg-blue-400">
	Button
</button>
```

```css
.flex {
	display: flex;
}
.items-center {
	align-items: center;
}

.px-4 {
	padding-left: 1rem;
	padding-right: 1rem;
}

.py-3 {
	padding-top: 0.75rem;
	padding-bottom: 0.75rem;
}

.bg-blue-500 {
	background-color: #4299e1;
}

.text-white {
  --tw-text-opacity: 1;
  color: rgb(255 255 255 / var(--tw-text-opacity));
}

.hover\:bg-blue-400:hover {
  --tw-bg-opacity: 1;
  background-color: rgb(96 165 250 / var(--tw-bg-opacity));
}
```

위 **TailWind CSS**로 작성한 버튼의 스타일을 `CSS`로 표현하면 아래와 같다.

위처럼 `CSS` 스타일을 신속하고 일관성 있게 작성할 수 있다.

### 기타

v3 부터 **IE** 지원을 하지 않는다고 한다.

### 참고

[TailWind CSS 문서](https://tailwindcss.com/)