# realm

**realm**은 모바일 환경에서 쿼리를 **CRUD**할 수 있도록 도와주는 **로컬 DB**이다.

- 데이터는 모두 로컬에 저장되어 앱 저장정보를 삭제할 경우 **realm** 데이터도 모두 삭제된다.
- realm은 DB간의 **join**이 불가능하고 `embedded Schema`를 통해 schema 내부에 schema를 넣어 **join**과 유사한 역할을 수행한다.

### **설치**

- `npm i realm`

### **기능**

**Schema 정의**

```jsx
const userSchema = {
  name: "user",
  properties: {
    name: { type: "string" },
    age: { type: "int" },
    hobby: { type: "list", objectType: "hobby", optional: true },
    is_check: { type: "bool", default: false }
  },
  primaryKey: 'name',
};
```

- `name`: 객체의 key
- `embedded`: 부모 객체에 포함 여부
    
    ⇒ **true**일 경우 `properties`의 `objectType`에 정의하여 부모 객체에서 사용 가능
    
- `properties`: 객체의 데이터형
    - `type`: 객체 value의 데이터형
    - `default`: 객체 value의 기본 값 (선택)
    - `objectType`: 객체 type이 list(array)일 때, list의 객체 데이터형
    - `optional`: 객체 value의 선택사항 여부 (javaScript의 ?와 동일)
- `primaryKey`: 지정할 고유키 (`embedded: true`일 경우 지정 불가)

**Realm** **초기화**

- `schemaVersion`을 업데이트 해야 한다.
- `migration`: realm을 불러올 때, 기존 realm과 비교하여 데이터를 초기화한다.
    
    **주요 용도**
    
    - `schemaVersion`을 업데이트 했을 때, **object type** 변경으로 충돌이 발생할 부분을 업데이트.
    - **realm**이 변경되었을 때, 다른 값이 변경되는 경우
    
    ```
    export const realm = {
       schema,
       schemaVersion: 1,
       onMigration: (oldRealm: Realm, newRealm: Realm) => {
    	   // update할 내용
       }
    }
    ```
    

**Realm Open**

```jsx
// 1번
const oRealm = new Realm(realm);

// 2번
const realm = useRef();
realm.current = await Realm.open({schema:[userSchema]});
```

1. 위에서 구조 변경할 때 선언한 **realm**을 `new` 객체로 불러오는 방법
2. **Realm**을 `useRef`에 참조하여 열고자 하는 **Schema**를 open하는 방법

**Schema CRUD (1번 방법으로 open한 기준)**

**Create**

```jsx
realm.write(() => {
	realm.create(SCHEMA_NAME, data);
}
```

- 삽입할 data에 없는 **property**일 경우 **null**로 저장

**Read**

```jsx
const schema = realm.objects(SCHEMA_NAME);
```

- `SCHEMA_NAME`은 사용자가 만든 schema의 name을 추가한다.

**Update**

```jsx
realm.write(() => {
	let filteredData = schema.filtered(`age == $0`, 20);
	if(filteredData.length > 0) {
		filteredData[0].is_check = true;
	}
}
```

- 먼저 업데이트할 schema를 read하여 데이터를 가져온다.
- `filtered`를 통해 **schema**에서 업데이트를 수행할 데이터만 분류한다.
- 분류한 데이터 변수 값을 변경하면 **realm** 데이터가 수정된다.

**Delete**

```jsx
realm.write(() => {
	let filteredData = schema.filtered(`age == $0`, 20);
	schema.delete(filteredData);
}
```

- `update`와 같은 방법으로 `filtered`를 통해 분류한 데이터를 **schema**에서 삭제할 수 있다.