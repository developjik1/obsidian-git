---
sticker: emoji//1f53d
---
  
## Next.js 기본 렌더링 방식

Next.js는 SSR을 기반으로 하지만, 페이지가 로드된 이후에는 React의 CSR 방식을 이용합니다.
1. Next.js 서버가 페이지를 그립니다. pages/안에 폴더를 만들면, 해당 라우팅의 페이지들은 서버측에서 먼저 로드합니다.
2. 페이지가 그려진 이후 페이지 내부에서 동적인 데이터를 패치하는 과정(axios 등)은 CSR의 방식을 따릅니다.
   이때의 데이터들은 일단 페이지가 로드된 이후에 클라이언트 측에서 다시 렌더됩니다. 따라서 이 데이터는 SEO에 걸리지 않습니다.

<br/>

## Next.js의 3가지 렌더링 방식 알아보기

랜더링 방식은 크게 `Static Generation(SSG)`, `SSR`, `CSR`가 있습니다.
우리가 보통 Next.js에서 기대하는 렌더링은 SSG과 SSR입니다.  

pages폴더 밑에 있는 모든 컴포넌트는 빌드시 HTML을 SSG 하거나, 요청이 올 때마다 서버측에서 HTML을 생성하는 SSR 렌더링을 합니다.

Next.js의 메서드를 어떻게 활용하느냐에 따라 빌드시점에 세가지 방식의 렌더링 중 하나로 결정됩니다.

만약, 페이지가 로드될 때 함께 데이터가 패칭되어야 하는 상황이라면(pre-rendering) 
Next.js의 데이터 패칭 방식 (getInitialProps, getStaticProps, getStaticPath, getServerSideProps)을 이용해 첫 렌더에 데이터가 패칭될 수 있도록 처리를 해주어야 합니다.

<br/>

## 정적생성(Static Generation)

- 빌드 시점에 온전한 페이지의 HTML이 생성되어 서버에서 물리적으로 HTML파일을 모두 갖고 있는 상태입니다.
- 페이지를 요청할 때 서버에서 갖고 있던 해당 페이지의 HTML을 응답합니다.
- Next.js에서 권장하는 렌더링 방식이며, 한 번 응답한 후에는 CDN(content delivery network)이 파일을 기억(캐쉬, cache)하여 응답하기 때문에 화면을 그리는 속도가 굉장히 빨라지고 불필요한 서버 요청이 줄어듭니다.
- 정적생성을 하기 위해서는 `getStaticProps`, `getStaticPaths` 메서드를 사용하여 데이터를 가져오거나, 데이터를 가져오는 메서드 없이 컴포넌트를 만들면 됩니다.
- 주로 Static이 가능한 페이지는 회사 소개 페이지, 개인정보처리 방침 같은 설명 페이지, 정적 블로그 페이지 등이 있습니다.

<br/>

## SSR(Server Side Rendering)

- SSR은 페이지 요청이 있으면 server side에서 api를 호출하고 데이터를 응답받아서 서버측에서 HTML을 완성시키고 클라이언트에 전달합니다.
    - 요청을 할 때마다 서버측에서 HTML을 만들 시간이 필요하기 때문에 HTML을 응답하는 시간이 오래걸릴 수 있다는 단점이 있습니다.
- 페이지를 요청할 때마다 SSR을 하고 싶은 경우 `getInitialProps`,`getServerSideProps`메서드를 사용해야합니다.

<br/>

## CSR(Client Side Rendering)

- Next.js로 프로젝트를 만들면서도 CSR이 필요한 경우도 있다. CSR은 일단 빈 html을 응답하고, 그 후에 브라우저에서 데이터를 호출하여 자바스크립트에서 DOM을 조작하여 마저 HTML을 완성합니다.
- SEO가 필요없는 마이페이지, 관리자페이지 등은 클라이언트 측에서 데이터를 호출해도 됩니다. 이 때는 Next.js 메서드가 아닌 react에서 **`componentDidMount`** 라이프사이클이나 **`useEffect`** 훅에서 데이터를 요청합니다.
- componentDidMount, useEffect 등은 마운트 되었을때 실행이 되기 때문에 SSR로 html을 받은 후 동작한다.

<br/>

## Next.js ServerSide Cycle 알아보기
1. Next Server가 GET 요청을 받는다.
2. 요청에 맞는 Page를 찾는다.
3. _app.jsx의 getInitialProps가 있다면 실행한다.
4. Page Component의 getInitialProps가 있다면 실행한다. pageProps들을 받아온다.
5. document.tsx의 getInitialProps가 있다면 실행한다. pageProps들을 받아온다.
6. 모든 props들을 구성하고, _app.jsx > page Component 순서로 rendering한다.
7. 모든 Content를 구성하고 _document.js를 실행하여 html 형태로 출력한다.

<br/>

즉, 모든 페이지에 공통적인 데이터 패칭이 필요하다면 _app.jsx에서 미리 데이터 패칭을 해주면 되고,
페이지마다 다른 데이터가 필요하다면 페이지마다 데이터 패칭을 해주면 됩니다.

Next.js 9.3버전 이후엔 SSR과 SSG를 분리해 getStaticProps / getServerSideProps로 나눠졌으며, 전역적인 데이터 패치 기능을 지원하지 않습니다.
따라서 전역적으로 SSR의 데이터 패칭을 해야만 하는 경우라면 getInitialProps를 써야만 전역적인 패치가 가능합니다. 하지만, 추천되지는 않습니다.

<br/>

## Next.js 데이터 패칭 메소드 알아보기

기본적으로 데이터 패칭 메소드들은 리턴한 값을 해당 컴포넌트의 props로 보냅니다.

- getInitialProps: 모든 페이지에 공통적인 데이터 패칭이 필요하다면 _app.jsx에서 getInitialProps를 사용하지만 권장되지는 않는다.
- getStaticProps: 빌드시 고정되는 값으로, 빌드이후에는 변경이 불가능합니다.
- getStaticPaths: 동적라우팅 + getStaticProps를 원할 때 사용합니다.
- getServerSideProps: 빌드와 상관없이, 매 요청마다 데이터를 서버로부터 가져옵니다.

### getInitialProps

Next.js 9.3버전 이전엔 getInitialProps만으로 데이터 패치를 전부 해결했지만,
9.3버전부터는 getInitialProps가 getStaticProps, getStaticPath (Static Generation) / getServerSideProps (Server-side Rendering) 로 분화되었습니다.

_app.js 뿐만 아니라 페이지 별로 데이터 패칭하는 것도 getInitialProps로 가능하지만
그것이 정적데이터인지, 페이지 요청마다 렌더되는 데이터인지에 따라 그 방식을 분리한 것이 getStaticProps / getStaticPath / getServerSideProps 이며,
Next.js 9.3버전 이후 Next에선 이 두 가지를 이용하는 것을 권고하고 있습니다.

getinitialProps는 context를 기본 props로 받는데, 그 내부엔 ctx객체와 Component가 존재합니다.
Component는 해당 컴포넌트를 의미하며, Component로 보내는 ctx 객체의 구성은 다음과 같습니다.

> pathname - 현재 pathname <br/>
query - 현재 query를 object형태로 출력 <br/>
asPath - 전체 path <br/>
req - HTTP request object (server only) <br/>
res - HTTP response object (server only) <br/>
err - Error object if any error is encountered during the rendering

<br/>

```javascript
// pages/_app.js
// getinitialProps 예제
import Head from "next/head";

function MyApp({ Component, pageProps }) {
  return (
    <>
      <Head>
        <title>Next!</title>
      </Head>
       <Component {...pageProps} />
    </>
  );
}

MyApp.getInitialProps = async (context) => {
  console.log('getInitialProps 시작');
  const { ctx, Component } = context;
  let pageProps = {};

  if (Component.getInitialProps) {
    pageProps = await Component.getInitialProps(ctx);
  }

  console.log('getInitialProps 종료');
  return { pageProps };
};

export default MyApp

```

> ⚠️ 주의1 ⚠️ : _app.js에서 전역적으로 데이터 패칭을 할 경우, getStaticProps나 getServerSideProps를 지원하지 않기 때문에 getInitialProps를 이용해야 합니다.
또한, _app에서 getInitialProps를 사용해 모든 페이지에서 사용할 공통 속성값을 지정할 수 있으나, 이럴 경우 자동 정적 최적화(Automatic Static Optimization)가 비활성화되어 모든 페이지가 SSR을 통해 제공됩니다.

> ⚠️ 주의2 ⚠️ : getInitialProps 내부 로직은 서버에서 실행되기 때문에 Client에서만 가능한 로직은 피해야 합니다. (Window, document 등)

> ⚠️ 주의3 ⚠️ : 한 페이지를 로드할 때, 하나의 getInitialProps 로직만 실행됩니다. 예를 들어 _app.js에 getInitialProps를 달아서 사용한다면 그 하부 페이지의 getInitialProps는 실행되지 않습니다. 이에 대한 해결법으로는 커스터마이징을 통해 따로 처리해줘야 합니다.

> 자동 정적 최적화(Automatic Static Optimization) : getInitialProps가 없으면, 페이지를 정적 HTML 으로 사전렌더링 해서 정적 최적화를한다. SSR 계산이 없기 때문에 사용자에게 즉시 뿌려지는 ultra fast 로딩이다.
Next.js는 기본적으로 데이터 요구 사항이 없는 경우 페이지가 정적인지 자동으로 확인하고, getInitialProps나 getServerSideProps를 사용하지 않는다면 페이지를 정적 HTML로 사전에 렌더링하여 자동으로 페이지를 최적화합니다.

즉, 이 모든 것을 고려해보았을 때, getInitialProps를 활용해 _app.jsx에서 데이터 패칭을 하는 것은 지양하는 것이 좋다.

### getStaticProps

data를 빌드시에 미리 가져와서 static하게 제공하기 때문에 굉장히 빠른 속도로 페이지가 렌더됩니다. 
따라서 매 유저의 요청마다 fetch할 필요가 없는 데이터를 가진 페이지를 렌더링 할때 매우 좋습니다.

다음과 같은때 getStaticProps를 활용하면 좋습니다.
- headless CMS로 부터 데이터가 올때
- 유저에 구애받지 않고 퍼블릭하게 캐시할 수 있는 데이터
- SEO 등의 이슈로 인해 빠르게 미리 렌더링 해야만 하는 페이지. 

또하, getStaticProps는 HTML과 JSON파일을 모두 생성해 두기 때문에, 성능을 향상시키기 위해 CDN 캐시를 하기 쉽습니다.

getStaticProps가 기본 props로 받는 context 객체의 구성은 다음과 같습니다

> params: 다이나믹 라우트 페이지라면, params를 라우트 파라미터 정보를 가지고 있다.<br/>
req: HTTP request object<br/>
res: HTTP response object<br/>
query: 쿼리스트링<br/>
preview: preview 모드 여부 >공식문서<br/>
previewData: setPreviewData로 설정된 데이터<br/>

<br/>

getStaticProps가 리턴할 수 있는 값은 다음과 같습니다
> props : 해당 컴포넌트로 리턴할 값 (선택적)<br/>
revalidate : 페이지 재생성이 발생할 수 있는 시간(초). 기본값은 false이며, 이게 거짓이면 다음 빌드때까지 페이지가 빌드된 상태로 캐시됨. (선택적)<br/>
notFound : Boolean값, 404status를 보내는 것을 허용한다. (선택적)<br/>

```javascript
//pages/get-static-props/index.js
// getStaticProps 예제
import React from 'react';
import axios from "axios";
import Link from "next/link";
import {C} from "common/styles";

function GetStaticProops(props) {
  return (
    <C>
      <Link href="/">
        <a>
          돌아가기
        </a>
      </Link>
      {
        props.data && <div>uuid: {props.data.uuid}</div>
      }
    </C>
  );
}

export const getStaticProps = async (context) => {
  console.log("getStaticProps 시작");
  const res = await axios.get('http://localhost:3000/api/uuid')
  const data = await res.data
  console.log("data 값: ", data)
  console.log("getStaticProps 종료");

  return {
    props: {
      data
    },
  }
}

export default GetStaticProops;
```

### getStaticPaths

페이지가 동적 라우팅을 쓰고 있고, getStaticProps를 쓰는 경우, getStaticPaths을 통해 빌드 타임 때 정적으로 렌더링할 경로를 설정해야합니다. 
여기서 정의하지 않은 하위 경로는 접근해도 화면이 뜨지 않습니다. 
동적라우팅 할 때, 라우팅 되는 경우의 수를 하나하나 집어넣어야 합니다.

getStaticPaths가 리턴할 수 있는 값은 다음과 같습니다

> paths : 빌드타임에 pre-rendering할 경로들<br/>
fallback : paths 이외의 경로들에 대해 추후 요청이 들어오면 만들어 줄지 말지. false이면 정해진 paths 이외의 요청에 404를 리턴함.


```javascript
// pages/get-static-paths/[id].js

// 이 페이지에서 렌더될 컴포넌트
import React from "react";
import Link from "next/link";

import axios from "axios";

import {C} from "../../common/styles";

function GetStaticPaths(props) {
  return (
    <C>
      <Link href="/">
        <a>
          돌아가기
        </a>
      </Link>
      {
        props.data && <div>uuid: {props.data.uuid}</div>
      }
    </C>
  )
}

// 빌드될 때 실행
export const getStaticPaths = async (context) => {
  console.log("getStaticPaths 시작");
  const res = await axios.get('http://localhost:3000/api/uuid')
  const data = await res.data

  // pre-render할 Path를 얻음
  const paths = [
    {
      params: { id: "1", },
    },
    {
      params: { id: "2", },
    },
    {
      params: { id: "3", },
    },
  ]

  console.log("data 값: ", data)
  console.log("getStaticPaths 종료");

  // 우리는 오로지 이 path들만(즉, 위의 path의 params에 정의한 값들) 빌드타임에 프리렌더 함
  // 즉, http://localhost:3000/get-static-paths/1, http://localhost:3000/get-static-paths/2, http://localhost:3000/get-static-paths/3 만 위의 예제에서 접근이 가능하다
  // { fallback: false } 는 다른 routes들은 404임을 의미
  // true이면 만들어지지 않은 것도 추후 요청이 들어오면 만들어 줄 거라는 뜻
  return { paths, fallback: false }
}

// 빌드될 때 실행 
export const getStaticProps = async (context) => {
  console.log("getStaticProps 시작");
  const res = await axios.get('http://localhost:3000/api/uuid')
  const data = await res.data
  console.log("data 값: ", data)
  console.log("getStaticProps 종료");

  return {
    props: {
      data
    },
  }
}

export default GetStaticPaths
```

### getServerSideProps
getServerSideProps는 페이지를 렌더링하기전에 반드시 fetch해야할 데이터가 있을 때 사용합니다. 
매 페이지 요청시마다 호출되므로 getStaticProps보다 느리지만, 빌드 이후에도 페이지 요청마다 실행된다는 특징이 있습니다.
getServerSideProps는 서버사이드에서만 실행되고, 절대로 브라우저에서 실행되지 않는다.
getServerSideProps는 매 요청시 마다 실행되고, 그 결과에 따른 값을 props로 넘겨준 뒤 렌더링을 한다.

getServerSideProps가 기본 props로 받는 context 객체의 구성은 다음과 같습니다
>params: 다이나믹 라우트 페이지인 경우, params를 라우트 파라미터 정보를 가지고 있다.<br/>
req: HTTP request object<br/>
res: HTTP response object<br/>
query: 쿼리스트링<br/>
preview: preview 모드 여부<br/>
previewData: setPreviewData로 설정된 데이터<br/>

getServerSideProps가 리턴할 수 있는 값은 다음과 같습니다

> props : 해당 컴포넌트로 리턴할 값 (선택적)<br/>
redirect : 값 내부와 외부 리소스 리디렉션 허용한다. (선택적) 
무조건 { destination: string, permanent: boolean } 의 형태이어야 한다. 
>몇몇 드문 케이스에서 오래된 HTTP클라이언트를 적절히 리디렉션하기 위해 커스텀 status코드가 필요할 수 있는데, 그땐 permanent property 대신에 statusCode property를 이용한다.<br/>
notFound : Boolean값, 404status를 보내는 것을 허용한다. (선택적)<br/>

```javascript
// 이 페이지에서 렌더될 컴포넌트
import React from "react";
import axios from "axios";
import Link from "next/link";
import {C} from "common/styles";

function GetServerSideProps(props) {
  return (
    <C>
      {
        props.data && <div>uuid: {props.data.uuid}</div>
      }
      <Link href="/">
        <a>
          돌아가기
        </a>
      </Link>
    </C>
  )
}

// 빌드될 때 실행 
export const getServerSideProps = async (context) => {
  console.log("getServerSideProps 시작");
  const res = await axios.get('http://localhost:3000/api/uuid')
  const data = await res.data
  console.log("data 값: ", data)
  console.log("getServerSideProps 종료");

  return {
    props: {
      data
    },
  }
}

export default GetServerSideProps
```

<br/>
<br/>

최종적으로, Next.js에서 우리는 데이터를 요청할 때 SSR이 되어야 하는 데이터인지, 아닌지 판단을 하며 로직을 구현해야한다.