---
tags: hook
sticker: emoji//1f49b
---
## Hook

```ts
export const useSsr = () => {  
  const isDOM =  
    typeof window !== 'undefined' &&  
    window.document &&  
    window.document.documentElement  
  
  return {  
    isBrowser: isDOM,  
    isServer: !isDOM,  
  }  
}
```

## Description
코드가 실행될 위치를 빠르게 알 수 있습니다.

- 서버에서(Server-Side-Rendering) 또는
- 클라이언트에서 네비게이터

이 hook은 `추가 렌더링을 발생되지 않고` 마운트 시 값을 직접 반환해 값이 변경되어도 다시 트리거되지 않습니다.

그렇지 않은 경우 값이 변경되어 이에 반응할 때 알림을 받으려면 다음을 사용할 수 있습니다.대신에.

## Example
```ts
const Component = () => {  
  const { isBrowser } = useSsr()  
  
  return <p>{isBrowser ? 'Browser' : 'Server'}!</p>  
}
```