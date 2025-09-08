# Flux 패턴

입력을 기반으로 Action을 Dispatcher에 전달하여 Store의 데이터를 변경하여 View에 위치시키는 애플리케이션 아키텍처이다.

- Flux 패턴의 흐름

![flux.png](Flux%20%ED%8C%A8%ED%84%B4%20461e9ba469b24a2b85a0902cceafa4ee/flux.png)

- MVC 모델의 흐름

![mvc_model.png](Flux%20%ED%8C%A8%ED%84%B4%20461e9ba469b24a2b85a0902cceafa4ee/mvc_model.png)

⇒ 양방향으로 데이터를 전달하는 MVC 모델에 비해 단순한 흐름을 가진다.

**장점**

- 기존의 MVC 모델과 다르게 위의 순서를 따라 데이터를 단방향으로 관리하여 **예측가능성**을 높인다.
- 모든 상태를 Store에서 전체적으로 관리하여 컴포넌트에 변경사항을 전달하기 용이하다.

**단점**

- 상태 변경이 추가될 때마다 코드를 추가적으로 작성해야 하기 때문에 코드의 양이 많다.
- 여러개의 Store를 사용할 경우 의존성 해결을 위한 로직이 추가적으로 필요하다.

**구성**

- **Action**: 데이터를 변경 ⇒ **type**과 **payload**를 묶어 **Dispatcher**에 전달한다.
    
    ```jsx
    {
      type: 'USER_INFO',
      data: {
        'name': 'JAMES',
        'phone_number': '010-1234-1234'
      }
    }
    ```
    
- **Dispatcher**: Action을 받아 Store에 저장하는 중앙 허브 역할
    
    ⇒ Dispatcher에서만 Store의 상태를 조작할 수 있다.
    
- **Store**: 모든 상태와 상태 변경 로직을 저장하는 저장소
- **View**: 컴포넌트를 의미 ⇒ Store에서 새롭게 데이터를 받을 경우 화면을 리렌더링한다.

![flux_process.png](Flux%20%ED%8C%A8%ED%84%B4%20461e9ba469b24a2b85a0902cceafa4ee/flux_process.png)

위와 같이 **View**에서 입력을 통해 **Action**을 발생시키면 **Dispatcher**로 전달하여 **Store**의 데이터를 변경, 변경된 데이터를 다시 **View**로 보내어 리렌더링하는 과정이 반복된다.