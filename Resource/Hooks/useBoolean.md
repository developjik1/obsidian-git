---
tags: hook
sticker: emoji//1f49b
---
## Hook

```ts
import { Dispatch, SetStateAction, useCallback, useState } from 'react'  
  
interface UseBooleanOutput {  
  value: boolean  
  setValue: Dispatch<SetStateAction<boolean>>  
  setTrue: () => void  
  setFalse: () => void  
  toggle: () => void  
}  
  
export const useBoolean = (defaultValue?: boolean): UseBooleanOutput => {  
  const [value, setValue] = useState(!!defaultValue)  
  
  const setTrue = useCallback(() => setValue(true), [])  
  const setFalse = useCallback(() => setValue(false), [])  
  const toggle = useCallback(() => setValue(x => !x), [])  
  
  return { value, setValue, setTrue, setFalse, toggle }  
}
```

## Description

`Boolean` 상태를 위한 간단한 `추상화`입니다.

## Example
```ts
const Component = () => {  
  const { value, setValue, setTrue, setFalse, toggle } = useBoolean(false)  
  
  return (  
    <>  
      <p>  
        Value is <code>{value.toString()}</code>  
        Value is <code>{value.toString()}</code>  
      </p>  
      <button onClick={setTrue}>set true</button>  
      <button onClick={setFalse}>set false</button>  
      <button onClick={toggle}>toggle</button>  
    </>  
  )  
}
```