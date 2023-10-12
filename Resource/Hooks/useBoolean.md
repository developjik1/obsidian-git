---
tags: hook
sticker: emoji//1fa9d
---
## Hook

```ts
import { Reducer, useReducer } from 'react';  
  
const reducer = (state: boolean, nextValue?: boolean) =>typeof nextValue === "boolean" ? nextValue : !state;  
  
const useBoolean = (initialValue?: boolean): [boolean, (nextValue?: boolean) => void] => {  
  return useReducer<Reducer<boolean, boolean| undefined>>(reducer, !! initialValue);  
};  
  
export default useBoolean;
```

## Description

`Boolean` 상태를 위한 간단한 `추상화`입니다.

## Example

```ts
const Component = () => {  
  const [open, toggle] = useToggle()  
  
  return (  
    <>  
      <p>  
        Value is <code>{open.toStrig()}</code>  
      </p>  
      <button onClick={()=>{toggle()}}>toggle</button>  
      <button onClick={()=>{toggle(true)}}>toggle</button>  
      <button onClick={()=>{toggle(false)}}>toggle</button>  
    </>  
  )  
}
```