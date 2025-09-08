# FlatList

`VirtualizedList`의 특징을 가진 `ScrollView`

- `VirtualizedList`의 특징
    - 현재 화면에 렌더링한 요소만 남기고 남은 부분은 여백으로 대체한다.
        
        ⇒ 메모리 사용량과 렌더링 성능을 향상시킨다.
        
    - 가시 항목에서 멀어지면 **low-pri로** 점진적 렌더링하고 그 외에는 **hi-pri**로 렌더링되어 여백을 최소화한다.
    - `key`를 통해 항목의 prop을 찾고 React 키에 사용합니다. 사용자 지정 `keyExtractor`prop을 제공할 수 있습니다.

⇒ `VirtualizedList`의 `props`를 상속받는다.

### 특징

- Header 및 Footer 생성이 가능하다.
- Item을 구분하는 구분선을 추가할 수 있다.
- 스크롤하여 데이터 refresh와 로딩이 가능하다.
- 다중 열 스크롤이 가능하다.

### 기능

```css
<FlatList
	data={itemList}
	renderItem={({item}) => <Item title={item.title} />}
	keyExtractor={item => [item.id](http://item.id/)}
/>
```

필수적으로 `data`와 데이터를 렌더할 `renderItem`을 추가해야한다.

- 리스트 컴포넌트 관련
    - `horizontal`: 수평 리스트 여부
    - `numColumns`: 리스트 줄바꿈이 일어날 아이템 수 (flexWrap: wrap과 유사한 기능)
    - `initialScrollIndex`: 최초 렌더링 시 시작하는 위치 (아이템의 **index**로 판단)
    - `inverted`: 리스트 방향 뒤집기 여부

- 아이템 컴포넌트 관련
    - `ListHeaderComponent`: 리스트 최상단에 대한 컴포넌트
    - `ListFooterComponent`: 리스트 최하단에 대한 컴포넌트
    - `ListEmptyComponent`: 리스트의 데이터가 없을 때 보여줄 컴포넌트
    - `ItemSeparatorComponent`: 최상단과 최하단을 제외한 아이템 컴포넌트

- 렌더링 관련 (렌더링 속도 개선에 영향을 준다.)
    - `extraData`: 리렌더링을 판단하는 데이터 (해당 값을 변경한 경우 리렌더링 발생)
    - `initialNumToRender`: 초기 배치에서 렌더링할 아이템 수
    - `maxToRenderPerBatch`: 배치 당 렌더링되는 아이템 수
    - `removeClippedSubviews`: 뷰포트 외부의 subView에 대한 분리 여부
        
        ⇒ **true**일 경우 화면에서 벗어난 아이템을 **unmount** ⇒ 메모리 절약에 유용
        
    - `updateCellsBatchingPeriod`: 아이템의 배치 렌더링 간 지연시간
    - `windowSize`: 렌더링할 영역 외부의 아이템 수 (default: 21)
    - `getItemLayout`: 아이템의 비동기 layout을 계산
        
        ⇒ 아이템의 구성요소가 동일할 경우 렌더링 개선에 효과적
        

- 이벤트 관련
    - `onRefresh`: 리스트를 상단으로 당겼을 때 refresh 이벤트 호출
    - `onViewableItemsChanged`: 보여지는 아이템이 변할 때, 이벤트 호출
    - `onEndReached`: 리스트 하단에 도달했을 때, 이벤트 호출
        - `onEndReachedThreshold`: **onEndReached** 이벤트의 가시영역 (default: 1)
            
            ⇒ 1에 가까울수록 하단 (0.5: 화면의 50%)