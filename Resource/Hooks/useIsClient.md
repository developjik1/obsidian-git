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
주로 SSR 환경에서 일부 기능을 실행하기 위해 브라우저에 있을 때까지 기다리는 데 유용하게 사용할 수 있다.

실제 SSR 애플리케이션에서 useIsClient Hook이 있는 구성 요소가 브라우저에 마운트되면 해당 구성 요소의 상태가 변경되고 다시 렌더링됩니다. 이것이 우리가 원하는 것이지만 추가 렌더링 없는 boolean을 원한다면 을 참조하십시오 [`useSSR()`](https://usehooks-ts.com/react-hook/use-ssr).

## Example
```ts
export default function Component() {  
  const isClient = useIsClient()  
  
  return <div>{isClient ? 'Client' : 'server'}</div>  
}
```