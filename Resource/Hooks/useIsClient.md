---
tags: hook
sticker: emoji//1f49b
---
## Hook

```ts
import { useEffect, useState } from 'react'  
  
export const useIsClient = () => {  
  const [isClient, setClient] = useState(false)  
  
  useEffect(() => {  
    setClient(true)  
  }, [])  
  
  return isClient  
}
```

## Description
주로 `SSR 환경`에서 일부 기능을 실행하기 위해 브라우저에 있을 때까지 기다리는 데 유용하게 사용할 수 있다.

실제 `SSR 애플리케이션`에서 `useIsClient Hook`이 있는 구성 요소가 `브라우저에 마운트`되면 해당 구성 요소의 상태가 변경되고 `다시 렌더링`됩니다. 

## Example
```ts
const Component = () => {  
  const isClient = useIsClient()  
  
  return <div>{isClient ? 'Client' : 'server'}</div>  
}
```