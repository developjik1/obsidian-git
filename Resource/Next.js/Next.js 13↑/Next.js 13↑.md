---
sticker: emoji//1f53c
---
## Next.js 13 업데이트

### App Router
- 기존에 pages/ 디렉토리에서 라우팅 되던 방식과 다르게, app/ 디렉토리로 라우팅 하는 방식이 추가됨
- app/ 디렉토리를 생성하여 라우팅을 설정할 수 있으며, 라우팅 환경 개선 뿐만 아니라, 레이아웃, 서버 컴포넌트, 스트리밍, 데이터 패칭까지도 지원하는 형태로 향상
    - Layout: 리렌더링 방지를 위한 레이아웃 제공 (기존 _app.tsx 파일과 비슷한 개념)
    - Server Component: app 디렉토리 내 파일은 디폴트로 서버 컴포넌트로 동작함
    - Streaming: app 디렉토리는 렌더링되는 UI 단위를 점진적으로 렌더링 & 스트리밍할 수 있는 기능 제공
    - Data Fetching 지원: fetch() Web API를 사용할 수 있게 되어, 컴포넌트 레벨에서도 SSR 적용 가능
- app/ 폴더로 기존 파일 시스템 라우팅을 구현할 수 있으며, 여기의 page.js가 해당 경로(라우팅)의 페이지 컴포넌트가 됨
- pages/ 라우팅과 함께 사용 가능 (점진적 업데이트)
- 
- 기존 pages 라우터 방식

```jsx
// pages/index.js
export default function Page() {
  return <h1>Hello, Next.js!</h1>;
}
```

- 새로운 앱 라우터 방식

```jsx
// app/layout.js
export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}

// app/page.js
export default function Page() {
  return <h1>Hello, Next.js!</h1>;
}
```