# html5 doctype

`<!DOCTYPE>`은 **HTML 문서**의 **문서 유형**을 선언하는 태그

```html
<!DOCTYPE html>
```

⇒ **브라우저**에게 이 문서가 **HTML5 표준**을 따르는 문서임을 알리는 역할

### <!DOCTYPE>의 역할

`<!DOCTYPE>`을 올바르게 선언하면 **브라우저**가 **표준 모드**로 동작

⇒ 생략하거나 잘못 선언하면, 일부 브라우저에서 **쿼크 모드**로 동작할 수 있음

- **표준 모드**: 최신 웹 표준을 준수하여 HTML/CSS를 해석하여 정확한 렌더링
- **쿼크 모드**: 오래된 브라우저와의 호환성을 위해 비표준 방식으로 렌더링

이전 HTML 버전에 비해 간결한 선언이 가능

⇒ 아래 코드는 HTML 4.01 버전의 doctype 선언

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"[http://www.w3.org/TR/html4/loose.dtd](http://www.w3.org/TR/html4/loose.dtd)">
```