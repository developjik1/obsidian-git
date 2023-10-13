---
sticker: emoji//1f5fd
---
리액트에서 상태 관리를 위해 사용되는 주요 훅은 다음과 같습니다:

1. **useState**: `useState`는 함수 컴포넌트에서 상태를 추가하는 가장 기본적인 훅입니다. 컴포넌트 내부에서 상태 값을 선언하고 업데이트할 때 사용됩니다.

```jsx
import React, { useState } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

2. **useEffect**: `useEffect`는 부수 효과(side effect)를 다루기 위한 훅으로, 컴포넌트 렌더링과 관련없는 작업을 수행하는 데 사용됩니다. 예를 들어 데이터 가져오기, 구독 관리, 라이프사이클 이벤트 처리 등을 할 수 있습니다.

```jsx
import React, { useState, useEffect } from 'react';

function Example() {
  const [data, setData] = useState([]);

  useEffect(() => {
    // 데이터 가져오기 또는 다른 부수 효과 작업 수행
  }, []); // 빈 배열은 컴포넌트가 처음 마운트될 때만 실행
}
```

3. **useContext**: `useContext`는 컴포넌트 트리 안에서 상태를 전파하기 위한 훅으로, 주로 전역 상태 관리를 위해 사용됩니다. 하위 컴포넌트에서 상위 컴포넌트의 컨텍스트에 접근할 수 있게 합니다.

```jsx
import React, { useContext } from 'react';

const MyContext = React.createContext();

function Example() {
  const value = useContext(MyContext);

  return <p>Value from context: {value}</p>;
}
```

4. **useReducer**: `useReducer`는 복잡한 상태 관리 로직을 다룰 때 사용하는 훅입니다. 상태의 변경 로직을 분리하고 더 예측 가능한 방식으로 관리할 수 있습니다.

5. **커스텀 훅**: 이를 통해 특정 컴포넌트에서 자주 사용하는 로직을 추상화하고 재사용성을 높일 수 있습니다.
