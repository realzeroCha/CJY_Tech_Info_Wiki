# Tanstack Query v5(react-query)

**서버 상태**를 관리하기 위한 상태 관리 라이브러리

- **※ 서버 상태란?**
    - 귀하가 통제하거나 소유할 수 없는 위치에 원격으로 지속됩니다.
    - 가져오기 및 업데이트를 위한 비동기 API가 필요합니다.
    - 공유 소유권을 암시하며 본인도 모르게 다른 사람이 변경할 수 있습니다.
    - 주의하지 않으면 애플리케이션이 "구식"이 될 수 있습니다.

클라이언트 관리를 주로 사용한다면 redux, mobX, jotai 등의 라이브러리가 더 적합하다.

### **기능**

- 캐싱
- 동일한 데이터에 대한 여러 요청을 단일 요청으로 중복 제거
- 백그라운드에서 오래된 데이터 업데이트
- 데이터가 오래된 시기 알기
- 최대한 빠르게 데이터 업데이트 반영
- 페이지 매김 및 지연 로딩 데이터와 같은 성능 최적화
- 서버 상태의 메모리 및 가비지 수집 관리
- 구조적 공유를 통해 쿼리 결과 메모

### **설치**

- `npm i @tanstack/react-query`
- 부가 설치
    
    <aside>
    💡 eslint 플러그인(권장): `npm i -D @tanstack/eslint-plugin-query`
    
    devTools: `npm i @tanstack/react-query-devtools`
    
    </aside>
    
    ⇒ devTools를 사용하면 우측 하단의 아이콘을 통해 데이터를 직관적으로 관리할 수 있다.
    
    ![Tanstack_query_devTools.png](Tanstack%20Query%20v5(react-query)%20c0eb774247384cb398b0e572bb0edb57/Tanstack_query_devTools.png)
    

### **초기 설정**

- queryClient 생성

```jsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'

const queryClient = new QueryClient();

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <Todos />
    </QueryClientProvider>
  )
}
```

### **주요 기능**

- **useQuery**
    
    ```jsx
    const info = useQuery({ queryKey: ['todos'], queryFn: fetchTodoList })
    ```
    
    query 사용 중 status 상태를 확인하는 요소
    
    - `isPending`: pending(데이터가 아직 없음) ⇒ 기존의 isLoading과 같음
    - `isError`: error(오류 발생)
    - `isSuccess`: success(queryFn이 성공적으로 수행되어 데이터가 있음)
    
    status 상태 확인 후 실행하거나 반환되는 요소
    
    - `data`: isSuccess가 true가 되어 반환되는 데이터
    - `error`: isError가 true가 되어 발생하는 에러
    - `isFetching`: 비동기 함수가 처리되었는지 여부 (queryFn이 실행중이거나 status가 pending일 때)
    
    **queryKey**는 기본적으로 배열 구조로 이루어진다.
    
    ```jsx
    useQuery(['todo', 5, { preview: true }], ...);
    ```
    
    - setQueryData 등으로 쿼리에 접근하기 위해서는 배열 안에 위치하는 **key**가 전부 일치해야한다.
        
        ⇒ 배열 안에는 쿼리 key말고도 **의존성 변수**, **index** 등이 포함될 수 있으며, 하나라도 일치하지 않거나 **배열 순서만 다르더라도 다른 쿼리**로 인식한다. ex) [’todo’]와 [’todo’, todoId]는 다르다.
        
    - 쿼리의 **value**는 객체로 구성되어 있으며, **value의 순서가 달라져도 같은 쿼리**로 인식한다.
        
        ⇒ [’todo’, {status: ‘success’}]와 [’todo’, {status: ‘error’}]와 같이 **value가 다르면 다른 쿼리**로 인식한다.
        
    
    **queryFn**를 통해 return되는 값을 저장한다.
    
    ```jsx
    	 const { isPending, isError, data, error } 
     = useQuery({ queryKey: ['todos'], queryFn: fetchTodoList })
    ```
    
    **queryOptions**를 통해 쿼리에 옵션을 부여할 수 있다.
    
    ```jsx
    	 const queryData = useQuery({
    						  queryKey: ['todo'],
    							queryFn: fetchTodoList,
    							staleTime: 5 * 1000, // queryOptions
    							gcTime: 100 * 1000, // queryOptions
    						}) 
    ```
    
    - `staleTime`: 데이터가 fresh에서 stale 상태로 변하는 시간 (Infinity: 무한대)
    - `gcTime`: 데이터가 inactive 상태일 때 캐싱되어 있는 시간
    - `enabled`: 초기 데이터를 자동으로 fetch 받는 여부
    - `initialData`: 데이터를 필요로 하기 전에 미리 캐시에 초기 데이터 제공
    - `networkMode`
        - **online**: 네트워크가 연결되어 있을 때 데이터 요청
        - **always**: 항상 데이터 요청
        - **offlineFirst**: 캐시 데이터를 1회 요청하고 fetch하지 않음
    - `notifyOnChangeProps`**:** 특정 값이 변경될 때만 리렌더링 발생
    - `refetchInterval`: queryFn을 재실행하여 fetch 받는 주기
    - `refetchIntervalInBackground`: 백그라운드 상태에서 fetch 받는 여부
    - `refetchOnMount`: 데이터가 stale 상태일 때 mount시 fetch 받는 여부
    - `refetchOnWindowFocus`: 화면이 포커싱되었을 때 fetch 받는 여부
    - `retry`: 쿼리가 오류를 표시하기 전 재시도할 횟수
    - `retryDelay`: 재시도할 때의 주기
    - `select`: queryFn을 통해 반환 받은 데이터를 가공하는 기능
        - ex)
            
            ```jsx
            	 const queryData = useQuery({
            						  queryKey: ['todo'],
            							queryFn: fetchTodoList,
            							select: (data) => {
            								const filteredData = data.filter((item)) 
            									=> item.value !== 1}
            								return filteredData;
            							}
            						}) 
            ```
            
    - **throwOnError**: 에러가 발생할 경우 Error Boundary에 에러 전파 ⇒ throw문과 동일
    
- **Parallel Queries**
    
    병렬 쿼리는 여러개의 데이터를 가져올 때 사용한다.
    
    - **Manual Parallel Queries**
        
        ```jsx
        const users = useQuery({ queryKey: ['users'], queryFn: fetchUsers })
        const teams = useQuery({ queryKey: ['teams'], queryFn: fetchTeams })
        ```
        
        ⇒ 여러개의 useQuery를 선언
        
        ⇒ 이전 쿼리가 Promise를 발생시켜 다른 쿼리가 실행되기 전에 구성 요소를 일시 중단하는 문제가 있다.
        
    
    - **Dynamic Parallel Queries**
        
        ```jsx
        const userQueries = useQueries({
            queries: users.map((user) => {
              return {
                queryKey: ['user', user.id],
                queryFn: () => fetchUserById(user.id),
              }
            }),
          })
        ```
        
        ⇒ 여러개의 useQuery를 선언할 필요가 없다. 
        
        ⇒ 실행해야 하는 쿼리 수가 렌더링마다 변경되어 후크 규칙을 위반하는 경우를 방지할 수 있다.
        
    
- **useInfinityQuery**
    
    ```jsx
    const {
        data,
        error,
        fetchNextPage,
        hasNextPage,
        isFetching,
        isFetchingNextPage,
        status,
      } = useInfiniteQuery({
        queryKey: ['projects'],
        queryFn: fetchProjects,
        initialPageParam: 0,
        getNextPageParam: (lastPage, pages) => lastPage.nextCursor,
      })
    ```
    
    무한 스크롤, 무한 데이터 로딩 등에 사용하는 useQuery로 배열의 형태로 데이터를 저장
    
    useQuery와 동일한 옵션을 사용하며, 아래의 추가 옵션들을 가진다.
    
    - `initialPageParam`: 첫 페이지를 가져올 인자
    - `getNextPageParam`: lastPage, allPages, lastPageParam, allPageParams를 사용하여 다음
        
        페이지 호출
        
    - `getPreviousPageParam` : 위와 같은 인자를 받아 이전 페이지 호출
    - `maxPages` : 반환 값에 저장할 최대 페이지 수
    
    반환 데이터
    
    - `data.pages`: 모든 페이지의 데이터
    - `data.pageParams`: 모든 페이지의 호출 인자
    - `isFetching(NextPage or PreviousPage)` : 다음 or 이전 페이지의 pending 여부 확인
    - `isRefetching`: refetching 여부 확인
    - `fetch(NextPage or PreviousPage)` : 다음 or 이전 페이지에 대한 fetch 함수
    - `has(NextPage or PreviousPage)`: 다음 or 이전 페이지 존재 여부 확인

- **useMutation**
    
    ```jsx
    const mutation = useMutation({
    		mutationFn: (newTodo) => {
          return axios.post('/todos', newTodo)
        },
    	})
    ```
    
    서버의 데이터를 변경할 때 사용한다.
    
    - **useQuery와 차이점**
        
        
        | **useMutation** | **useQuery** |
        | --- | --- |
        | 주로 **post** 통신에 사용 | 주로 **get** 통신에 사용 |
        | **CUD**(Create, Update, Delete) 수행 | **R**(Read) 수행 |
        | 반환 함수를 호출하는 방식 | 반환된 데이터를 저장하는 방식 |
        | mutationKey가 필수가 아님 | queryKey가 필수 |
    
    mutation 사용 중 status 상태를 확인하는 요소
    
    - `isIdle`: idle(mutation이 실행되기 전 상태)
    - `isPending`: pending(mutation이 현재 실행 중 (= loading))
    - `isError`: error(오류 발생)
    - `isSuccess`: success(mutationFn이 성공적으로 수행되어 mutation 데이터가 있음)
    
    status 상태 확인 후 실행하거나 반환되는 요소
    
    - `data`: isSuccess가 true가 되어 반환되는 데이터
    - `error`: isError가 true가 되어 발생하는 에러
    - `onMutate`: mutation 실행 전 호출
        
        ⇒ mutationFn과 동일한 인자를 받으며 반환 값은 onError, onSettle에 전달
        
    - `onSuccess`: mutation이 성공했을 때 콜백함수
    - `onError`: mutation이 실패했을 때 콜백함수
    - `onSettled`: mutation이 성공하거나 실패했을 때 콜백함수

`retry`, `retryDelay`, `throwOnError` 등 useQuery와 동일한 함수 존재

`mutateAsync`: mutate와 동일하지만 Promise 객체를 반환

- **QueryClient**
    
    ```jsx
    const queryClient = new QueryClient();
    
    <QueryClientProvider client={queryClient}>
    	...
    </QueryClientProvider>
    ```
    
    캐시와 상호작용하여 쿼리를 호출하거나 저장할 수 있다.
    
    가장 최상단(ex: app)에 provider를 등록한 뒤 queryClient를 사용할 수 있다.
    
    - `fetchQuery`: 쿼리로 데이터를 받아 저장한다. ⇒ **Promise** 형태로 저장
    - `setQueryData`: 쿼리 데이터를 받아 저장한다.
        
        ⇒ 동기적으로 사용 가능한 데이터가 있다고 가정하는 점에서 `fetchQuery` 와 차이가 있음
        
    - `getQueryData`: 쿼리 데이터를 가져온다. ⇒ 데이터가 없다면 **undefined** 반환
    - `ensureQueryData`: 쿼리 데이터를 가져온다. ⇒ 데이터가 없다면 `fetchQuery` 호출
    - `prefetchQuery`: 데이터를 미리 받아 저장. 데이터를 반환하지 않는다.
        
        ⇒ UX 개선에 용이
        
    - `invalidateQueries`: 쿼리를 무효화하고 데이터를 재호출한다.
        
        **refetchType**
        
        | **active** | 활성화된 쿼리 무효화, 데이터 재호출 |
        | --- | --- |
        | **inactive** | 비활성화된 쿼리 무효화, 데이터 재호출 |
        | **all** | 모든 쿼리 무효화, 데이터 재호출 |
        | **none** | 모든 쿼리 무효화, 데이터 재호출 X |
    - `cancelQueries`: 쿼리 가져오기를 취소한다.
    - `removeQueries`: 쿼리의 캐시를 삭제한다.
    - `resetQueries`: 쿼리의 데이터를 초기 상태로 재설정한다.
    - `clear`: 모든 캐시를 삭제한다.
    - `resumePausedMutations`: 일시중지된 mutation을 재실행한다.

### **참고**

- [Tanstack Query 문서](https://tanstack.com/query/latest/docs/framework/react/overview)