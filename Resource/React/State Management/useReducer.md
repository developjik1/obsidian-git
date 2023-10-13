---
tags: react
sticker: emoji//1f5fd
---
## Description

`useReducer`는 react 에서 상태 관리를 위한 훅 중 하나로, 복잡한 상태 관리 논리를 보다 효과적으로 다룰 수 있도록 도와줍니다. `useReducer`는 일반적으로 컴포넌트에서 여러 상태 값이 연관되어 있고, 이들의 변경이 복잡한 로직을 필요로 할 때 사용됩니다. 이 훅을 사용하면 상태 업데이트 로직을 더 분리하고, 더 예측 가능한 방식으로 상태를 변경할 수 있습니다.

`useReducer`는 `useState`와 함께 사용되는데, `useState`는 각 상태 값에 대한 개별적인 `setState` 함수를 제공하고, `useReducer`는 하나의 상태 값에 대한 업데이트를 관리하는 데 적합합니다.

`useReducer` 로 구현했을 때의 장점은 `useState` 의 `setState` 함수를 여러번 사용하지 않아도 된다는점과, Reducer로 로직을 분리했으니 다른곳에서도 쉽게 재사용을 할 수 있다는 점 입니다.

## Definition

```jsx
const [state, dispatch] = useReducer(reducer, initialArg, init);
```

- `state`: 현재 상태 값을 나타내는 변수입니다.
- `dispatch`: 상태를 업데이트하기 위한 함수로, 액션 객체를 받아서 상태를 변경합니다.

`useReducer`의 매개변수는 다음과 같습니다:

- `reducer`: 액션을 처리하고 새로운 상태를 반환하는 함수입니다. 이 함수는 현재 상태와 액션 객체를 받아서 다음 상태를 계산합니다.

- `initialArg`: 초기 상태 값이나 초기화 함수입니다.

- `init` (선택적): 초기화 함수로, 초기 상태를 계산하는 데 사용됩니다. 초기화 함수를 제공하면 `initialArg`는 무시됩니다.

## Example

예를 들어, 간단한 카운터를 `useReducer`를 사용하여 관리하는 코드는 다음과 같이 작성할 수 있습니다:

```jsx
import React, { useReducer } from 'react';

function counterReducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(counterReducer, { count: 0 });

  return (
    <div>
      Count: {state.count}
      <button onClick={() => dispatch({ type: 'increment' })}>Increment</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>Decrement</button>
    </div>
  );
}

export default Counter;
```

`counterReducer` 함수는 액션에 따라 상태를 업데이트하는 역할을 하며, `dispatch` 함수를 통해 액션을 전달하여 상태를 변경합니다. 이렇게 하면 복잡한 상태 관리 로직을 분리하고 관리하기 쉽게 만들 수 있습니다.