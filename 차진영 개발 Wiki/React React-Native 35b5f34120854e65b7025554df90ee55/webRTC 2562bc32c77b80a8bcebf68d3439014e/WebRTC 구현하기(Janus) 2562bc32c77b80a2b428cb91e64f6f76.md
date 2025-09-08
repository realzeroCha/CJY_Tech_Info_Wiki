# WebRTC 구현하기(Janus)

**Janus**는 SFU 성격의 **WebRTC Gateway**로 **플러그인** 기반 구조

**Plugin 기반 구조**: `videoroom`, `echotest`, `audiobridge` 등의 플러그인에 `attach`하여 사용

⇒ **세션(Session)** 단위로 동작 관리

### Publisher와 Subcriber

- **Publisher(송신자) ⇒ 음성 송출**
    
    local SDP offer 생성 → Janus 전달 → answer 받음 → ICE candidate 교환 → media 송출
    
- **Subcriber(수신자) ⇒ 음성 수신**
    
    subscriber attach → SDP offer/answer → ICE candidate 교환 → media 수신
    

### 구현 순서

1. **WebRTC 연결**
    
    `Long Polling` or `WebSocket`을 통해 시그널링 메시지를 주고받음
    
    ```tsx
    const ws = new WebSocket("websocket_server_url");
    ```
    
2. **세션 생성**
    
    응답으로 `session_id` 획득
    
    ```tsx
    {
    	janus: "create",
    	transaction: "random_uuid"
    }
    ```
    
3. **Plugin Handle Attach**
    
    응답으로 `handle_id` 획득
    
    ```tsx
    {
    	janus: "attach",
    	plugin: "janus.plugin.videoroom",
    	session_id: “session_id”,
    	transaction: "random_uuid"
    }
    ```
    

4. **Room 생성 / 참여**

이전 단계에서 획득한 `session_id`와 `handle_id`를 사용하여 방에 접근

**방 생성**

```tsx
{
	janus: "message",
	body: {
		request: "create",
		room: "room_id",
		publishers: "maximum_publishers"
	},
	handle_id: "handle_id",
	session_id: "session_id",
	transaction: "random_uuid"
}
```

**방 참여(publisher)**

```tsx
{
	janus: "message",
	body: {
		request: "join",
		room: "room_id",
		ptype: "publisher",
		display: "user1"
	},
	handle_id: "handle_id",
	session_id: "session_id",
	transaction: "random_uuid"
}
```

**방 참여(subscriber)**

```tsx
{
	janus: "message",
	body: {
		request: "join",
		room: "room_id",
		ptype: "subscriber",
		feed: "feed_id"
	},
	handle_id: "handle_id",
	session_id: "session_id",
	transaction: "random_uuid"
}
```

**Publisher 기능**

**PeerConnection 생성 후 SDP Offer 생성 및 전송, Answer 처리**

`RTCPeerConnection`: 두 피어 간의 직접적인 연결을 관리하는 역할 수행

`SDP Offer`: 상대방에게 자신의 **미디어 기능** 및 **네트워크 정보**를 제안하는 역할 수행

- **미디어 정보**: 코덱, 해상도, 프레임 등
- **네트워크 정보:** 네트워크 후보 목록, 공인 IP(STUN), 중계 주소(TURN)
- **세션 속성**: 미디어 스트림 전송방향 (sendonly, recvonly, sendrecv), 암호화 방식

**Janus**는 **answer**을 `jsep`로 반환

- `getUserMedia()`를 통해 **track**을 **PeerConnection**에 추가하여 마이크 연결

```tsx
const pc = new RTCPeerConnection();

navigator.mediaDevices.getUserMedia({ audio: true, video: false })
	.then(stream => {
	stream.getTracks().forEach(track => pc.addTrack(track, stream));
});

const offer = await pc.createOffer();
await pc.setLocalDescription(offer);

ws.send(JSON.stringify({
  janus: "message",
  body: { request: "publish", audio: true, video: false },
  jsep: offer,
  handle_id,
  session_id,
  transaction: "random_uuid"
}));

if (msg.jsep) {
  await pc.setRemoteDescription(new RTCSessionDescription(msg.jsep));
}
```

**Subscriber 기능**

**SDP Offer 생성 및 Answer 교환**

**Subscriber**도 `PeerConnection`을 생성하지만, **로컬 스트림** 없음

- `ontrack` 이벤트에서 받은 remote stream을 `audio` 태그로 자동 재생

```tsx
const pcSub = new RTCPeerConnection();

pcSub.ontrack = (event) => {
	const audio = document.createElement("audio");
	audio.srcObject = event.streams[0];
	audio.autoplay = true;
};

const subOffer = await pcSub.createOffer();
await pcSub.setLocalDescription(subOffer);

ws.send(JSON.stringify({
  janus: "message",
  body: { request: "start", room: "room_id" },
  jsep: subOffer,
  handle_id: subHandleId,
  session_id,
  transaction: "random_uuid"
}));

if (msg.jsep) {
  await pcSub.setRemoteDescription(new RTCSessionDescription(msg.jsep));
}
```

1. **ICE Candidate 교환**
    
    Janus가 보내주는 candidate도 `pc.addIceCandidate()`로 추가
    
    `onicecandidate`로 **ICE candidate**를 **Janus**로 전달 ⇒ **Publisher**, **Subscriber** 모두 해당
    
    ```tsx
    pc.onicecandidate = (event) => {
    	if (event.candidate) {
    		ws.send(JSON.stringify({
    			janus: "trickle",
    			candidate: event.candidate,
    			"handle_id",
    			"session_id",
    			transaction: "random_uuid"
    		}));
    	}
    };
    ```