---
sticker: emoji//1fa9d
---
```ts
import { useState } from "react"  
  
const useArray<T> = (defaultValue:T) => {  
  const [array, setArray] = useState<T>(defaultValue || [])  
  
  const push = (element) => {  
    setArray(a => [...a, element])  
  }  
  
  const filter = (callback) => {  
    setArray(a => a.filter(callback))  
  }  
  
  const update = (index, newElement) => {  
    setArray(a => [  
      ...a.slice(0, index),  
      newElement,  
      ...a.slice(index + 1, a.length),  
    ])  
  }  
  
  const remove = (index) => {  
    setArray(a => [...a.slice(0, index), ...a.slice(index + 1, a.length)])  
  }  
  
  const clear = () => {  
    setArray([])  
  }  
  
  return { array, set: setArray, push, filter, update, remove, clear }  
}

export default useArray;
```