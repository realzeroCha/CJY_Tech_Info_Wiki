# React 드래그 앤 드롭 / 번호 입력을 통한 리스트 순서 변경하기

**드래그 앤 드롭**은 주로 웹보다 모바일 앱 개발에 많이 쓰이는 기능이다.

하지만 요즘 **webView**를 통한 모바일 앱이 많아지는 추세이기 때문에 알아두면 좋을 것 같았다.

“`react-dnd`” 라이브러리를 사용하여 구현하였다.

### 설치

- `npm i react-dnd react-dnd-html5-backend`
- **모바일 웹**: `npm i react-dnd-touch-backend`

### **코드**

- **리스트**

```jsx
import { DndProvider } from 'react-dnd';
import { HTML5Backend } from 'react-dnd-html5-backend';
// 모바일 웹: import { TouchBackend } from 'react-dnd-touch-backend';

const [itemList, setItemList] = useState([]) // 배열 형태의 데이터

const moveItem = (fromIndex: number, toIndex: number) => {
	if (toIndex < 1) toIndex = 0;
  else if (toIndex > itemList.length - 1) toIndex = itemList.length;
  
  const newItems = [...itemList];
  const [movedItem] = newItems.splice(fromIndex, 1);
  newItems.splice(toIndex, 0, movedItem);
  setItemList(newItems);
};

<DndProvider backend={HTML5Backend}> {/* 모바일 웹: TouchBackend */}
	<div>
		{itemList.map((item, index) => {
			return <ListItem key={index} item={item} index={index} moveItem={moveItem} />;
		})}
	</div>
</DndProvider>
```

리스트 컴포넌트의 경우, 외부에 `DndProvider`를 감싸고 `backend`로 `HTML5Backend`를 지정해주어야 한다.

`moveItem` 함수는 **listItem**을 **드래그 앤 드롭**했을 때, 배열을 재배치하고 **index**가 기존 배열을 벗어나 에러가 발생하지 않도록 방어 로직을 추가했다.

- **리스트 아이템**

```jsx
import { DndProvider, DropTargetMonitor, useDrag, useDrop } from 'react-dnd';

const ListItem = ({ item, index, moveItem }) => {
  const [, drag] = useDrag({
    type: 'ITEM',
    item: { item, index },
  });

  const [, drop] = useDrop({
    accept: 'ITEM',
    hover: (item: { index: number }, monitor: DropTargetMonitor) => {
      if (item.index !== index) {
        moveItem(item.index, index);
        item.index = index;
      }
    },
  });

  return (
    <div key={index}>
	    <button ref={(node) => drag(drop(node))}>
		    {item}
	    </button>
      {/* 추가적인 스타일 */}    
    </div>
  );
};
```

**드래그 앤 드롭**을 허용할 요소에 `ref={(node) => drag(drop(node))}`를 추가한다.

⇒ 위 방식은 **드래그 앤 드롭**을 같은 요소에서 동시에 수행하기 위함이고, 드래그와 드롭의 역할을 분리하여 수행하고 싶다면 각각 `ref={drag}` || `ref={drop}`으로도 충분하다.

`useDrag`의 선언한 **type**을 통해 `useDrop`의 허용 요소를 구분할 수 있다.

⇒ **type**에 따라 드래그 앤 드롭 아이템의 기능을 분리하거나 복수의 아이템을 사용 시 유용하다.

드래그 앤 드롭 과정에서 `useDrag`, `useDrop`에 추가적인 옵션 기능을 추가할 수 있다.

**공통**

- `collect`: 백엔드에서 수집된 드래그 앤 드롭 이벤트에 대한 반환(monitor, props 반환)

**useDrag**

- `isDragging`: 현재 드래그 중인지에 대한 여부
- `end`: 드래그 종료 여부
- `canDrag`: 드래그 가능 여부

**useDrop**

- `hover`: 드롭 대상의 마우스 hover 여부
- `drop`: 드롭 여부
- `canDrop`: 드롭 가능 여부

위에서는 사용하지 않았지만 `DropTargetMonitor`를 사용하여 **드래그 앤 드롭 요소의 상태**를 받을 수 있다.

- `canDrag`: 드래그 가능 여부 (`useDrag`에서 설정 가능)
- `canDrop`: 드롭 가능 여부 (`useDrop`에서 설정 가능)
- `isDragging`: 현재 드래그 중인지에 대한 여부
- `didDrop`: 현재 드래그 요소가 드롭 요소에 위치한지에 대한 여부
- `getDropResult`: 드롭 결과 객체 반환
- `getInitialClientOffset`: 드래그가 시작된 좌표 혹은 초기 좌표 반환
- `getInitialSourceClientOffset`: 드래그 요소의 초기 좌표 반환
- `getDifferenceFromInitialOffset`: 드래그 시작 지점과 현재 마우스 좌표 사이의 거리 반환
- `getItem`: 드래그한 아이템의 데이터 반환

- **번호 입력으로 순서 변경 기능 추가**

```jsx
const ListItem = ({ item, index, moveItem }) => {
	const [inputValue, set_inputValue] = useState<number>(index + 1);

  const [, drag] = useDrag({
    type: 'ITEM',
    item: { item, index },
  });

  const [, drop] = useDrop({
    accept: 'ITEM',
    hover: (item: { index: number }, monitor: DropTargetMonitor) => {
      if (item.index !== index) {
        moveItem(item.index, index);
        item.index = index;
      }
    },
  });

  return (
    <div key={index}>
	    <input 
		    ref={(node) => drag(drop(node))}
		    value={inputValue}
        onChange={(e) => {
	        set_inputValue(Number(e.target.value));
        }}
        onBlur={() => {
	        moveItem(index, inputValue - 1);
        }}
        onKeyDown={(e) => {
	        if (e.key === 'Enter') {
		        moveItem(index, inputValue - 1);
	        }
        }}
		  />
      {/* 추가적인 스타일 */}    
    </div>
  );
};
```

기존 **드래그 앤 드롭**을 수행하는 `button`을 `input`으로 바꾸었다.

`input`에 순서를 입력할 수 있는 **state**인 `inputValue`를 추가하여 변경된 값으로 기능을 수행한다.

⇒ `onChange` 자체에 `moveItem`을 수행하면 값이 변경될 때마다 위치가 변경되면 과하게 움직이기 때문에 `onBlur` || `onKeyDown` 등으로 **input**을 마쳤을 때, 순서를 재배치하는 것이 좋다.

### **기타**

**react-draggable**, **react-beautiful-dnd** 등의 유사한 기능을 수행하는 라이브러리도 구현이 가능할 것 같다.