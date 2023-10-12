---
tags: hook
sticker: emoji//1fa9d
---
## Hook

```ts
import { Reducer, useReducer } from 'react';  
  
const reducer = (state: boolean, nextValue?: boolean) =>typeof nextValue === "boolean" ? nextValue : !state;  
  
const useToggle = (initialValue?: boolean): [boolean, (nextValue?: boolean) => void] => {  
  return useReducer<Reducer<boolean, boolean| undefined>>(reducer, !! initialValue);  
};  
  
export default useToggle;
```

## Description

`Boolean` 상태를 위한 간단한 `추상화`입니다.

## Example

```ts
const Component = () => {  
  const { value, } = useToggle(false)  
  
  return (  
    <>  
      <p>  
        Value is <code>{value.toString()}</code>  
      </p>  
      <button onClick={toggle}>toggle</button>  
    </>  
  )  
}
```