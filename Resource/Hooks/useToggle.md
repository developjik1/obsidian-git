---
tags: hook
sticker: emoji//1f49b
---
## Hook

```ts
import { Dispatch, SetStateAction, useCallback, useState } from 'react' 

interface UseToggleOutput {  
  value: boolean  
  setValue: Dispatch<SetStateAction<boolean>>  
  toggle: () => void  
}  
  
export function useToggle( defaultValue?: boolean ): UseToggleOutput {  
  const [value, setValue] = useState(!!defaultValue)  
  
  const toggle = useCallback(() => setValue(x => !x), [])  
  
  return { value, setValue, toggle }
}
```

## Description

`Boolean` 상태를 위한 간단한 `추상화`입니다.

## Example

```ts
export default const Component = () => {  
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