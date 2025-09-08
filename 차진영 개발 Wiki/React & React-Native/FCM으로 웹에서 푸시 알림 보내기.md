# FCM으로 웹에서 푸시 알림 보내기

### **React 등 웹 환경에서 푸시 알림을 받는 방법**

- Foreground 푸시는 **onMessage**, Background 푸시는 **Service Worker(onBackgroundMessage)** 사용

### **firebase 설정**

- [firebase 프로젝트 생성](https://firebase.google.com/products/cloud-messaging?hl=ko)
- 생성한 프로젝트의 Config 값과 서버 vapid-key 복사

### **React 프로젝트**

1. **firebase 설치**: `npm i firebase`

1. **firebase 설정 추가**
- 2-1. firebase config 등록

```jsx
const config = "firebase에서 생성한 Config"
firebase.initializeApp(config)
```

- 2-2. permission 요청 및 Token 발급

```jsx
const messaging = firebase.messaging();

Notification.requestPermission()
.then((permission) {
	if(permission === 'granted') {
		return messaging.getToken(messaging(), {
				vapidKey: "firebase에서 복사한 서버 vapid-key"
		}) // 토큰 발급(이후 .then 등을 통해 토큰 발급 후 후속 로직을 적용해도 좋음)
	}
})
```

⇒ permission은 granted(허가) denied(실패) 두 가지가 존재

⇒ vapidKey는 서버 키이므로 외부에 숨겨두는 것이 좋다. (.env 등)

⇒ `onTokenRefresh()`을 통해 토큰 갱신 가능

1. **Service Worker 설정**
- 프로젝트의 index.html 등이 위치한 public 폴더에 **firebase-messaging-sw.js** 파일 생성 및 작성

```jsx
importScripts("https://www.gstatic.com/firebasejs/9.0.0/firebase-app-compat.js")
importScripts("https://www.gstatic.com/firebasejs/9.0.0/firebase-messaging-compat.js")

const config = "firebase에서 생성한 Config"
firebase.initializeApp(config);

const messaging = firebase.messaging();
messaging.onBackgroundMessage((payload) => {
	// console.log()로 payload를 확인하는 등의 로직
});
```

- etc) Background Message 커스터마이징

```jsx
messaging.setBackgroundMessageHandler(function(payload) {
	const title  =  payload.notification.title
	const options  = {
		body: payload.notification.body,
	};
	return self.registration.showNotification(title, options)
})
```

⇒ **firebase-messaging-sw.js**에 추가

⇒ Background Message의 커스터마이징은 FCM을 발송하는 firebase에서도 설정이 가능하다.

### **참고**

- [firebase 문서](https://firebase.google.com/docs/cloud-messaging/js/receive?hl=ko)