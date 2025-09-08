# react에서 마크다운 편집하기

**react-md-editor**는 react에서 마크다운을 보거나 편집하기 위해 사용하는 라이브러리이다.

### 설치

- `npm install @uiw/react-md-editor`

### 사용방법

```tsx
import React from 'react';
import MDEditor from '@uiw/react-md-editor';

  const [mdText, setMdText] = useState("")
  
  return (
	  <MDEditor value={mdText} onChange={setMdText} />
  );
```

- **MDEditor**는 기본적으로 **편집기**와 **뷰어**를 모두 제공한다.

**Props**

- `preview?: "edit"|"live"|"preview”`: live: 편집기, 뷰어 / edit: 편집기만 / preview: 뷰어만
- `visibleDragbar?: boolean`: 우측 하단에 사이즈를 조절할 수 있는 드래그바 생성
- `fullscreen?: boolean`: 편집기를 풀스크린으로 보여주기
- `hideToolbar?: boolean`: 편집 툴바 가시 여부
- `toolbarBottom?: boolean`: 편집 툴바 하단 배치 여부
- `data-color-mode?: “light”|”dark”`: 다크모드 설정
- `previewOptions?: object`: **Style**이나 **className** 등을 정의할 수 있는 옵션 제공
- `components: object`: 컴포넌트 UI를 커스텀 (ex: 툴바, textarea 등)
    - `textarea`: 편집 textarea
    - `toolbar: {(command, disabled, executeCommand) ⇒ {}}`: 툴바
        
        **command.keyCommand**를 통해 툴바 도구 커스텀 가능
        
    - `preview: (source: String)`: 마크다운 뷰어

### 참고

- [@uiw/react-md-editor](https://github.com/uiwjs/react-md-editor)