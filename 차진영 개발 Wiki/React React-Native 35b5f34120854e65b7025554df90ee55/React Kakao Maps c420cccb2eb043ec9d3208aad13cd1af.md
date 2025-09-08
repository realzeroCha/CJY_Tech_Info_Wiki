# React Kakao Maps

웹에서 **Kakao Maps**는 공식적으로 **open API**를 지원하며, 플랫폼을 등록하고 키를 발급받아 사용할 수 있다.

### **키 발급**

- [*카카오 개발자사이트*](https://developers.kakao.com/)  접속 ⇒ 개발자 등록 및 앱 생성
- **[플랫폼]** ⇒ **Web**에서 프로젝트 사이트 도메인 등록 (테스트를 위한 **http://localhost:3000**도 가능하다)
- **[앱 키]**에서 **JavaScript 키**를 복사하여 프로젝트에 적용 (**.env**등 외부 파일에 적용하여 **key 숨김** 필수)
    - **네이티브 앱:** AOS or IOS SDK에서 API 호출
    - **REST API 키**: REST API 호출 (카카오 로그인, 푸시 등)
    - **JavaScript 키**: JavaScript SDK에서 API 호출 (주로 **web, node.js** 등에 사용)
    - **Admin 키**: 모든 권한을 가진 키

### 초기 설정

```jsx
<script 
  type="text/javascript"
	src="//dapi.kakao.com/v2/maps/sdk.js?appkey=발급받은 App API 키" 
/>
```

index.html에 발급받은 **App API 키**를 포함하여 **Kakao Maps script 태그**를 등록한다.

태그 위치는 **head**, **body** 상관 없다.

### 지도 생성 코드

```jsx
const mapContainer = document.getElementById("map")
const mapOptions = {
  center: new kakao.maps.LatLng(lat, lng),
  level: 3,
}

const map = new kakao.maps.Map(mapContainer, mapOptions)

<div id={"map"} />
```

1. 지도를 담을 영역의 **DOM reference**를 지정한다.
2. 생성할 지도의 옵션을 지정한다.
    - `center`: 보여줄 위치(**위도**, **경도**)를 지정한다.
    - `level`: 지도의 확대 정도를 지정한다.
3. `Map`**(지도 DOM, 옵션)**을 통해 지도를 생성한다.

- typeScript에서 사용할 때
    
    ```tsx
    declare global {
      interface Window {
        kakao: any;
      }
    }
    
    const map = new Window.kakao.maps.Map(mapContainer, mapOptions);
    ```
    
    typescript에서는 kakao를 읽을 수 없어 `Window` interface에 `kakao`를 추가하여 지정한다.
    
    ⇒ 기존 `kakao.maps` 위에 `Window`를 추가 지정해야한다.
    

### 지도 기능

**기능 추가(**`new kakao.maps`를 통해 선언)

- `Map(Dom reference, Option)`: 지도
- `LatLng(위도, 경도)`: 좌표 값
- `LatLngBounds(sw, ne)`: 동서남북 좌표
- `MapTypeControl()`: 지도타입 컨트롤
- `zoomControl()`: 확대 제어 컨트롤
- `ControlPosition(방향 ⇒ ex: TOP, TOPRIGHT…)`: 컨트롤 위치
- `MapTypeId`: 지도 타입(뒤에 추가할 타입 입력)
    - `TRAFFIC`: 교통정보
    - `ROADVIEW`: 로드뷰
    - `TERRAIN`: 지형정보
    - `USE_DISTRICT`: 지적편집도

**지도 기능 제어(생성한** `Map`**에 선언)**

- `addControl(컨트롤 요소, ControlPosition)`: 컨트롤러 추가
- `addOverlayMapTypeId(MapTypeId)`: 지도타입 추가
- `removeOverlayMapTypeId(MapTypeId)`: 지도타입 삭제
- `relayout()`: resize된 지도의 픽셀과 좌표정보 재설정

**getter(생성한** `Map`**에 선언)**

- `getCenter()`: 현재 좌표를 받아온다.
- `getLevel()`: 현재 지도의 level을 받아온다.
- `getMapTypeId()`: 지도 타입을 받아온다. (지도 or 스카이뷰)
- `getBounds()`: 지도의 현재 영역을 받아온다.

**setter(생성한** `Map`**에 선언)**

- `setCenter(LatLng)`: 지도 중심을 이동한다. (지도 좌표가 즉시 갱신)
- `panTo(LatLng)`: 지도 중심을 부드럽게 이동한다.
- 대부분 set + 제어할 요소로 변경이 가능하다. (ex: `setDraggable`)

**Overlay(`new kakao.maps`를 통해 선언)**

**Marker**

- `Marker({ position: LatLng, ... })`: 마커 생성 ⇒ 생성 후 `생성된 마커.setMap(Map)` 필요
    - `draggable`: 드래그 가능 여부
    - `clickable`: 마커 클릭 여부 (true일 경우 지도 이벤트 발생 X)
    - `text`: 추가하고자 하는 텍스트
- `Size(width, height)`: 마커 이미지 크기
- `Point(x, y)`: 이미지 내의 좌표
- `MarkerImage(imgSrc, imgSize, imgOption)`: 마커 이미지 생성
    - `imgOption: { offset: Point }`
- `infoWindow({ map: Map, position: LatLng, content: HTML or document element, removable: boolean })`: 텍스트가 있는 말풍선
    - `removable`: 말풍선의 x 버튼 여부
    - `infoWindow.open(Map, marker?)`: 인포윈도우 표시(marker가 없다면 지도에 표시)

**도형**

- `Circle({ center: LatLng, radius: 반지름, strokeWeight: 두께, strokeColor: 선의 색, strokeOverlay: 선의 불투명도, strokeStyle: 선의 스타일, fillColor: 채우는 색, fillOverlay: 채우기 불투명도 })`: 원 표시
- `Polyline({ path: LatLng[], strokeWeight: 두께, strokeColor: 선의 색, strokeOverlay: 선의 불투명도, strokeStyle: 선의 스타일 })`: 다중 라인 표시
- `Rectangle({ bounds: LatLngBounds, strokeWeight: 선의 두께, strokeColor: 선의 색, strokeOpacity: 선의 불투명도, strokeStyle: 선의 스타일, fillColor: 채우기 색깔, fillOpacity: 채우기 불투명도 })`: 사각형 표시
- `Polygon({ path: [LatLng[], LatLng[]? => 구멍 좌표], strokeWeight: 두께, strokeColor: 선의 색, strokeOverlay: 선의 불투명도, strokeStyle: 선의 스타일, fillColor: 채우는 색, fillOverlay: 채우기 불투명도 })`: 다각형 표시
- `getPath()`: 도형의 좌표 받아오기

**CustomOverlay**

- `CustomOverlay({ map: Map, content: HTML or document element,  xAnchor: x, yAnchor: y, position: LatLng })`: 커스텀 오버레이

**RoadView**

- `Roadview(Dom reference)`: 로드뷰
- `RoadviewClient()`: 로드뷰 helper 객체
- `RoadviewClient.getNearestPanoId(LatLng, number, function)`: 특정 좌표의 가까운 파노라마 id를 받아온다.
- `roadview.setPanoId(panoId, LatLng)`:  파노라마 id와 좌표를 통해 로드뷰 실행
- `roadview.getProjection()`: 화면 좌표 값을 불러올 **projection** 불러오기
- `projection.viewpointFromCoords(LatLng, altitude)`: 화면 좌표 불러오기

**RoadView Overlay**

- 생성 방법은 기존 **Overlay**와 동일
- `altitude`: 마커의 높이(m)
- `range`: 마커가 보이는 범위(m)

**etc)**

- `staticMap(Dom reference, Option)`: 이미지 지도

### 참고

- [*카카오 개발자사이트*](https://developers.kakao.com/)
- [Kakao Maps API Guide](https://apis.map.kakao.com/web/guide/)