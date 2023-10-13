---
sticker: emoji//1f53c
---
### 0. Update `Next.js`
### 1. create `app` directory

`src directory` 내부 root 에 생성 (`src/app`)

### 2. Root Layout 생성

- `app/layout.tsx `파일을 생성하여 레이아웃 작업
- `<html>`과 `<body>` 태그 정의
- 기존 pages 라우터의 `pages/_app.tsx`와` pages/_document.tsx `파일을 대체하므로, 해당 내용 적용

### 3. next/head 마이그레이션

- Next.js의 `Metadata 객체`로 `<head>` 태그 정의 (SEO)

```jsx
  import { Metadata } from 'next'
 
export const metadata: Metadata = {
  title: 'My Page Title',
}
 
export default function Page() {
  return '...'
}
```

### 4. Pages 마이그레이션

- 기존 index 파일들을 page.js 파일로 변경
- `클라이언트 컴포넌트`를 정의하려면 페이지 상위에 `‘use client’` 디렉티브 입력  
    ![](https://velog.velcdn.com/images/khy226/post/154f01b9-2715-47b9-990d-d97ab068155e/image.png)

### 5. Routing hook 마이그레이션

- 기존 import { useRouter } from 'next/router' 으로 사용했던 Routing Hook은 
  import { useRouter, usePathname, useSearchParams } from 'next/navigation' 으로 변경
- [useRouter()](https://nextjs.org/docs/app/api-reference/functions/use-router), [usePathname()](https://nextjs.org/docs/app/api-reference/functions/use-pathname), [useSearchParams()](https://nextjs.org/docs/app/api-reference/functions/use-search-params)

### 6. Data Fetching 마이그레이션

- 기존에 사용하던 getServerSideProps 나 getStaticProps 메서드로 data fetching X
- React Server Component의 fetch() 와 async 사용

```jsx
export default async function Page() {
  // This request should be cached until manually invalidated.
  // Similar to `getStaticProps`.
  // `force-cache` is the default and can be omitted.
  const staticData = await fetch(`https://...`, { cache: 'force-cache' })
 
  // This request should be refetched on every request.
  // Similar to `getServerSideProps`.
  const dynamicData = await fetch(`https://...`, { cache: 'no-store' })
 
  // This request should be cached with a lifetime of 10 seconds.
  // Similar to `getStaticProps` with the `revalidate` option.
  const revalidatedData = await fetch(`https://...`, {
    next: { revalidate: 10 },
  })
 
  return <div>...</div>
}
```

### (optional 7). Styling

- `tailwind.config.js` 파일에 `./app/**/*.{js,ts,jsx,tsx,mdx}` 해당 내용 추가
- 기존 pages di에서는 글로벌 스타일이 pages/_app.js 에만 한정되었지만, app 디렉토리에서는 레이아웃, 페이지, 컴포넌트에 모두 import 해서 적용 가능