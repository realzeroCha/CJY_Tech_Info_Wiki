# Chrome에서의 오디오 자동재생 에러

Chrome 보안 정책으로 오디오를 재생할 때 에러 출력과 함께 오디오가 실행되지 않는 에러

### **에러 내용**

- play() failed because the user didn't interact with the document first

### **에러 원인**

Chrome에서 자동재생을 허용하는 경우는 다음과 같다. 

<aside>
💡 Chrome 자동재생 정책

- 음소거된 자동재생은 항상 허용됩니다.
- 다음과 같은 경우 소리와 함께 자동재생이 허용됩니다.
    - 사용자가 도메인과 상호작용했습니다 (클릭, 탭 등).
    - 데스크톱에서 사용자의 [미디어 참여 지수](https://developer.chrome.com/blog/autoplay/?hl=ko#media_engagement_index) 기준점이 초과되었습니다. 즉, 사용자가 이전에 사운드가 포함된 동영상을 재생했다는 의미입니다.
    - 사용자가 모바일에서 [홈 화면에 사이트를 추가](https://web.dev/customize-install/?hl=ko)했거나 데스크톱에 [PWA를 설치](https://web.dev/progressive-web-apps/?hl=ko)했습니다.
- 상단 프레임은 iframe에 [자동재생 권한을 위임](https://developer.chrome.com/blog/autoplay?hl=ko#iframe_delegation)하여 사운드가 포함된 자동재생을 허용할 수 있습니다.
</aside>

⇒ 위 조건을 충족하지 않는 상황(페이지 진입 시 오디오가 자동재생 되는 상황 등)에 발생

### **해결 방법**

- 1. MEI(미디어 참여 지수) 충족
    - 미디어 (오디오/동영상) 사용 시간이 7초보다 커야 합니다.
    - 오디오가 있고 음소거가 해제되어 있어야 합니다.
    - 동영상이 있는 탭이 활성 상태입니다.
    - 동영상 크기 (픽셀)는 [200x140](https://chromium.googlesource.com/chromium/src/+/1c63b1b71d28851fc495fdee9a2c724ea148e827/chrome/browser/media/media_engagement_contents_observer.cc#38)보다 커야 합니다.
1. iframe에 오디오를 주어 권한 위임
    - 오디오를 로컬 데이터로 사용하기 어려운 문제 발생
2. 페이지 최초 진입 시 오디오를 바로 재생하지 않고 기간을 두어 재생

### **참고**

- [Chrome Audio Autoplay error 문서](https://developer.chrome.com/blog/autoplay?hl=ko#webaudio)