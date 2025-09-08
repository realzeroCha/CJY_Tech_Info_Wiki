# 시멘틱 태그(Semantic Tags)

**시멘틱 태그**는 콘텐츠의 의미를 명확하게 정의하는 **HTML 태그**를 의미

### 특징

- **웹 콘텐츠**의 특정 부분을 **정의**
    
    ⇒ 검색 엔진에 페이지 구조와 의미를 명확하게 전달하여 **검색 엔진 최적화(SEO)** 향상
    
- 스크린 리더 등의 보조 기술의 **접근성 향상**
- **페이지의 구조**가 명확해져 이해 및 관리에 도움을 주어 **유지보수 용이**
- **코드 가독성 향상**

### 주 사용 태그

`<header>`

- 페이지나 섹션의 **머리말**을 정의
    
    ex) 로고, 네비게이션 메뉴, 검색창
    

```html
<header>
	<h1>Title</h1>
</header>
```

`<footer>`

- 페이지나 섹션의 **바닥글**을 정의
    
    ex) 저작권 정보, 연락처, 사이트맵
    

```html
<footer>
  <p>2025 My Website. All rights reserved.</p>
</footer>
```

`<main>`

- 문서에서 **중심적인 콘텐츠**를 정의
    
    ⇒ 한 문서에 하나만 사용 가능
    

```html
<main>
	<h1>MainPage Title</h1>
	<p>This is main content</p>
</main>
```

`<aside>`

- 본문과 **간접적**으로 관련된 콘텐츠를 정의
    
    ex) 사이드바, 관련 링크, 인용
    

```html
<aside>
	<h2>Sidebar</h2>
	<ul>
    <li><a href="/intro">Introduces</a></li>
    <li><a href="/list">Item List</a></li>
  </ul>
</aside>
```

`<article>`

- 독립적으로 **자체적인 의미**를 가지는 콘텐츠를 정의
    
    ex) 블로그 포스트, 뉴스 기사
    

```html
<article>
	<h2>라볶이 만들기</h2>
	<p>떡과 라면을 익히고 고추장 한 큰술...</p>
</article>
```

`<section>`

- 주제별로 **구분되는 부분** 정의

```html
<section>
  <h3>1st Section</h3>
  <p>section content</p>
</section>
```

`<nav>`

- 웹사이트 내의 **네비게이션 링크** 정의

```html
<nav>
  <ul>
    <li><a href="/home">Home</a></li>
    <li><a href="/intro">Introduces</a></li>
    <li><a href="/list">Item List</a></li>
  </ul>
</nav>
```

`<detail>`, `<summary>`

- **세부 정보를 숨기거나 표시**하는 콘텐츠 정의
    
    ⇒ `<summary>`는 세부 정보를 펼칠 때 나타나는 제목
    
        `<details>`는 숨겨진 내용을 포함한 태그
    

```html
<details>
	<summary>Read more...</summary>
	<p>hidden contents</p>
</details>
```

`<figure>`, `<figcaption>`

- **이미지**의 콘텐츠를 설명하는 **캡션** 정의
    
    ⇒ `<figure>`는 이미지 등의 콘텐츠를 포함한 **전체 내용**
    
        `<figcaption>`는 시각 콘텐츠에 대한 **설명**
    

```html
<figure>
	<img src="image.jpg" alt="img name">
	<figcaption>Image description</figcaption>
</figure>
```