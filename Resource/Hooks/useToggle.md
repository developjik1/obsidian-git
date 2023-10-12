---
tags: hook
---
## Hook

```ts
import { Dispatch, SetStateAction, useCallback, useState } from 'react' 

interface UseBooleanOutput {  
  value: boolean  
  setValue: Dispatch<SetStateAction<boolean>>  
  toggle: () => void  
}  
  
export function useToggle(  
  defaultValue?: boolean,  
): [boolean, () => void, Dispatch<SetStateAction<boolean>>] {  
  const [value, setValue] = useState(!!defaultValue)  
  
  const toggle = useCallback(() => setValue(x => !x), [])  
  
  return [value, toggle, setValue]  
}

```

## Example
```ts
  
export default const Component = () => {  
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