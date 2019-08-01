[![Next.js](https://assets.zeit.co/image/upload/v1538361091/repositories/next-js/next-js.png)](https://nextjs.org)

[![NPM version](https://img.shields.io/npm/v/next.svg)](https://www.npmjs.com/package/next)
[![Build Status](https://travis-ci.org/zeit/next.js.svg?branch=master)](https://travis-ci.org/zeit/next.js)
[![Build Status](https://dev.azure.com/nextjs/next.js/_apis/build/status/zeit.next.js)](https://dev.azure.com/nextjs/next.js/_build/latest?definitionId=3)
[![Join the community on Spectrum](https://withspectrum.github.io/badge/badge.svg)](https://spectrum.chat/next-js)

**Visit [nextjs.org/learn](https://nextjs.org/learn) to get started with Next.js.**

---

**아래 문서는 `canary` (prerelease) 버전을 기준으로 작성 되었습니다. 가장 최신의 문서를 확인 하려면 [nextjs.org/docs] 에서 확인해주세요.(https://nextjs.org/docs).**

---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
<!-- https://github.com/thlorenz/doctoc -->

- [사용법](#how-to-use)
  - [설치](#setup)
  - [자동 코드 스플리팅](#automatic-code-splitting)
  - [CSS](#css)
    - [Built-in CSS support](#built-in-css-support)
    - [CSS-in-JS](#css-in-js)
    - [Importing CSS / Sass / Less / Stylus files](#importing-css--sass--less--stylus-files)
  - [Static file serving (e.g.: images)](#static-file-serving-eg-images)
  - [다이나믹 루팅](#dynamic-routing)
  - [Populating `<head>`](#populating-head)
  - [Fetching data and component lifecycle](#fetching-data-and-component-lifecycle)
  - [Routing](#routing)
    - [With `<Link>`](#with-link)
      - [With URL object](#with-url-object)
      - [Replace instead of push url](#replace-instead-of-push-url)
      - [Using a component that supports `onClick`](#using-a-component-that-supports-onclick)
      - [Forcing the Link to expose `href` to its child](#forcing-the-link-to-expose-href-to-its-child)
      - [Disabling the scroll changes to top on page](#disabling-the-scroll-changes-to-top-on-page)
    - [Imperatively](#imperatively)
    - [Intercepting `popstate`](#intercepting-popstate)
      - [With URL object](#with-url-object-1)
      - [Router Events](#router-events)
      - [Shallow Routing](#shallow-routing)
    - [useRouter](#userouter)
    - [Using a Higher Order Component](#using-a-higher-order-component)
  - [Prefetching Pages](#prefetching-pages)
    - [With `<Link>`](#with-link-1)
    - [Imperatively](#imperatively-1)
  - [API Routes](#api-routes)
    - [Dynamic routes support](#dynamic-routes-support)
    - [API Middlewares](#api-middlewares)
    - [Helper Functions](#helper-functions)
  - [Custom server and routing](#custom-server-and-routing)
    - [Disabling file-system routing](#disabling-file-system-routing)
    - [Dynamic assetPrefix](#dynamic-assetprefix)
  - [Dynamic Import](#dynamic-import)
    - [Basic Usage (Also does SSR)](#basic-usage-also-does-ssr)
    - [With named exports](#with-named-exports)
    - [With Custom Loading Component](#with-custom-loading-component)
    - [With No SSR](#with-no-ssr)
  - [Custom `<App>`](#custom-app)
  - [Custom `<Document>`](#custom-document)
    - [Customizing `renderPage`](#customizing-renderpage)
  - [Custom error handling](#custom-error-handling)
  - [Reusing the built-in error page](#reusing-the-built-in-error-page)
  - [Custom configuration](#custom-configuration)
    - [Setting a custom build directory](#setting-a-custom-build-directory)
    - [Disabling etag generation](#disabling-etag-generation)
    - [Configuring the onDemandEntries](#configuring-the-ondemandentries)
    - [Configuring extensions looked for when resolving pages in `pages`](#configuring-extensions-looked-for-when-resolving-pages-in-pages)
    - [Configuring the build ID](#configuring-the-build-id)
    - [Configuring next process script](#configuring-next-process-script)
  - [Customizing webpack config](#customizing-webpack-config)
  - [Customizing babel config](#customizing-babel-config)
  - [Exposing configuration to the server / client side](#exposing-configuration-to-the-server--client-side)
    - [Build-time configuration](#build-time-configuration)
    - [Runtime configuration](#runtime-configuration)
  - [Starting the server on alternative hostname](#starting-the-server-on-alternative-hostname)
  - [CDN support with Asset Prefix](#cdn-support-with-asset-prefix)
- [Automatic Prerendering](#automatic-prerendering)
- [Production deployment](#production-deployment)
  - [Serverless deployment](#serverless-deployment)
    - [One Level Lower](#one-level-lower)
    - [Summary](#summary)
- [Browser support](#browser-support)
- [TypeScript](#typescript)
  - [Exported types](#exported-types)
- [AMP Support](#amp-support)
  - [Enabling AMP Support](#enabling-amp-support)
  - [AMP First Page](#amp-first-page)
  - [Hybrid AMP Page](#hybrid-amp-page)
  - [AMP Page Modes](#amp-page-modes)
  - [AMP Behavior with `next export`](#amp-behavior-with-next-export)
  - [Adding AMP Components](#adding-amp-components)
  - [AMP Validation](#amp-validation)
  - [TypeScript Support](#typescript-support)
- [Static HTML export](#static-html-export)
  - [Usage](#usage)
  - [Limitation](#limitation)
- [Multi Zones](#multi-zones)
  - [How to define a zone](#how-to-define-a-zone)
  - [How to merge them](#how-to-merge-them)
- [Recipes](#recipes)
- [FAQ](#faq)
- [Contributing](#contributing)
- [Authors](#authors)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## 사용법

### 설치

아래 코드를 실행한다:

```bash
npm install --save next react react-dom
```

당신의 package.json에 아래와 같은 json script를 추가한다:

```json
{
  "scripts": {
    "dev": "next",
    "build": "next build",
    "start": "next start"
  }
}
```

그 뒤에, file-system 이 main API 가 된다. 모든 `.js` 파일들은 자동적으로 실행되고 렌더링 되는 루트가 될 것이다.

`./pages/index.js` 를 당신의 프로젝트에 추가하자:

```jsx
function Home() {
  return <div>Welcome to Next.js!</div>
}

export default Home
```

그리고 `npm run dev` 를 실행 시킨 뒤에 `http://localhost:3000` 를 확인하세요. 다른 포트를 사용하려면, 다음과 같이 실행한다. `npm run dev -- -p <your port here>`.

지금까지 우리가 얻은 건 :

- 자동적인 코드 변환과 번들링 (webpack 과 babel 을 이용)
- 핫코드 리로딩
- 서버 렌더링과 `./pages/` 인덱싱
- 고정파일 서빙(Static file serving). `./static/` 은 `/static/` 로 맵핑 된다. (given you [create a `./static/` directory](#static-file-serving-eg-images) inside your project)

### 자동 코드 스플리팅

당신이 선언한 모든 `import` 는 각각의 페이지에서 적용되게 된다. 그 뜻은 페이지들이 필요 없는 코드들을 절대 로드 하지 않는 다는 것!

```jsx
import cowsay from 'cowsay-browser'

function CowsayHi() {
  return <pre>{cowsay.say({ text: 'hi there!' })}</pre>
}

export default CowsayHi
```

### CSS

#### Built-in CSS support

<details>
  <summary><b>Examples</b></summary>
  <ul>
    <li><a href="/examples/basic-css">Basic css</a></li>
  </ul>
</details>

We bundle [styled-jsx](https://github.com/zeit/styled-jsx) to provide support for isolated scoped CSS. The aim is to support "shadow CSS" similar to Web Components, which unfortunately [do not support server-rendering and are JS-only](https://github.com/w3c/webcomponents/issues/71).

```jsx
function HelloWorld() {
  return (
    <div>
      Hello world
      <p>scoped!</p>
      <style jsx>{`
        p {
          color: blue;
        }
        div {
          background: red;
        }
        @media (max-width: 600px) {
          div {
            background: blue;
          }
        }
      `}</style>
      <style global jsx>{`
        body {
          background: black;
        }
      `}</style>
    </div>
  )
}

export default HelloWorld
```

더 많은 예제를 원한다면 [styled-jsx documentation](https://www.npmjs.com/package/styled-jsx) 를 확인한다.

#### CSS-in-JS

<details>
  <summary>
    <b>Examples</b>
  </summary>
  <ul>
    <li><a href="/examples/with-styled-components">Styled components</a></li>
    <li><a href="/examples/with-styletron">Styletron</a></li>
    <li><a href="/examples/with-glamor">Glamor</a></li>
    <li><a href="/examples/with-cxs">Cxs</a></li>
    <li><a href="/examples/with-aphrodite">Aphrodite</a></li>
    <li><a href="/examples/with-fela">Fela</a></li>
  </ul>
</details>

존재하는 어떠한 CSS-in_JS 솔루션을 사용할 수 있습니다. 제일 심플한 방법은 inline styles 이다 :

```jsx
function HiThere() {
  return <p style={{ color: 'red' }}>hi there</p>
}

export default HiThere
```

더 복잡하고 정교한 CSS-in-JS solutions을 사용하길 원한다면, you typically have to implement style flushing for server-side rendering. We enable this by allowing you to define your own [custom `<Document>`](#custom-document) component that wraps each page.

#### Importing CSS / Sass / Less / Stylus files

`.css`, `.scss`, `.less` 혹은 `.styl` 를 임포트 시키기 위해서는 files you can use these modules, which configure sensible defaults for server rendered applications.

- [@zeit/next-css](https://github.com/zeit/next-plugins/tree/master/packages/next-css)
- [@zeit/next-sass](https://github.com/zeit/next-plugins/tree/master/packages/next-sass)
- [@zeit/next-less](https://github.com/zeit/next-plugins/tree/master/packages/next-less)
- [@zeit/next-stylus](https://github.com/zeit/next-plugins/tree/master/packages/next-stylus)

### Static file serving (e.g.: images)

<details>
  <summary><b>Examples</b></summary>
  <ul>
    <li><a href="/examples/public-file-serving">Public file serving</a></li>
  </ul>
</details>

딩신의 프로젝트의 루트에 `static` 라는 디렉토리를 생성한다. From your code you can then reference those files with `/static/` URLs:

```jsx
function MyImage() {
  return <img src="/static/my-image.png" alt="my image" />
}

export default MyImage
```

<!--
To serve static files from the root directory you can add a folder called `public` and reference those files from the root, e.g: `/robots.txt`.
-->

_Note: `static` 이라는 디렉토리 이름은 다른 곳에서 사용하지 말 것. 저 이름은 바뀔 수 없는 것이고 Next.js의 고정파일 서빙의 자산들을만을 위한 곳이다.._

### 다이나믹 루팅 (Dynamic Routing)

<details>
  <summary><b>Examples</b></summary>
  <ul>
    <li><a href="/examples/dynamic-routing">Dynamic routing</a></li>
  </ul>
</details>

고정 패스를 사용하여 루트를 정의 하는 것은 복잡도가 높은 어플리케이션들에게는 충분 하지 않을 경우가 많다. Next.js 에서는 괄호를 추가하여 (`[param]`)에 동적인 루트를 생성할 수 있다. (a.k.a. url slugs, pretty urls, et al).

다음과 같은 페이지를 만든다고 가정해본다.  `pages/post/[pid].js`:

```jsx
import { useRouter } from 'next/router'

const Post = () => {
  const router = useRouter()
  const { pid } = router.query

  return <p>Post: {pid}</p>
}

export default Post
```

`/post/1`, `/post/abc` 같은 어떤 루트라도 `pages/post/[pid].js` 와 매치 시킬 수 있다.
매치 되는 패스의 파라메터는 마치 쿼리 파라메터처럼 페이지로 보내지게 된다. 

예를 들어서, `/post/abc` 라는 루트는 다음과 같은 `쿼리` 오브젝트를 가지게 된다 : `{ pid: 'abc' }`.
유사한 예로, `/post/abc?foo=bar` 라는 루트는 다음과 같은 `쿼리` 오브젝트를 가지게 된다: `{ foo: 'bar', pid: 'abc' }`.

`/post/abc`로 이동하기 위한 `<Link>`는 다음과 같이 보여지게 된다 :

```jsx
<Link href="/post/[pid]" as="/post/abc">
  <a>First Post</a>
</Link>
```

- `href`:  `pages` 패스 안에 위치한 디렉토리.
- `as`: 브라우저 바에서 리렌더링 되어서 보여질 주소.

`href` 가 파일시스템의 주소로 사용되기 때문에 런타임에서는 변경되지 않고, 그 대신, 필요한 경우 `as` 의 값을 변경하여야 한다.
여기 링크들을 만드는 예제가 있다.:

```jsx
const pids = ['id1', 'id2', 'id3'];
{pids.map(pid => (
  <Link href="/post/[pid]" as={`/post/${pid}`}>
    <a>Post {pid}</a>
  </Link>
))}
```

> [더 많은 `<Link>` 예제는 여기서 확인한다 ](#with-link).

그러나, 쿼리와 루트의 파라메터 이름이 같을 경우, 루트의 파라메터들은 매칭되는 쿼리의 파라메터들로 대체될 것이다.
예를 들면, `/post/abc?pid=bcd` 는 다음과 같은 `쿼리` 오브젝트를 가지게 된다. : `{ pid: 'abc' }`.

> **Note**: 이미 선언이 되어있는 정적 루트들의 경우 동적 루트들 보다 우선 순위를 가지게 된다. 
>예를 들어, 당신이 `pages/post/[pid].js` 와 `pages/post/create.js` 라는 루트를 가지고 있다고 했을 때, `/post/create` (`[pid]`)와 매치되는 대신 `pages/post/create.js` 와 매치 될 것이다. 

> **Note**: [automatic prerendering](#automatic-prerendering) 에 의해 정적으로 옵티마이징된 페이지들의 경우 제공되는 루트의 파라메터들에 관계 없이 루팅 될 것이다.(`쿼리` 는 공백이 된다. , i.e. `{}`).
> 하이드레이션 작업 이후, Next.js 는 `query` 오브젝트 안에 있는 루트 파라메터를 이용하여 당신의 어플리케이션을 업데이트 시킨다.
> 만약 당신의 어플리케이션이 이 작업을 견뎌내지 못한다면, you can opt-out of static optimization by capturing the query parameter in `getInitialProps`.

### Populating `<head>`

<details>
<summary><b>Examples</b></summary>
<ul>
 <li><a href="/examples/head-elements">Head elements</a></li>
 <li><a href="/examples/layout-component">Layout component</a></li>
</ul>
</details>

페이지의 `<head>`에 위치에 자리할 컴포넌트를 만든다. 

```jsx
import Head from 'next/head'

function IndexPage() {
  return (
    <div>
      <Head>
        <title>My page title</title>
        <meta name="viewport" content="initial-scale=1.0, width=device-width" />
      </Head>
      <p>Hello world!</p>
    </div>
  )
}

export default IndexPage
```
`<head>` 안에 중복 되는 태그 작성을 피하기 위해 `key` 프로퍼티를 사용할 수 있다. 이 태그를 통해 렌더링을 한번만 시킨다는 걸 확신 시킨다 :

```jsx
import Head from 'next/head'

function IndexPage() {
  return (
    <div>
      <Head>
        <title>My page title</title>
        <meta
          name="viewport"
          content="initial-scale=1.0, width=device-width"
          key="viewport"
        />
      </Head>
      <Head>
        <meta
          name="viewport"
          content="initial-scale=1.2, width=device-width"
          key="viewport"
        />
      </Head>
      <p>Hello world!</p>
    </div>
  )
}

export default IndexPage
```

이 경우에 오직 두번째 `<meta name="viewport" />` 만이 렌더링 된다. 

_Note: `<head>` 의 컨텐츠는 컴포넌트를 언마운팅 할 경우 완전히 떨어져나가므로, 각 페이지가 `<head>` 에서 필요로 하는 것을 완벽하게 정의할 것을 명심하세요, 단순히 다른 페이지들에 있는 것들로만 짐작하지 말자 ._

_Note: `<title>` 과 `<meta>` 엘리멘트들은 `<Head>` 엘리멘트에 **직속** 자식으로 종속되어 있어야 한다, 또는 `<React.Fragment>` 의 최대 한 레벨로 감싸질 수 있다, 그렇지 않으면 메타태그는 클라이언트 사이드의 네비게이션에서 정확하게 짚어내지 못한다._

### 데이터 패칭과 컴포넌트의 라이프사이클 

<details>
  <summary><b>Examples</b></summary>
  <ul>
    <li><a href="/examples/data-fetch">Data fetch</a></li>
  </ul>
</details>

당신이 스테이트가 필요할 때, 라이프 사이클을 훅 하거나 **initial data population** you can export a [React.Component](https://reactjs.org/docs/react-component.html) 또는 스테이트가 필요없는 함수를 사용하고 [Hooks](https://reactjs.org/docs/hooks-intro.html).

스테이트가 필요 없는 함수 사용: 

```jsx
import fetch from 'isomorphic-unfetch'

function Page({ stars }) {
  return <div>Next stars: {stars}</div>
}

Page.getInitialProps = async ({ req }) => {
  const res = await fetch('https://api.github.com/repos/zeit/next.js')
  const json = await res.json()
  return { stars: json.stargazers_count }
}

export default Page
```

`React.Component` 사용:

```jsx
import React from 'react'

class HelloUA extends React.Component {
  static async getInitialProps({ req }) {
    const userAgent = req ? req.headers['user-agent'] : navigator.userAgent
    return { userAgent }
  }

  render() {
    return <div>Hello World {this.props.userAgent}</div>
  }
}

export default HelloUA
```

여기서 우리는 페이지가 로드 될 때 데이터를 로드하기 위해, 우리는 [`async`](https://zeit.co/blog/async-and-await) 라는 동적 메소드 `getInitialProps` 를 사용할 수 있다는 것을 확인 할 수 있다. 이것은 어떤 것이든 비동기적으로 패치하는데 It can asynchronously fetch anything that resolves to a JavaScript plain `Object`, which populates `props`.

`getInitialProps` 로부터 받아온 데이터는 서버가 렌더링 될 때 직렬화 되는데, `JSON.stringify`와 비슷한 기능을 한다. 명심할 것은 `getInitialProps` 오브젝트로부터 돌아온 리턴 값은 순수한 `Object` 여야하지, `Date`, `Map` 또는 `Set` 같은 것들은 사용해서는 안된다. 

페이지를 초기 로드하기 위해, `getInitialProps` 는 서버만을 작동시킬 것이다. `getInitialProps` 은 클라이언트가 `Link` 컴포넌트나 루팅 API들을 사용하여 다른 루트로 이동할 때만 실행된다.  

<br/>

> - `getInitialProps` 는 자식 컴포넌트들에서 사용 될 수 **없다**. 오직 `pages` 안에서만 가능하다.
> - 만약 당신이 `getInitialProps` 안에서 서버에서만 사용되는 모듈들을 사용 중이라면, [그들을 제대로 임포트 할 것]을 명심해야 한다.(https://arunoda.me/blog/ssr-and-server-only-modules), 그렇지 않는다면, 당신의 앱의 속도가 저하될 것이다. 

<br/>

`getInitialProps`는 다음의 프로퍼티들에서 컨텍스트 오브젝트들을 제공 받는다:

- `pathname` - path section of URL
- `query` - query string section of URL parsed as an object
- `asPath` - `String` of the actual path (including the query) shows in the browser
- `req` - HTTP request object (server only)
- `res` - HTTP response object (server only)
- `err` - Error object if any error is encountered during the rendering

### 라우팅(Routing)

Next.js는 어플리케이션 안에서 가능한 모든 루트들을 명확하게 알고 있지 않다, 그 말은 현재 페이지는 클라이언트 사이드의 다른 어떠한 다른 페이지들도 인식하지 못한다는 소리다. 모든 이 후에 발생할 루트들은 후생성(lazy-loaded) 된다, 망할 확장성을 위해서 말이다.

#### `<Link>` 를 함께 사용

<details>
  <summary><b>Examples</b></summary>
  <ul>
    <li><a href="/examples/hello-world">Hello World</a></li>
  </ul>
</details>

클라이언트-사이드 단에서의 루트를 이용한 이동은 `<Link>` 컴포넌트를 통해 가능하다.

> 이 컴포넌트는 리프레쉬가 필요한 정적인 페이지로의 이동에서는 요구되지 않는다. [AMP](#amp-support) 를  사용하는 것과 같이 말이다.

**Basic Example**

아래 두 페이지를 보면:

```jsx
// pages/index.js
import Link from 'next/link'

function Home() {
  return (
    <>
      <ul>
        <li>Home</li>
        <li>
          <Link href="/about">
            <a>About Us</a>
          </Link>
        </li>
      </ul>

      <h1>This is our homepage.</h1>
    </>
  )
}

export default Home
```

```jsx
// pages/about.js
function About() {
  return (
    <>
      <ul>
        <li>
          <Link href="/">
            <a>Home</a>
          </Link>
        </li>
        <li>About Us</li>
      </ul>

      <h1>About</h1>
      <p>We are a cool company.</p>
    </>
  )
}

export default About
```

**Custom routes (URL의 props를 사용)**

만약 당신이 [Dynamic Routing](#dynamic-routing)가 커버하지 못하는 케이스를 찾았다면, custom server 를 만들고 수동으로 동적 루트들을 추가 할 수 있다.

Example:

1. 당신이 `/post/:slug` 라는 URL 을 가지고 있다고 가정해보자.

2. `pages/post.js` 를 만든다:

   ```jsx
   import { useRouter } from 'next/router'

   const Post = () => {
     const router = useRouter()
     const { slug } = router.query

     return <p>My Blog Post: {slug}</p>
   }

   export default Post
   ```

3. `server.js` 에 `express` (다른 어떤 서버도 상관없다) 로 루트를 추가한다(이 부분은 SSR만을 위한 것이다). 이 것은 `/post/:slug` 를 `pages/post.js` 로 url을 루팅 시키면서 `slug` 부분을 `query` 오브젝트 처럼 페이지에 제공할 것이다.

   ```jsx
   server.get('/post/:slug', (req, res) => {
     return app.render(req, res, '/post', { slug: req.params.slug })
   })
   ```

4. 클라이언트 단의 루팅을 위해서는 `next/link` 를 사용한다:

   ```jsx
   <Link href="/post?slug=something" as="/post/something">
   ```

   - `href`: `pages` 디렉토리 안의 경로
   - `as`: 서버 단의 루트로 사용될 주소

클라이언트 단의 루팅은 정확히 브라우저처럼 작동한다:

1. The component is fetched.
2. `getInitialProps`가 정의 될 경우, 데이터가 패치된다. 에러가 발생할 경우, `_error.js` 가 렌더링 된다.
3. 1,2 번이 완료 된 후, `pushState` 가 작동하고 새로운 컴포넌트가 렌더링 된다.

당신의 컴포넌트에 `pathname`, `query`, `asPath` 를 심어주기 위해, [useRouter](#useRouter)hook 을 사용할 수 있다, 또는 클래스 컴포넌트를 위해 [withRouter](#using-a-higher-order-component) 를 사용.

##### URL 오브젝트를 사용

<details>
  <summary><b>Examples</b></summary>
  <ul>
    <li><a href="/examples/with-url-object-routing">With URL Object Routing</a></li>
  </ul>
</details>

`<Link>` 컴포넌트는 URL 오브젝트 또한 받을 수 있고 이는 자동적으로 URL string 으로 포맷팅 된다.

```jsx
// pages/index.js
import Link from 'next/link'

function Home() {
  return (
    <div>
      Click{' '}
      <Link href={{ pathname: '/about', query: { name: 'Zeit' } }}>
        <a>here</a>
      </Link>{' '}
      to read more
    </div>
  )
}

export default Home
```

이 것은 `/about?name=Zeit` 라는 URL string을 발생 시키게 될 것이고, [Node.js URL module documentation](https://nodejs.org/api/url.html#url_url_strings_and_url_objects)에 정의된 모든 프로퍼티를 이용할 수 있다.

##### 새로운 url 을 push 하지 않고 replace 

`<Link>` 컴포넌트의 기본 기능은 새 url을 스택에 `push` 하는 것이다. 당신은 prop 에 `replace` 를 사용하여 스택에 새로 추가 되는 것을 방지 할 수 있다.

```jsx
// pages/index.js
import Link from 'next/link'

function Home() {
  return (
    <div>
      Click{' '}
      <Link href="/about" replace>
        <a>here</a>
      </Link>{' '}
      to read more
    </div>
  )
}

export default Home
```

##### `onClick`을 지원하는 컴포넌트 사용하기

`<Link>` 는 `onClick` 를 서포트 하는 모든 컴포넌트를 지원한다. `<a>` 태그를 사용하지 않는 경우에는, `onClick` 이벤트 핸들러를 추가할 뿐, `href` 프로퍼티로 이동하지 않는다.

```jsx
// pages/index.js
import Link from 'next/link'

function Home() {
  return (
    <div>
      Click{' '}
      <Link href="/about">
        <img src="/static/image.png" alt="image" />
      </Link>
    </div>
  )
}

export default Home
```

##### Link 가 `href` 를 자식 컴포넌트에 노출 시키도록 강요하기

만약 자식 컴포넌트가 `<a>` 태그이고 herf 어트리뷰트를 가지고 있지 않다면 유저에 의해 반복적인 작업을 할 필요가 없다는 걸 명시 한다. 그러나, 가끔은, `<a>` 태그 안의 통과 하고 싶을 수 있는데 `Link`는 그것을 _hyperlink_ 로 인지하지 못하고, 그 결과로 인해 자식 컴포넌트의 `href`로 전이 되지 못한다. 이런 경우에는,  불리언 프로퍼티인 `passHref` 를 선언하고, 자식 컴포넌트의 `href` 프로퍼티로 노출되도록 강요할 수 있다.

**Please note**: `a` 이 외의 다른 태그를 사용하고 `passHref` 를 패스하는 것에 실패한다면 링크로써의 이동은 정확하게 작동할 수도 있지만, 서치 엔진이 크롤링 할 경우에는 링크로 인식되지 못한다. (`href` 애트리뷰트의 부재로 인해). 이 것은 당신의 사이트들의 SEO 에 영향을 끼칠 수 있다.

```jsx
import Link from 'next/link'
import Unexpected_A from 'third-library'

function NavLink({ href, name }) {
  return (
    <Link href={href} passHref>
      <Unexpected_A>{name}</Unexpected_A>
    </Link>
  )
}

export default NavLink
```

##### 페이지의 top 으로 스크롤 이동 막기

`<Link>`의 기본 기능은 페이지의 상단으로 스크롤을 이동하는 것이다. 만약 그 곳에 해쉬가 정의되어 있어서 특정한 id로 스크롤 한다면, 평범한 `<a>` 태그와 다를 바 없다. top / hash 로 이동하는 것을 막고자 한다면 `<Link>` 에 `scroll={false}`를 추가할 수 있다:

```jsx
<Link scroll={false} href="/?counter=10"><a>Disables scrolling</a></Link>
<Link href="/?counter=10"><a>Changes with scrolling to top</a></Link>
```

#### Imperatively

<details>
  <summary><b>Examples</b></summary>
  <ul>
    <li><a href="/examples/using-router">Basic routing</a></li>
    <li><a href="/examples/with-loading">With a page loading indicator</a></li>
  </ul>
</details>

당신은 `next/router`를 사용하여 클라이언트 단의 페이지를 이행할 수 있다.

```jsx
import Router from 'next/router'

function ReadMore() {
  return (
    <div>
      Click <span onClick={() => Router.push('/about')}>here</span> to read more
    </div>
  )
}

export default ReadMore
```

#### Intercepting `popstate`

어떠한 경우에는 (예를 들어 [custom router](#custom-server-and-routing) 를 사용한다고 가정했을 때), 라우터가 작동하기 전에 [`popstate`](https://developer.mozilla.org/en-US/docs/Web/Events/popstate)를 먼저 적용하고 싶을 수 있다.
예를 들어, 리퀘스트를 조종하는데 사용 할 수도 있고, SSR 의 갱신을 강요할 수 있다.

```jsx
import Router from 'next/router'

Router.beforePopState(({ url, as, options }) => {
  // I only want to allow these two routes!
  if (as !== '/' || as !== '/other') {
    // Have SSR render bad routes as a 404.
    window.location.href = as
    return false
  }

  return true
})
```

만약 `beforePopState`를 통과하는 함수가 `false` 를 리턴할 경우, `Router` 는 `popstate` 를 처리하지 않는다;
이런 경우에는 이 것을 처리하는 것은 당신에게 책임이 있다. 
다음을 보자. [Disabling File-System Routing](#disabling-file-system-routing).

`Router` 보다 상위의 오브젝트는 다음과 같은 API 와 함께 한다:

- `route` - `String` of the current route
- `pathname` - `String` of the current path excluding the query string
- `query` - `Object` with the parsed query string. Defaults to `{}`.
- `asPath` - `String` of the actual path (including the query) shows in the browser
- `push(url, as=url)` - performs a `pushState` call with the given url
- `replace(url, as=url)` - performs a `replaceState` call with the given url
- `beforePopState(cb=function)` - intercept popstate before router processes the event

두번째의 `push`를 위한 `as` 파라메터는 URL 을 위한 추가적인  _장식_ 이다. 당신이 서버에 커스텀 된 루트를 사용할 경우 유용하다.

##### URL object 사용

당신은 `<Link>` 컴포넌트를 URL 오브젝트가 `push` 하고 `replace` 하는 것과 같이 사용할 수 있다.

```jsx
import Router from 'next/router'

const handler = () => {
  Router.push({
    pathname: '/about',
    query: { name: 'Zeit' },
  })
}

function ReadMore() {
  return (
    <div>
      Click <span onClick={handler}>here</span> to read more
    </div>
  )
}

export default ReadMore
```

이 것은 [`<Link>` component 안의](#with-url-object) 정확히 같은 파라메터들을 사용한다. 첫 번째 파라메터 맵
This uses the same exact parameters as [in the `<Link>` component](#with-url-object). The first parameter maps to `href` while the second parameter maps to `as` in the `<Link>` component as documented [here](#with-url-object).

##### Router Events

당신은 또한 라우터 안에서 일어나는 많은 이벤트들을 볼 수 있다.
여기에 지원되고 있는 이벤트의 목록이 있다:

- `routeChangeStart(url)` - 루트가 변경되기 시작할 때 작동
- `routeChangeComplete(url)` - 루트의 변경이 완전히 끝났을 때 작동
- `routeChangeError(err, url)` - 루트 변경 중 에러가 발생 했을 경우 작동, 또는 루트 변경이 취소 됐을 경우.
- `beforeHistoryChange(url)` - 브라우저의 기록을 변경 하기 직전에 작동 
- `hashChangeStart(url)` - 페이지가 아닌 해쉬가 변경 될 때 작동 
- `hashChangeComplete(url)` - 페이지가 아닌 해쉬가 완전히 변경 됐을 때 작동 

> 여기의 `url` 이 브라우저에서 URL 로 표시 되는 값이다. 만약 당신이 `Router.push(url, as)` 를 호출할 경우 ( 또는 비슷한 어떤 것이라도 ), `url`의 값은 `as` 가 된다.

여기 라우터 이벤트 `routeChangeStart` 를 올바르게 쓰는 방법이 있다:

```js
const handleRouteChange = url => {
  console.log('App is changing to: ', url)
}

Router.events.on('routeChangeStart', handleRouteChange)
```

만약 당신이 저 이벤트를 쓰고 싶지 않다면, `off` 메소드를 사용해서 해지 할 수 있다:

```js
Router.events.off('routeChangeStart', handleRouteChange)
```

만약 루트 로딩이 취소 됐다면 ( 예를 들어 두 링크를 한번에 클릭하는게 성공했다고 치고 ), `routeChangeError` 가 작동할 것이다. 패스된 `err` 는 `true` 값을 가진 `cancelled` 프로퍼티를 포함하고 있게 된다.

```js
Router.events.on('routeChangeError', (err, url) => {
  if (err.cancelled) {
    console.log(`Route to ${url} was cancelled!`)
  }
})
```

> **Note**: `getInitialProps` 안에서 라우터 이벤트를 사용하는 것은 권장되지 않는다. 예측 불가한 결과를 초래할 수 있기 때문.<br/>
> 라우터 이벤트는 컴포넌트가 마운트 될 때나 (`useEffect` 또는 `componentDidMount`/`componentWillUnmount`) 또는 무조건 이벤트가 실행 되야 할 경우 선언되는 것을 권장한다.
>
> ```js
> useEffect(() => {
>   const handleRouteChange = url => {
>     console.log('App is changing to: ', url)
>   }
>
>   Router.events.on('routeChangeStart', handleRouteChange)
>   return () => {
>     Router.events.off('routeChangeStart', handleRouteChange)
>   }
> }, [])
> ```

##### Shallow Routing

<details>
  <summary><b>Examples</b></summary>
  <ul>
    <li><a href="/examples/with-shallow-routing">Shallow Routing</a></li>
  </ul>
</details>

얕은 라우팅(Shallow routing)은 `getInitialProps` 없이 URL을 변경할 수 있도록 허용해준다. 당신은 업데이트 된 `pathname` 과 `query` 를 `router` prop (injected by using [`useRouter`](#useRouter) or [`withRouter`](#using-a-higher-order-component))를 통해서 받게 된다, 스테이트는 잃지 않는다.

당신은 이 것을 `shallow: true` 옵션을 가진 `Router.push` 또는 `Router.replace` 를 호출함으로써 행할 수 있다. 여기에 예제를 보자:  

```js
// Current URL is "/"
const href = '/?counter=10'
const as = href
Router.push(href, as, { shallow: true })
```

이제, URL 은 `/?counter=10` 로 업데이트 되었다. 당신은 `Component` (`router` prop 를 적용하기 위해 `Component` 바깥으로 [`withRouter`](#using-a-higher-order-component)를 꼭 사용할 것을 명심) 안에 `this.props.router.query` 를 통해서 업데이트 된 URL을 볼 수 있다.

당신은 [`componentDidUpdate`](https://reactjs.org/docs/react-component.html#componentdidupdate)를 통해서 URL 의 변경을 아래와 같이 확인 할 수 있다: 

```js
componentDidUpdate(prevProps) {
  const { pathname, query } = this.props.router
  // verify props have changed to avoid an infinite loop
  if (query.id !== prevProps.router.query.id) {
    // fetch data based on the new query
  }
}
```

> NOTES:
>
> Shallow routing 은 **오직** 같은 페이지의 URL 변경만 가능하다. 예를 들어서, 우리가 `about` 이라는 다른 페이지를 불러야 한다고 가정했을 때, 아래와 같이 실행해야 한다:
>
> ```js
> Router.push('/?counter=10', '/about?counter=10', { shallow: true })
> ```
>
> 새로운 페이지가 되었기 때문에, 현재의 페이지는 언로드 될 것이고, 새로운 페이지가 로드 되면서 shallow routing 을 셋팅했음에도 불구하고  `getInitialProps` 를 호출하게 될 것이다. .

#### useRouter

<details>
  <summary><b>Examples</b></summary>
  <ul>
    <li><a href="/examples/dynamic-routing">Dynamic routing</a></li>
  </ul>
</details>

만약 당신이 당신의 앱 안에 어떠한 아무 컴포넌트에서 `router` 오브젝트를 접근하길 원한다면, `useRouter`기능을 사용할 수 있다. 바로 이렇게:

```jsx
import { useRouter } from 'next/router'

export default function ActiveLink({ children, href }) {
  const router = useRouter()
  const style = {
    marginRight: 10,
    color: router.pathname === href ? 'red' : 'black',
  }

  const handleClick = e => {
    e.preventDefault()
    router.push(href)
  }

  return (
    <a href={href} onClick={handleClick} style={style}>
      {children}
    </a>
  )
}
```

위의 `router` 오브젝트는 [`next/router`](#imperatively) 와 비슷한 API 기능을 가지고 있다. 

#### 더 상위 순위를 가진 컴포넌트 사용하기  

<details>
  <summary><b>Examples</b></summary>
  <ul>
    <li><a href="/examples/using-with-router">Using the `withRouter` utility</a></li>
  </ul>
</details>

만약 [useRouter](#useRouter) 가 당신이 정확하게 원하는게 아닌 것 같다면, `withRouter` 또한 어떠한 컴포넌트에서라도 `router` 라는 이름으로 사용할 수 있다, 여기 예제를 보자:

```jsx
import { withRouter } from 'next/router'

function Page({ router }) {
  return <p>{router.pathname}</p>
}

export default withRouter(Page)
```

### Prefetching Pages

⚠️ This is a production only feature ⚠️

<details>
  <summary><b>Examples</b></summary>
  <ul>
    <li><a href="/examples/with-prefetching">Prefetching</a></li>
  </ul>
</details>

Next.js 는 페이지를 프리패칭 할 수 있는 API 를 제공한다.

Next.js 가 당신의 페이지를 서버 사이드 렌더링 server-renders your pages, 당신의 앱의 앞으로 있을 상호작용들을 인스턴트화 시키는 것을 허용한다. Next.js 는 Effectively Next.js gives you the great initial download performance of a _website_, with the ahead-of-time download capabilities of an _app_. [Read more](https://zeit.co/blog/next#anticipation-is-the-key-to-performance).

> Next.js 로 프리패칭을 할 경우 JS 코드만을 다운로드한다. 페이지가 렌더링 될 때 데이터를 위해 잠시 기다려야 할 수도 있다.

> `<link rel="preload">` 가 프리패칭을 위해 사용되는 코드이다. 가끔씩은 브라우저들이 리소스가 3초 안에 사용되지 않으면 경고를 보낼 수도 있는데, 그런 경고들은 이 것을 사용하여 무시할 수 있다. https://github.com/zeit/next.js/issues/6517#issuecomment-469063892.

#### With `<Link>`

`<Link>` 는 뷰에 보여지는 즉시 페이지를 자동적으로 백그라운드에서 프리패칭 시킨다. 만약 잘 접속되지 않는 특정한 페이지들이 있다면 수동으로  `prefetch` 를 `false` 로 셋팅 할 수 있다, 이렇게 말이다:

```jsx
<Link href="/about" prefetch={false}>
  <a>About</a>
</Link>
```

#### Imperatively

대부분의 프리패칭은 `<Link />`를 사용하여 선언이 필요하다. 하지만 우리는 더 고급된 방법으로 명령형(imperative) API를 쓸 수 있다:

```jsx
import { useRouter } from 'next/router'

export default function MyLink() {
  const router = useRouter()

  return (
    <>
      <a onClick={() => setTimeout(() => router.push('/dynamic'), 100)}>
        A route transition will happen after 100ms
      </a>
      {// and we can prefetch it!
      router.prefetch('/dynamic')}
    </>
  )
}
```

`router` 메소드들은 오직 당신의 앱에 클라이언트 단에서만 사용되야 하긴 하지만.  `useEffect()` 안에 명령적인 `prefetch` 를 사용해서 이 주제에 관한 어떠한 에러라도 예방 할 수 있다:

```jsx
import { useRouter } from 'next/router'

export default function MyLink() {
  const router = useRouter()

  useEffect(() => {
    router.prefetch('/dynamic')
  })

  return (
    <a onClick={() => setTimeout(() => router.push('/dynamic'), 100)}>
      A route transition will happen after 100ms
    </a>
  )
}

export default withRouter(MyLink)
```

당신은 또한 `React.Component` 를 사용할 때 `componentDidMount()` 의 라이프 사이클 메소드에 이 것을 추가할 수 있다:

```jsx
import React from 'react'
import { withRouter } from 'next/router'

class MyLink extends React.Component {
  componentDidMount() {
    const { router } = this.props
    router.prefetch('/dynamic')
  }

  render() {
    const { router } = this.props

    return (
      <a onClick={() => setTimeout(() => router.push('/dynamic'), 100)}>
        A route transition will happen after 100ms
      </a>
    )
  }
}

export default withRouter(MyLink)
```

### API Routes

<details>
  <summary><b>Examples</b></summary>
  <ul>
    <li><a href="/examples/api-routes">Basic API routes</a></li>
    <li><a href="/examples/api-routes-micro">API routes with micro</a></li>
    <li><a href="/examples/api-routes-middleware">API routes with middleware</a></li>
    <li><a href="/examples/api-routes-graphql">API routes with GraphQL server</a></li>
    <li><a href="/examples/api-routes-rest">API routes with REST</a></li>    
  </ul>
</details>

API 라우터들은 당신이 Next.js 를 사용하여 **API**를 만들 수 있도록 직관적인 해결법들을 제공한다. 
`./pages/` 폴더 안에 `api/` 를 만드는 것부터 시작한다.

`./pages/api` 안의 모든 파일은 `/api/*` 로 맵핑된다.
예를 들어서, `./pages/api/posts.js` 는 `/api/posts` 라는 라우터로 맵핑된다.

여기 API 루트 예제 파일이 있다:

```js
export default (req, res) => {
  res.setHeader('Content-Type', 'application/json')
  res.statusCode = 200
  res.end(JSON.stringify({ name: 'Nextjs' }))
}
```

- `req` refers to [NextApiRequest](https://github.com/zeit/next.js/blob/v9.0.0/packages/next-server/lib/utils.ts#L143-L158) which extends [http.IncomingMessage](https://nodejs.org/api/http.html#http_class_http_incomingmessage)

- `res` refers to [NextApiResponse](https://github.com/zeit/next.js/blob/v9.0.0/packages/next-server/lib/utils.ts#L168-L178) which extends [http.ServerResponse](https://nodejs.org/api/http.html#http_class_http_serverresponse)

For [API routes](#api-routes) there are build in types `NextApiRequest` and `NextApiResponse`, which extend the `Node.js` request and response objects.

```ts
import { NextApiRequest, NextApiResponse } from 'next'

export default (req: NextApiRequest, res: NextApiResponse) => {
  res.status(200).json({ title: 'Next.js' })
}
```

API 호출을 위한 다른 HTTP 메소드를 다루기 위해서는 리졸버(resolver) 함수의 `req.method` 에 접근할 수 있다: 

```js
export default (req, res) => {
  if (req.method === 'POST') {
    // Process your POST request
  } else {
    // Handle the rest of your HTTP methods
  }
}
```

> **Note**: API Routes [do not specify CORS headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS), so they'll be **same-origin only** by default.
> CORS 미들웨어를 사용하여 You can customize this behavior by wrapping your export with CORS middleware.
> 아래의 [예제](#api-middlewares)를 통해 확인할 수 있다.

API 루트들은 당신의 클라이언트단 번들 크기를 증가시키지 않는다. 그 것들은 서버단에서만 속한 번들들이다.

#### 동적 루트 지원 

API 페이지들은 [동적 라우팅](#dynamic-routing)을 지원한다, 그러므로 당신은 위에 언급된 모든 기능들을 사용할 수 있다.
`./pages/api/post/[pid].js` 라는 페이지를 봤을 때, 리졸버 메소드 안에서 파라메터를 얻는 방법은 다음과 같다: 

```js
export default (req, res) => {
  const {
    query: { pid },
  } = req

  res.end(`Post: ${pid}`)
}
```

#### API 미들웨어

API 루트는 `req` 로 분석되는 미들웨어 provides built in middlewares which parse the incoming `req`.
다음과 같은 미들웨어들이다:

- `req.cookies` - 쿠키를 포함하고 있는 오브젝트 an object containing the cookies sent by the request. 기본값은 `{}`
- `req.query` - [쿼리 스트링](https://en.wikipedia.org/wiki/Query_string)을 포함하고 있는 오브젝트. 기본값은 `{}`
- `req.body` - an object containing the body parsed by `content-type`, 또는 바디가 없을 경우에 `null` 

바디 파싱은 `1mb` 로 기본적으로는 제한된 크기 내에서 가능하다.

Body parsing is enabled by default with a size limit of `1mb` for the parsed body.
You can opt-out of automatic body parsing if you need to consume it as a `Stream`:

```js
// ./pages/api/my-endpoint.js
export default (req, res) => {
  // ...
}

export const config = {
  api: {
    bodyParser: false,
  },
}
```

`bodyParser` 안에 `sizeLimit` 를 추가하면 파스된 바디의 크기를 조절할 수 있다. 단위는 [bytes](https://github.com/visionmedia/bytes.js) 라이브러리로 지원된다. 

```js
// ./pages/api/my-endpoint.js
export default (req, res) => {
  // ...
}

export const config = {
  api: {
    bodyParser: {
      sizeLimit: '1mb',
    }
  },
}
```

추가적으로,  [middleware](https://github.com/amio/awesome-micro) 와 호환되는 [Micro](https://github.com/zeit/micro) 는 모두 다 사용할 수 있다!

예를 들면, API 의 엔드포인트는  [configuring CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) for your API endpoint can be done leveraging `micro-cors`.

첫번째로, `micro-cors` 설치하자:

```bash
npm i micro-cors
# or
yarn add micro-cors
```

그리고 `micro-cors` 임포트 시킨 뒤 [config에 추가한다](https://github.com/possibilities/micro-cors#readme). 마지막으로, 미들웨어 안에서 export 시킬 함수를 감싼다:

```js
import Cors from 'micro-cors'

const cors = Cors({
  allowedMethods: ['GET', 'HEAD'],
})

function Endpoint(req, res) {
  res.json({ message: 'Hello Everyone!' })
}

export default cors(Endpoint)
```

#### 헬퍼 함수 

우리는 개발자의 경험과 API 엔드포인트 생성의 속도를 향상 시키기 위해 Express.js 같은 메소드들을 제공한다:

```js
export default (req, res) => {
  res.status(200).json({ name: 'Next.js' })
}
```

- `res.status(code)` - status 코드를 셋팅 할 수 있는 함수. `code` must be a valid [HTTP status code](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)
- `res.json(json)` - `JSON` 응답을 보낸다. `json` 은 `JSON` 오브젝트 형태여야 한다.
- `res.send(body)` - HTTP 응답을 보낸다. `body`는 `string`, `object` 또는 `Buffer` 형이 될 수 있다.

### Custom server and routing

<details>
  <summary><b>Examples</b></summary>
  <ul>
    <li><a href="/examples/custom-server">Basic custom server</a></li>
    <li><a href="/examples/custom-server-express">Express integration</a></li>
    <li><a href="/examples/custom-server-hapi">Hapi integration</a></li>
    <li><a href="/examples/custom-server-koa">Koa integration</a></li>
    <li><a href="/examples/parameterized-routing">Parameterized routing</a></li>
    <li><a href="/examples/ssr-caching">SSR caching</a></li>
  </ul>
</details>

보통 당신은 next 서버를 `next start` 사용해서 시작할 것이다. 그래도 되지만 커스터마이징된 루트를 가진 서버를 100% 프로그래밍 된대로 시작하려면 루트 패턴이나 다른 걸 사용한다.

서버 파일들로 된 커스터마이징된 서버를 사용할 때는, 예를 들어서 `server.js` 같은 것들,  `package.json` 안에 스크립트 키를 업데이트 해야 한다는 것을 명심하자:

```json
{
  "scripts": {
    "dev": "node server.js",
    "build": "next build",
    "start": "NODE_ENV=production node server.js"
  }
}
```

이 예제는 `/a` 를 `./pages/b` 로 변환시키고 , `/b` 를 `./pages/a` 로 변환 시킨다:

```js
// This file doesn't go through babel or webpack transformation.
// Make sure the syntax and sources this file requires are compatible with the current node version you are running
// See https://github.com/zeit/next.js/issues/1245 for discussions on Universal Webpack or universal Babel
const { createServer } = require('http')
const { parse } = require('url')
const next = require('next')

const dev = process.env.NODE_ENV !== 'production'
const app = next({ dev })
const handle = app.getRequestHandler()

app.prepare().then(() => {
  createServer((req, res) => {
    // Be sure to pass `true` as the second argument to `url.parse`.
    // This tells it to parse the query portion of the URL.
    const parsedUrl = parse(req.url, true)
    const { pathname, query } = parsedUrl

    if (pathname === '/a') {
      app.render(req, res, '/b', query)
    } else if (pathname === '/b') {
      app.render(req, res, '/a', query)
    } else {
      handle(req, res, parsedUrl)
    }
  }).listen(3000, err => {
    if (err) throw err
    console.log('> Ready on http://localhost:3000')
  })
})
```

`next` API 는 다음과 같다:

- `next(opts: object)`

제공되는 옵션들:

- `dev` (`bool`) whether to launch Next.js in dev mode - default `false`
- `dir` (`string`) where the Next project is located - default `'.'`
- `quiet` (`bool`) Hide error messages containing server information - default `false`
- `conf` (`object`) the same object you would use in `next.config.js` - default `{}`

그리고, `start` 스크립트를 `NODE_ENV=production node server.js` 로 변경한다.

#### Disabling file-system routing

기본 값으로는 `Next` 는 `/pages` 안에 있는 각각의 파일들을 파일 네임에 매칭 시켜 경로를 지정시켜 준다. (eg, `/pages/some-file.js` 는 `site.com/some-file` 로 연결된다.

만약 당신의 프로젝트가 커스터마이징 된 라우팅을 사용한다면, 이 행위는 하나의 컨텐츠를 여러 개의 경로로 연결시킬 수도 있기 때문에 SEO 나 UX에 문제를 야기할 수 있다.
라우팅 베이스가 되는 `/pages` 안에서 이 행위를 막거나 방지하려면 단순히 `next.config.js` 의 옵션을 변경해주면 된다:

```js
// next.config.js
module.exports = {
  useFileSystemPublicRoutes: false,
}
```

Note that `useFileSystemPublicRoutes` 는 간단하게 SSR 의 파일네임 루트들을 무력화 시킨다; 클라이언트 단의 라우팅은 여전히 저 경로들을 접근 할 수 있을 수도 있다. 만약에 이 옵션을 사용하게 된다면,  you should guard against navigation to routes you do not want programmatically.

당신은 클라이언트 단의 라우터 또한 설정을 변경하고자 할 수 있는데
You may also wish to configure the client-side Router to disallow client-side redirects to filename routes; please refer to [Intercepting `popstate`](#intercepting-popstate).

#### Dynamic assetPrefix

가끔씩은 `assetPrefix`를 동적으로 셋팅할 필요가 있다. 호출받는 리퀘스트의 `assetPrefix`를 바꿀 때 유용한 것들이 있다.
그 것을 위해서 우리는 `app.setAssetPrefix`를 사용한다.

사용하는 :

```js
const next = require('next')
const http = require('http')

const dev = process.env.NODE_ENV !== 'production'
const app = next({ dev })
const handleNextRequests = app.getRequestHandler()

app.prepare().then(() => {
  const server = new http.Server((req, res) => {
    // Add assetPrefix support based on the hostname
    if (req.headers.host === 'my-app.com') {
      app.setAssetPrefix('http://cdn.com/myapp')
    } else {
      app.setAssetPrefix('')
    }

    handleNextRequests(req, res)
  })

  server.listen(port, err => {
    if (err) {
      throw err
    }

    console.log(`> Ready on http://localhost:${port}`)
  })
})
```

### Dynamic Import

<details>
  <summary><b>Examples</b></summary>
  <ul>
    <li><a href="/examples/with-dynamic-import">With Dynamic Import</a></li>
  </ul>
</details>

Next.js 는 자바스크립트를 위해 ES2020 [dynamic `import()`](https://github.com/tc39/proposal-dynamic-import) 를 지원한다.
그것을 위해서 JavaScript 모듈을 (inc. React Components) 동적으로 임포트 시켜서 사용할 수 있다. 

당신은 동적 임포트를 당신의 코드를 알맞은 양으로 쪼개는 또 다른 방식이라고 생각해도 된다. 

Since Next.js 가 SSR 를 통한 동적 임포트를 제공하기 때문에 이런 멋진 것도 할 수 있다.
동적 임포트를 하는 몇가지 방법들을 보자.

#### Basic Usage (Also does SSR)

```jsx
import dynamic from 'next/dynamic'

const DynamicComponent = dynamic(() => import('../components/hello'))

function Home() {
  return (
    <div>
      <Header />
      <DynamicComponent />
      <p>HOME PAGE is here!</p>
    </div>
  )
}

export default Home
```

#### With named exports

```jsx
// components/hello.js
export function Hello() {
  return <p>Hello!</p>
}
```

```jsx
import dynamic from 'next/dynamic'

const DynamicComponent = dynamic(() =>
  import('../components/hello').then(mod => mod.Hello)
)

function Home() {
  return (
    <div>
      <Header />
      <DynamicComponent />
      <p>HOME PAGE is here!</p>
    </div>
  )
}

export default Home
```

#### With Custom Loading Component

```jsx
import dynamic from 'next/dynamic'

const DynamicComponentWithCustomLoading = dynamic(
  () => import('../components/hello2'),
  { loading: () => <p>...</p> }
)

function Home() {
  return (
    <div>
      <Header />
      <DynamicComponentWithCustomLoading />
      <p>HOME PAGE is here!</p>
    </div>
  )
}

export default Home
```

#### SSR 없이 하기

```jsx
import dynamic from 'next/dynamic'

const DynamicComponentWithNoSSR = dynamic(
  () => import('../components/hello3'),
  { ssr: false }
)

function Home() {
  return (
    <div>
      <Header />
      <DynamicComponentWithNoSSR />
      <p>HOME PAGE is here!</p>
    </div>
  )
}

export default Home
```

### Custom `<App>`

<details>
  <summary><b>Examples</b></summary>
  <ul>
    <li><a href="/examples/with-app-layout">Using `_app.js` for layout</a></li>
    <li><a href="/examples/with-componentdidcatch">Using `_app.js` to override `componentDidCatch`</a></li>
  </ul>
</details>

Next.js 는 `App` 컴포넌트를 페이지들을 초기화 시키는 데 사용한다. 페이지 초기화를 컨트롤 하기 위해 오버라이딩도 가능하다. 당신이 할 수 있는 멋진 것들은 다음과 같다:

- Persisting layout between page changes
- Keeping state when navigating pages
- Custom error handling using `componentDidCatch`
- Inject additional data into pages (for example by processing GraphQL queries)

오버라이드를 위해선, `./pages/_app.js` 파일을 생성하고 아래 보이는 것과 같이 App 클래스를 오버라이드 시킨다:

```js
import React from 'react'
import App, { Container } from 'next/app'

class MyApp extends App {
  // Only uncomment this method if you have blocking data requirements for
  // every single page in your application. This disables the ability to
  // perform automatic static optimization, causing every page in your app to
  // be server-side rendered.
  //
  // static async getInitialProps({ Component, ctx }) {
  //   let pageProps = {}
  //
  //   if (Component.getInitialProps) {
  //     pageProps = await Component.getInitialProps(ctx)
  //   }
  //
  //   return { pageProps }
  // }

  render() {
    const { Component, pageProps } = this.props

    return (
      <Container>
        <Component {...pageProps} />
      </Container>
    )
  }
}

export default MyApp
```

> **Note:** 커스터마이징된 `getInitialProps`을 App에 추가하면 [Automatic Prerendering](#automatic-prerendering) 에 영향을 끼치게 된다.

### Custom `<Document>`

<details>
  <summary><b>Examples</b></summary>
  <ul>
    <li><a href="/examples/with-styled-components">Styled components custom document</a></li>
  </ul>
</details>

커스터마이징된 `<Document>` 는 보통 당신의 어플리케이션의 `<html>` 와 `<body>` 를 증가 시켰다.
 Next.js 페이지가 도큐먼트의 마크업 주변에 선언들을 건너 뛰기 때문에 이 것은 필수적이다. 

이 것은 [styled-components](/examples/with-styled-components) or [emotion](/examples/with-emotion) 같은 CSS-in-JS 라이브러리들을 서버 사이드 렌더링 하는데 사용될 수 있도록 허용해준다. 
참고할 것은, Next.js 에 [styled-jsx](https://github.com/zeit/styled-jsx)는 기본값으로 셋팅 되어 있다는 것.

커스터마이징 된  `<Document>` 또한 `getInitialProps` 포함한다.  for expressing asynchronous server-rendering data requirements.

> **Note**: `<Document>`의 `getInitialProps` 함수는 클라이언트 단에서는 호출되지 않는다. 
> 페이지가 [automatically prerendered](#automatic-prerendering) 될 때도 마찬가지이다.

> **Note**: `getInitialProps`에 `ctx.req` / `ctx.res` 가 선언되어 있는지 꼭 확인하자.
>  These variables will be `undefined` when a page is being statically exported for `next export` or [automatic prerendering (static optimization)](#automatic-prerendering).

커스터마이징 된 `<Document>`를 사용하기 위해서는, `./pages/_document.js` 이라는 파일을 생성해야 하고 `Document` 클래스를 상속받아야 한다:

```jsx
// _document is only rendered on the server side and not on the client side
// Event handlers like onClick can't be added to this file

// ./pages/_document.js
import Document, { Html, Head, Main, NextScript } from 'next/document'

class MyDocument extends Document {
  static async getInitialProps(ctx) {
    const initialProps = await Document.getInitialProps(ctx)
    return { ...initialProps }
  }

  render() {
    return (
      <Html>
        <Head />
        <body>
          <Main />
          <NextScript />
        </body>
      </Html>
    )
  }
}

export default MyDocument
```

모든 `<Html>`, `<Head />`, `<Main />` 그리고 `<NextScript />` 는 정확한 페이지 렌더링을 위해 필수적이다.

**Note: `<Main />` 밖에 위치한 리액트-컴포넌트들은 브라우저에 의해 초기화 되지 않는다. 어플리케이션의 로직을 이 곳에 위치 시키지 _말 것_. 만약 당신이 모든 페이지들이 공유할 수 있는 컴포넌트가 필요하다면 (메뉴나 툴바 같은 것들), [`<App>`](#custom-app) 컴포넌트를 알아 보는 것이 좋다.**

`ctx` 오브젝트는 모든 [`getInitialProps`](#fetching-data-and-component-lifecycle) 훅에서 받는 것들과 동등한 위치를 가진다, 그리고 하나를 더 추가하자면: 

- `renderPage` (`Function`) a callback that executes the actual React rendering logic (synchronously). It's useful to decorate this function in order to support server-rendering wrappers like Aphrodite's [`renderStatic`](https://github.com/Khan/aphrodite#server-side-rendering).

#### Customizing `renderPage`

🚧 서버사이드 렌더링을 제대로 하기 위해서는 당신이 `renderPage`를 커스터마이징할 이유는 오직 어플리케이션에 적용할 css-in-js 라이브러리를 사용하기 위해서 뿐이라는 걸 명심해야 한다. 🚧

- 더 많은 커스터마이징을 위해서는 아규먼트들을 옵션으로 사용한다:

```js
import Document from 'next/document'

class MyDocument extends Document {
  static async getInitialProps(ctx) {
    const originalRenderPage = ctx.renderPage

    ctx.renderPage = () =>
      originalRenderPage({
        // useful for wrapping the whole react tree
        enhanceApp: App => App,
        // useful for wrapping in a per-page basis
        enhanceComponent: Component => Component,
      })

    // Run the parent `getInitialProps` using `ctx` that now includes our custom `renderPage`
    const initialProps = await Document.getInitialProps(ctx)

    return initialProps
  }
}

export default MyDocument
```

### Custom error handling

404 또는 500 에러는 디폴트 컴포넌트인  `error.js` 를 통해 서버와 클라이언트 양쪽에서 처리된다. 오버라이드를 하고 싶을 경우, `_error.js` 을 pages 폴더에 정의해라:

⚠️ `pages/_error.js` component is only used in production. In development you get an error with call stack to know where the error originated from. ⚠️

```jsx
import React from 'react'

class Error extends React.Component {
  static getInitialProps({ res, err }) {
    const statusCode = res ? res.statusCode : err ? err.statusCode : null
    return { statusCode }
  }

  render() {
    return (
      <p>
        {this.props.statusCode
          ? `An error ${this.props.statusCode} occurred on server`
          : 'An error occurred on client'}
      </p>
    )
  }
}

export default Error
```

### Reusing the built-in error page

If you want to render the built-in error page you can by using `next/error`:

```jsx
import React from 'react'
import Error from 'next/error'
import fetch from 'isomorphic-unfetch'

class Page extends React.Component {
  static async getInitialProps() {
    const res = await fetch('https://api.github.com/repos/zeit/next.js')
    const errorCode = res.statusCode > 200 ? res.statusCode : false
    const json = await res.json()

    return { errorCode, stars: json.stargazers_count }
  }

  render() {
    if (this.props.errorCode) {
      return <Error statusCode={this.props.errorCode} />
    }

    return <div>Next stars: {this.props.stars}</div>
  }
}

export default Page
```

> If you have created a custom error page you have to import your own `_error` component from `./_error` instead of `next/error`.

The Error component also takes `title` as a property if you want to pass in a text message along with a `statusCode`.

### Custom configuration

For custom advanced behavior of Next.js, you can create a `next.config.js` in the root of your project directory (next to `pages/` and `package.json`).

Note: `next.config.js` is a regular Node.js module, not a JSON file. It gets used by the Next server and build phases, and not included in the browser build.

```js
// next.config.js
module.exports = {
  /* config options here */
}
```

Or use a function:

```js
module.exports = (phase, { defaultConfig }) => {
  return {
    /* config options here */
  }
}
```

`phase` is the current context in which the configuration is loaded. You can see all phases here: [constants](/packages/next-server/lib/constants.ts)
Phases can be imported from `next/constants`:

```js
const { PHASE_DEVELOPMENT_SERVER } = require('next/constants')
module.exports = (phase, { defaultConfig }) => {
  if (phase === PHASE_DEVELOPMENT_SERVER) {
    return {
      /* development only config options here */
    }
  }

  return {
    /* config options for all phases except development here */
  }
}
```

#### Setting a custom build directory

You can specify a name to use for a custom build directory. For example, the following config will create a `build` folder instead of a `.next` folder. If no configuration is specified then next will create a `.next` folder.

```js
// next.config.js
module.exports = {
  distDir: 'build',
}
```

#### Disabling etag generation

You can disable etag generation for HTML pages depending on your cache strategy. If no configuration is specified then Next will generate etags for every page.

```js
// next.config.js
module.exports = {
  generateEtags: false,
}
```

#### Configuring the onDemandEntries

Next exposes some options that give you some control over how the server will dispose or keep in memories pages built:

```js
module.exports = {
  onDemandEntries: {
    // period (in ms) where the server will keep pages in the buffer
    maxInactiveAge: 25 * 1000,
    // number of pages that should be kept simultaneously without being disposed
    pagesBufferLength: 2,
  },
}
```

This is development-only feature. If you want to cache SSR pages in production, please see [SSR-caching](https://github.com/zeit/next.js/tree/canary/examples/ssr-caching) example.

#### Configuring extensions looked for when resolving pages in `pages`

Aimed at modules like [`@next/mdx`](https://github.com/zeit/next.js/tree/canary/packages/next-mdx), that add support for pages ending with `.mdx`. `pageExtensions` allows you to configure the extensions looked for in the `pages` directory when resolving pages.

```js
// next.config.js
module.exports = {
  pageExtensions: ['mdx', 'jsx', 'js'],
}
```

#### Configuring the build ID

Next.js uses a constant generated at build time to identify which version of your application is being served. This can cause problems in multi-server deployments when `next build` is ran on every server. In order to keep a static build id between builds you can provide the `generateBuildId` function:

```js
// next.config.js
module.exports = {
  generateBuildId: async () => {
    // For example get the latest git commit hash here
    return 'my-build-id'
  },
}
```

To fall back to the default of generating a unique id return `null` from the function:

```js
module.exports = {
  generateBuildId: async () => {
    // When process.env.YOUR_BUILD_ID is undefined we fall back to the default
    if (process.env.YOUR_BUILD_ID) {
      return process.env.YOUR_BUILD_ID
    }

    return null
  },
}
```

#### Configuring next process script

You can pass any node arguments to `next` CLI command.

```bash
NODE_OPTIONS="--throw-deprecation" next
NODE_OPTIONS="-r esm" next
NODE_OPTIONS="--inspect" next
```

### Customizing webpack config

<details>
  <summary><b>Examples</b></summary>
  <ul>
    <li><a href="/examples/with-webpack-bundle-analyzer">Custom webpack bundle analyzer</a></li>
  </ul>
</details>

Some commonly asked for features are available as modules:

- [@zeit/next-css](https://github.com/zeit/next-plugins/tree/master/packages/next-css)
- [@zeit/next-sass](https://github.com/zeit/next-plugins/tree/master/packages/next-sass)
- [@zeit/next-less](https://github.com/zeit/next-plugins/tree/master/packages/next-less)
- [@zeit/next-preact](https://github.com/zeit/next-plugins/tree/master/packages/next-preact)
- [@next/mdx](https://github.com/zeit/next.js/tree/canary/packages/next-mdx)

> **Warning:** The `webpack` function is executed twice, once for the server and once for the client. This allows you to distinguish between client and server configuration using the `isServer` property.

Multiple configurations can be combined together with function composition. For example:

```js
const withMDX = require('@next/mdx')
const withSass = require('@zeit/next-sass')

module.exports = withMDX(
  withSass({
    webpack(config, options) {
      // Further custom configuration here
      return config
    },
  })
)
```

In order to extend our usage of `webpack`, you can define a function that extends its config via `next.config.js`.

```js
// next.config.js is not transformed by Babel. So you can only use javascript features supported by your version of Node.js.

module.exports = {
  webpack: (config, { buildId, dev, isServer, defaultLoaders, webpack }) => {
    // Note: we provide webpack above so you should not `require` it
    // Perform customizations to webpack config
    // Important: return the modified config

    // Example using webpack option
    config.plugins.push(new webpack.IgnorePlugin(/\/__tests__\//))
    return config
  },
  webpackDevMiddleware: config => {
    // Perform customizations to webpack dev middleware config
    // Important: return the modified config
    return config
  },
}
```

The second argument to `webpack` is an object containing properties useful when customizing its configuration:

- `buildId` - `String` the build id used as a unique identifier between builds
- `dev` - `Boolean` shows if the compilation is done in development mode
- `isServer` - `Boolean` shows if the resulting configuration will be used for server side (`true`), or client side compilation (`false`)
- `defaultLoaders` - `Object` Holds loader objects Next.js uses internally, so that you can use them in custom configuration
  - `babel` - `Object` the `babel-loader` configuration for Next.js

Example usage of `defaultLoaders.babel`:

```js
// Example next.config.js for adding a loader that depends on babel-loader
// This source was taken from the @next/mdx plugin source:
// https://github.com/zeit/next.js/tree/canary/packages/next-mdx
module.exports = {
  webpack: (config, options) => {
    config.module.rules.push({
      test: /\.mdx/,
      use: [
        options.defaultLoaders.babel,
        {
          loader: '@mdx-js/loader',
          options: pluginOptions.options,
        },
      ],
    })

    return config
  },
}
```

### Customizing babel config

<details>
  <summary><b>Examples</b></summary>
  <ul>
    <li><a href="/examples/with-custom-babel-config">Custom babel configuration</a></li>
  </ul>
</details>

In order to extend our usage of `babel`, you can simply define a `.babelrc` file at the root of your app. This file is optional.

If found, we're going to consider it the _source of truth_, therefore it needs to define what next needs as well, which is the `next/babel` preset.

This is designed so that you are not surprised by modifications we could make to the babel configurations.

Here's an example `.babelrc` file:

```json
{
  "presets": ["next/babel"],
  "plugins": []
}
```

The `next/babel` preset includes everything needed to transpile React applications. This includes:

- preset-env
- preset-react
- preset-typescript
- plugin-proposal-class-properties
- plugin-proposal-object-rest-spread
- plugin-transform-runtime
- styled-jsx

These presets / plugins **should not** be added to your custom `.babelrc`. Instead, you can configure them on the `next/babel` preset:

```json
{
  "presets": [
    [
      "next/babel",
      {
        "preset-env": {},
        "transform-runtime": {},
        "styled-jsx": {},
        "class-properties": {}
      }
    ]
  ],
  "plugins": []
}
```

The `modules` option on `"preset-env"` should be kept to `false` otherwise webpack code splitting is disabled.

### Exposing configuration to the server / client side

There is a common need in applications to provide configuration values.

Next.js supports 2 ways of providing configuration:

- Build-time configuration
- Runtime configuration

#### Build-time configuration

The way build-time configuration works is by inlining the provided values into the Javascript bundle.

You can add the `env` key in `next.config.js`:

```js
// next.config.js
module.exports = {
  env: {
    customKey: 'value',
  },
}
```

This will allow you to use `process.env.customKey` in your code. For example:

```jsx
// pages/index.js
function Index() {
  return <h1>The value of customKey is: {process.env.customKey}</h1>
}

export default Index
```

> **Warning:** Note that it is not possible to destructure process.env variables due to the webpack `DefinePlugin` replacing process.env.XXXX inline at build time.

```js
// Will not work
const { CUSTOM_KEY, CUSTOM_SECRET } = process.env
AuthMethod({ key: CUSTOM_KEY, secret: CUSTOM_SECRET })

// Will work as replaced inline
AuthMethod({ key: process.env.CUSTOM_KEY, secret: process.env.CUSTOM_SECRET })
```

#### Runtime configuration

> **Warning:** Note that this option is not available when using `target: 'serverless'`

> **Warning:** Generally you want to use build-time configuration to provide your configuration.
> The reason for this is that runtime configuration adds rendering / initialization overhead and is **incompatible with [automatic prerendering](#automatic-prerendering)**.

The `next/config` module gives your app access to the `publicRuntimeConfig` and `serverRuntimeConfig` stored in your `next.config.js`.

Place any server-only runtime config under a `serverRuntimeConfig` property.

Anything accessible to both client and server-side code should be under `publicRuntimeConfig`.

> **Note**: A page that relies on `publicRuntimeConfig` **must** use `getInitialProps` to opt-out of [automatic prerendering](#automatic-prerendering).
> You can also de-optimize your entire application by creating a [Custom `<App>`](#custom-app) with `getInitialProps`.

```js
// next.config.js
module.exports = {
  serverRuntimeConfig: {
    // Will only be available on the server side
    mySecret: 'secret',
    secondSecret: process.env.SECOND_SECRET, // Pass through env variables
  },
  publicRuntimeConfig: {
    // Will be available on both server and client
    staticFolder: '/static',
  },
}
```

```js
// pages/index.js
import getConfig from 'next/config'
// Only holds serverRuntimeConfig and publicRuntimeConfig from next.config.js nothing else.
const { serverRuntimeConfig, publicRuntimeConfig } = getConfig()

console.log(serverRuntimeConfig.mySecret) // Will only be available on the server side
console.log(publicRuntimeConfig.staticFolder) // Will be available on both server and client

function MyImage() {
  return (
    <div>
      <img src={`${publicRuntimeConfig.staticFolder}/logo.png`} alt="logo" />
    </div>
  )
}

export default MyImage
```

### Starting the server on alternative hostname

To start the development server using a different default hostname you can use `--hostname hostname_here` or `-H hostname_here` option with next dev. This will start a TCP server listening for connections on the provided host.

### CDN support with Asset Prefix

To set up a CDN, you can set up the `assetPrefix` setting and configure your CDN's origin to resolve to the domain that Next.js is hosted on.

```js
const isProd = process.env.NODE_ENV === 'production'
module.exports = {
  // You may only need to add assetPrefix in the production.
  assetPrefix: isProd ? 'https://cdn.mydomain.com' : '',
}
```

Note: Next.js will automatically use that prefix in the scripts it loads, but this has no effect whatsoever on `/static`. If you want to serve those assets over the CDN, you'll have to introduce the prefix yourself. One way of introducing a prefix that works inside your components and varies by environment is documented [in this example](https://github.com/zeit/next.js/tree/master/examples/with-universal-configuration-build-time).

If your CDN is on a separate domain and you would like assets to be requested using a [CORS aware request](https://developer.mozilla.org/en-US/docs/Web/HTML/CORS_settings_attributes) you can set a config option for that.

```js
// next.config.js
module.exports = {
  crossOrigin: 'anonymous',
}
```

## Automatic Prerendering

Next.js automatically determines that a page is static (can be prerendered) if it has no blocking data requirements.
This determination is made by the absence of `getInitialProps` in the page.

If `getInitialProps` is present, Next.js will not prerender the page.
Instead, Next.js will use its default behavior and render the page on-demand, per-request (meaning Server-Side Rendering).

If `getInitialProps` is absent, Next.js will **statically optimize** your page automatically by prerendering it to static HTML.

This feature allows Next.js to emit hybrid applications that contain **both server-rendered and statically generated pages**.
This ensures Next.js always emits applications that are **fast by default**.

> **Note**: Statically generated pages are still reactive: Next.js will hydrate your application client-side to give it full interactivity.

This feature provides many benefits.
For example, optimized pages require no server-side computation and can be instantly streamed to the end-user from CDN locations.

The result is an _ultra fast_ loading experience for your users.

`next build` will emit `.html` files for statically optimized pages.
The result will be a file named `.next/server/static/${BUILD_ID}/about.html` instead of `.next/server/static/${BUILD_ID}/about.js`.
This behavior is similar for `target: 'serverless'`.

The built-in Next.js server (`next start`) and programmatic API (`app.getRequestHandler()`) both support this build output transparently.
There is no configuration or special handling required.

> **Note**: If you have a [custom `<App>`](#custom-app) with `getInitialProps` then this optimization will be disabled.

> **Note**: If you have a [custom `<Document>`](#custom-document) with `getInitialProps` be sure you check if `ctx.req` is defined before assuming the page is server-side rendered.
> `ctx.req` will be `undefined` for pages that are prerendered.

## Production deployment

To deploy, instead of running `next`, you want to build for production usage ahead of time. Therefore, building and starting are separate commands:

```bash
next build
next start
```

To deploy Next.js with [ZEIT Now](https://zeit.co/now) see the [ZEIT Guide for Deploying Next.js with Now](https://zeit.co/guides/deploying-nextjs-with-now/).

Next.js can be deployed to other hosting solutions too. Please have a look at the ['Deployment'](https://github.com/zeit/next.js/wiki/Deployment) section of the wiki.

Note: `NODE_ENV` is properly configured by the `next` subcommands, if absent, to maximize performance. if you’re using Next.js [programmatically](#custom-server-and-routing), it’s your responsibility to set `NODE_ENV=production` manually!

Note: we recommend putting `.next`, or your [custom dist folder](https://github.com/zeit/next.js#custom-configuration), in `.gitignore` or `.npmignore`. Otherwise, use `files` or `now.files` to opt-into a whitelist of files you want to deploy, excluding `.next` or your custom dist folder.

### Serverless deployment

<details>
  <summary><b>Examples</b></summary>
  <ul>
    <li><a href="https://github.com/zeit/now-examples/tree/master/nextjs">now.sh</a></li>
    <li><a href="https://github.com/TejasQ/anna-artemov.now.sh">anna-artemov.now.sh</a></li>
    <li>We encourage contributing more examples to this section</li>
  </ul>
</details>

Serverless deployment dramatically improves reliability and scalability by splitting your application into smaller parts (also called [**lambdas**](https://zeit.co/docs/v2/deployments/concepts/lambdas/)).
In the case of Next.js, each page in the `pages` directory becomes a serverless lambda.

There are [a number of benefits](https://zeit.co/blog/serverless-express-js-lambdas-with-now-2#benefits-of-serverless-express) to serverless.
The referenced link talks about some of them in the context of Express, but the principles apply universally:
serverless allows for distributed points of failure, infinite scalability, and is incredibly affordable with a "pay for what you use" model.

To enable **serverless mode** in Next.js, add the `serverless` build `target` in `next.config.js`:

```js
// next.config.js
module.exports = {
  target: 'serverless',
}
```

The `serverless` target will output a single lambda or [HTML file](#automatic-prerendering) per page.
This file is completely standalone and doesn't require any dependencies to run:

- `pages/index.js` => `.next/serverless/pages/index.js`
- `pages/about.js` => `.next/serverless/pages/about.js`
- `pages/blog.js` => `.next/serverless/pages/blog.html`

The signature of the Next.js Serverless function is similar to the Node.js HTTP server callback:

```ts
export function render(req: http.IncomingMessage, res: http.ServerResponse) => void
```

- [http.IncomingMessage](https://nodejs.org/api/http.html#http_class_http_incomingmessage)
- [http.ServerResponse](https://nodejs.org/api/http.html#http_class_http_serverresponse)
- `void` refers to the function not having a return value and is equivalent to JavaScript's `undefined`. Calling the function will finish the request.

The static HTML files are ready to be served as-is.
You can read more about this feature, including how to opt-out, in the [Automatic Prerendering section](#automatic-prerendering).

Using the serverless target, you can deploy Next.js to [ZEIT Now](https://zeit.co/now) with all of the benefits and added ease of control like for example; [custom routes](https://zeit.co/guides/custom-next-js-server-to-routes/) and caching headers. See the [ZEIT Guide for Deploying Next.js with Now](https://zeit.co/guides/deploying-nextjs-with-now/) for more information.

#### One Level Lower

Next.js provides low-level APIs for serverless deployments as hosting platforms have different function signatures. In general you will want to wrap the output of a Next.js serverless build with a compatibility layer.

For example if the platform supports the Node.js [`http.Server`](https://nodejs.org/api/http.html#http_class_http_server) class:

```js
const http = require('http')
const page = require('./.next/serverless/pages/about.js')
const server = new http.Server((req, res) => page.render(req, res))
server.listen(3000, () => console.log('Listening on http://localhost:3000'))
```

For specific platform examples see [the examples section above](#serverless-deployment).

#### Summary

- Low-level API for implementing serverless deployment
- Every page in the `pages` directory becomes a serverless function (lambda)
- Creates the smallest possible serverless function (50Kb base zip size)
- Optimized for fast [cold start](https://zeit.co/blog/serverless-ssr#cold-start) of the function
- The serverless function has 0 dependencies (they are included in the function bundle)
- Uses the [http.IncomingMessage](https://nodejs.org/api/http.html#http_class_http_incomingmessage) and [http.ServerResponse](https://nodejs.org/api/http.html#http_class_http_serverresponse) from Node.js
- opt-in using `target: 'serverless'` in `next.config.js`
- Does not load `next.config.js` when executing the function, note that this means `publicRuntimeConfig` / `serverRuntimeConfig` are not supported

## Browser support

Next.js supports IE11 and all modern browsers out of the box using [`@babel/preset-env`](https://new.babeljs.io/docs/en/next/babel-preset-env.html). In order to support IE11 Next.js adds a global `Promise` polyfill. In cases where your own code or any external NPM dependencies you are using requires features not supported by your target browsers you will need to implement polyfills.

The [polyfills](https://github.com/zeit/next.js/tree/canary/examples/with-polyfills) example demonstrates the recommended approach to implement polyfills.

## TypeScript

TypeScript is supported out of the box in Next.js. To get started using it create a `tsconfig.json` in your project:

```js
{
  "compilerOptions": {
    "allowJs": true, /* Allow JavaScript files to be type checked. */
    "alwaysStrict": true, /* Parse in strict mode. */
    "esModuleInterop": true, /* matches compilation setting */
    "isolatedModules": true, /* to match webpack loader */
    "jsx": "preserve", /* Preserves jsx outside of Next.js. */
    "lib": ["dom", "es2017"], /* List of library files to be included in the type checking. */
    "module": "esnext", /* Specifies the type of module to type check. */
    "moduleResolution": "node", /* Determine how modules get resolved. */
    "noEmit": true, /* Do not emit outputs. Makes sure tsc only does type checking. */

    /* Strict Type-Checking Options, optional, but recommended. */
    "noFallthroughCasesInSwitch": true, /* Report errors for fallthrough cases in switch statement. */
    "noUnusedLocals": true, /* Report errors on unused locals. */
    "noUnusedParameters": true, /* Report errors on unused parameters. */
    "strict": true, /* Enable all strict type-checking options. */
    "target": "esnext" /* The type checking input. */
  }
}
```

After adding the `tsconfig.json` you need to install `@types` to get proper TypeScript typing.

```bash
npm install --save-dev @types/react @types/react-dom @types/node
```

Now can change any file from `.js` to `.ts` / `.tsx` (tsx is for files using JSX). To learn more about TypeScript checkout its [documentation](https://www.typescriptlang.org/).

### Exported types

Next.js provides `NextPage` type that can be used for pages in the `pages` directory. `NextPage` adds definitions for [`getInitialProps`](#fetching-data-and-component-lifecycle) so that it can be used without any extra typing needed.

```tsx
import { NextPage } from 'next'

interface Props {
  userAgent?: string
}

const Page: NextPage<Props> = ({ userAgent }) => (
  <main>Your user agent: {userAgent}</main>
)

Page.getInitialProps = async ({ req }) => {
  const userAgent = req ? req.headers['user-agent'] : navigator.userAgent
  return { userAgent }
}

export default Page
```

For `React.Component` you can use `NextPageContext`:

```tsx
import React from 'react'
import { NextPageContext } from 'next'

interface Props {
  userAgent?: string
}

export default class Page extends React.Component<Props> {
  static async getInitialProps({ req }: NextPageContext) {
    const userAgent = req ? req.headers['user-agent'] : navigator.userAgent
    return { userAgent }
  }

  render() {
    const { userAgent } = this.props
    return <main>Your user agent: {userAgent}</main>
  }
}
```

## AMP Support

<details>
  <summary><b>Examples</b></summary>
  <ul>
    <li><a href="https://github.com/zeit/next.js/tree/canary/examples/amp">amp</a></li>
  </ul>
</details>

### Enabling AMP Support

To enable AMP support for a page, add `export const config = { amp: true }` to your page.

### AMP First Page

```js
// pages/about.js
export const config = { amp: true }

export default function AboutPage(props) {
  return <h3>My AMP About Page!</h3>
}
```

### Hybrid AMP Page

```js
// pages/hybrid-about.js
import { useAmp } from 'next/amp'

export const config = { amp: 'hybrid' }

export default function AboutPage(props) {
  return (
    <div>
      <h3>My AMP Page</h3>
      {useAmp() ? (
        <amp-img
          width="300"
          height="300"
          src="/my-img.jpg"
          alt="a cool image"
          layout="responsive"
        />
      ) : (
        <img width="300" height="300" src="/my-img.jpg" alt="a cool image" />
      )}
    </div>
  )
}
```

### AMP Page Modes

AMP pages can specify two modes:

- AMP-only (default)
  - Pages have no Next.js or React client-side runtime
  - Pages are automatically optimized with [AMP Optimizer](https://github.com/ampproject/amp-toolbox/tree/master/packages/optimizer), an optimizer that applies the same transformations as AMP caches (improves performance by up to 42%)
  - Pages have a user-accessible (optimized) version of the page and a search-engine indexable (unoptimized) version of the page
  - Opt-in via `export const config = { amp: true }`
- Hybrid
  - Pages are able to be rendered as traditional HTML (default) and AMP HTML (by adding `?amp=1` to the URL)
  - The AMP version of the page only has valid optimizations applied with AMP Optimizer so that it is indexable by search-engines
  - Opt-in via `export const config = { amp: 'hybrid' }`
  - Able to differentiate between modes using `useAmp` from `next/amp`

Both of these page modes provide a consistently fast experience for users accessing pages through search engines.

### AMP Behavior with `next export`

When using `next export` to statically prerender pages Next.js will detect if the page supports AMP and change the exporting behavior based on that.

Hybrid AMP (`pages/about.js`) would output:

- `out/about.html` - with client-side React runtime
- `out/about.amp.html` - AMP page

AMP-only (`pages/about.js`) would output:

- `out/about.html` - Optimized AMP page

During export Next.js automatically detects if a page is hybrid AMP and outputs the AMP version to `page.amp.html`. We also automatically insert the `<link rel="amphtml" href="/page.amp" />` and `<link rel="canonical" href="/" />` tags for you.

> **Note**: When using `exportTrailingSlash: true` in `next.config.js`, output will be different. For Hybrid AMP pages, output will be `out/page/index.html` and `out/page.amp/index.html`, and for AMP-only pages, output will be `out/page/index.html`

### Adding AMP Components

The AMP community provides [many components](https://amp.dev/documentation/components/) to make AMP pages more interactive. You can add these components to your page by using `next/head`:

```js
// pages/hello.js
import Head from 'next/head'

export const config = { amp: true }

export default function MyAmpPage() {
  return (
    <div>
      <Head>
        <script
          async
          key="amp-timeago"
          custom-element="amp-timeago"
          src="https://cdn.ampproject.org/v0/amp-timeago-0.1.js"
        />
      </Head>

      <p>Some time: {date.toJSON()}</p>
      <amp-timeago
        width="0"
        height="15"
        datetime={date.toJSON()}
        layout="responsive"
      >
        .
      </amp-timeago>
    </div>
  )
}
```

### AMP Validation

AMP pages are automatically validated with [amphtml-validator](https://www.npmjs.com/package/amphtml-validator) during development. Errors and warnings will appear in the terminal where you started Next.js.

Pages are also validated during `next export` and any warnings / errors will be printed to the terminal.
Any AMP errors will cause `next export` to exit with status code `1` because the export is not valid AMP.

### TypeScript Support

AMP currently doesn't have built-in types for TypeScript, but it's in their roadmap ([#13791](https://github.com/ampproject/amphtml/issues/13791)). As a workaround you can manually add the types to `amp.d.ts` like [here](https://stackoverflow.com/a/50601125).

## Static HTML export

<details>
  <summary><b>Examples</b></summary>
  <ul>
    <li><a href="/examples/with-static-export">Static export</a></li>
  </ul>
</details>

`next export` is a way to run your Next.js app as a standalone static app without the need for a Node.js server.
The exported app supports almost every feature of Next.js, including dynamic urls, prefetching, preloading and dynamic imports.

The way `next export` works is by prerendering all pages possible to HTML. It does so based on a mapping of `pathname` key to page object. This mapping is called the `exportPathMap`.

The page object has 2 values:

- `page` - `String` the page inside the `pages` directory to render
- `query` - `Object` the `query` object passed to `getInitialProps` when prerendering. Defaults to `{}`

### Usage

Simply develop your app as you normally do with Next.js. Then run:

```
next build
next export
```

By default `next export` doesn't require any configuration. It will generate a default `exportPathMap` containing the routes to pages inside the `pages` directory. This default mapping is available as `defaultPathMap` in the example below.

If your application has dynamic routes you can add a dynamic `exportPathMap` in `next.config.js`.
This function is asynchronous and gets the default `exportPathMap` as a parameter.

```js
// next.config.js
module.exports = {
  exportPathMap: async function(
    defaultPathMap,
    { dev, dir, outDir, distDir, buildId }
  ) {
    return {
      '/': { page: '/' },
      '/about': { page: '/about' },
      '/readme.md': { page: '/readme' },
      '/p/hello-nextjs': { page: '/post', query: { title: 'hello-nextjs' } },
      '/p/learn-nextjs': { page: '/post', query: { title: 'learn-nextjs' } },
      '/p/deploy-nextjs': { page: '/post', query: { title: 'deploy-nextjs' } },
    }
  },
}
```

The pages will be exported as html files, i.e. `/about` will become `/about.html`.

It is possible to configure Next.js to export pages as `index.html` files and require trailing slashes, i.e. `/about` becomes `/about/index.html` and is routable via `/about/`.
This was the default behavior prior to Next.js 9.
You can use the following `next.config.js` to switch back to this behavior:

```js
// next.config.js
module.exports = {
  exportTrailingSlash: true,
}
```

> **Note**: If the export path is a filename (e.g. `/readme.md`) and is different than `.html`, you may need to set the `Content-Type` header to `text/html` when serving this content.

The second argument is an `object` with:

- `dev` - `true` when `exportPathMap` is being called in development. `false` when running `next export`. In development `exportPathMap` is used to define routes.
- `dir` - Absolute path to the project directory
- `outDir` - Absolute path to the `out/` directory (configurable with `-o` or `--outdir`). When `dev` is `true` the value of `outDir` will be `null`.
- `distDir` - Absolute path to the `.next/` directory (configurable using the `distDir` config key)
- `buildId` - The `buildId` the export is running for

Then simply run these commands:

```bash
next build
next export
```

For that you may need to add a NPM script to `package.json` like this:

```json
{
  "scripts": {
    "build": "next build",
    "export": "npm run build && next export"
  }
}
```

And run it at once with:

```bash
npm run export
```

Then you have a static version of your app in the `out` directory.

> You can also customize the output directory. For that run `next export -h` for the help.

Now you can deploy the `out` directory to any static hosting service. Note that there is an additional step for deploying to GitHub Pages, [documented here](https://github.com/zeit/next.js/wiki/Deploying-a-Next.js-app-into-GitHub-Pages).

For an example, simply visit the `out` directory and run following command to deploy your app to [ZEIT Now](https://zeit.co/now).

```bash
now
```

### Limitation

With `next export`, we build a HTML version of your app. At export time we will run `getInitialProps` of your pages.

The `req` and `res` fields of the `context` object passed to `getInitialProps` are empty objects during export as there is no server running.

> **Note**: If your pages don't have `getInitialProps` you may not need `next export` at all, `next build` is already enough thanks to [automatic prerendering](#automatic-prerendering).

> You won't be able to render HTML dynamically when static exporting, as we pre-build the HTML files. If you want to do dynamic rendering use `next start` or the custom server API

## Multi Zones

<details>
  <summary><b>Examples</b></summary>
  <ul>
    <li><a href="/examples/with-zones">With Zones</a></li>
  </ul>
</details>

A zone is a single deployment of a Next.js app. Just like that, you can have multiple zones and then you can merge them as a single app.

For an example, you can have two zones like this:

- An app for serving `/blog/**`
- Another app for serving all other pages

With multi zones support, you can merge both these apps into a single one allowing your customers to browse it using a single URL, but you can develop and deploy both apps independently.

> This is exactly the same concept of microservices, but for frontend apps.

### How to define a zone

There are no special zones related APIs. You only need to do following:

- Make sure to keep only the pages you need in your app, meaning that an app can't have pages from another app, if app `A` has `/blog` then app `B` shouldn't have it too.
- Make sure to add an [assetPrefix](https://github.com/zeit/next.js#cdn-support-with-asset-prefix) to avoid conflicts with static files.

### How to merge them

You can merge zones using any HTTP proxy.

You can use [now dev](https://zeit.co/docs/v2/development/basics) as your local development server. It allows you to easily define routing routes for multiple apps like below:

```json
{
  "version": 2,
  "builds": [
    { "src": "docs/next.config.js", "use": "@now/next" },
    { "src": "home/next.config.js", "use": "@now/next" }
  ],
  "routes": [
    { "src": "/docs/_next(.*)", "dest": "docs/_next$1" },
    { "src": "/docs(.*)", "dest": "docs/docs$1" },
    { "src": "(.*)", "dest": "home$1" }
  ]
}
```

For the production deployment, you can use the same configuration and run `now` to do the deployment with [ZEIT Now](https://zeit.co/now). Otherwise you can also configure a proxy server to route using a set of routes like the ones above, e.g deploy the docs app to `https://docs.example.com` and the home app to `https://home.example.com` and then add a proxy server for both apps in `https://example.com`.

## Recipes

- [Setting up 301 redirects](https://www.raygesualdo.com/posts/301-redirects-with-nextjs/)
- [Dealing with SSR and server only modules](https://arunoda.me/blog/ssr-and-server-only-modules)
- [Building with React-Material-UI-Next-Express-Mongoose-Mongodb](https://github.com/builderbook/builderbook)
- [Build a SaaS Product with React-Material-UI-Next-MobX-Express-Mongoose-MongoDB-TypeScript](https://github.com/async-labs/saas)

## FAQ

<details>
  <summary>Is this production ready?</summary>
  Next.js has been powering https://zeit.co since its inception.

We’re ecstatic about both the developer experience and end-user performance, so we decided to share it with the community.

</details>

<details>
  <summary>How big is it?</summary>

The client side bundle size should be measured in a per-app basis.
A small Next main bundle is around 65kb gzipped.

</details>

<details>
  <summary>Is this like `create-react-app`?</summary>

Yes and No.

Yes in that both make your life easier.

No in that it enforces a _structure_ so that we can do more advanced things like:

- Server side rendering
- Automatic code splitting

In addition, Next.js provides two built-in features that are critical for every single website:

- Routing with lazy component loading: `<Link>` (by importing `next/link`)
- A way for components to alter `<head>`: `<Head>` (by importing `next/head`)

If you want to create re-usable React components that you can embed in your Next.js app or other React applications, using `create-react-app` is a great idea. You can later `import` it and keep your codebase clean!

</details>

<details>
  <summary>How do I use CSS-in-JS solutions?</summary>

Next.js bundles [styled-jsx](https://github.com/zeit/styled-jsx) supporting scoped css. However you can use any CSS-in-JS solution in your Next app by just including your favorite library [as mentioned before](#css-in-js) in the document.

</details>

<details>
  <summary>What syntactic features are transpiled? How do I change them?</summary>

We track V8. Since V8 has wide support for ES6 and `async` and `await`, we transpile those. Since V8 doesn’t support class decorators, we don’t transpile those.

See the documentation about [customizing the babel config](#customizing-babel-config) and [next/preset](/packages/next/build/babel/preset.js) for more information.

</details>

<details>
  <summary>Why a new Router?</summary>

Next.js is special in that:

- Routes don’t need to be known ahead of time
- Routes are always lazy-loadable
- Top-level components can define `getInitialProps` that should _block_ the loading of the route (either when server-rendering or lazy-loading)

As a result, we were able to introduce a very simple approach to routing that consists of two pieces:

- Every top level component receives a `url` object to inspect the url or perform modifications to the history
- A `<Link />` component is used to wrap elements like anchors (`<a/>`) to perform client-side transitions

</details>

<details>
<summary>How do I define a custom fancy route?</summary>

We [added](#custom-server-and-routing) the ability to map between an arbitrary URL and any component by supplying a request handler.

On the client side, we have a parameter call `as` on `<Link>` that _decorates_ the URL differently from the URL it _fetches_.

</details>

<details>
<summary>How do I fetch data?</summary>

It’s up to you. `getInitialProps` is an `async` function (or a regular function that returns a `Promise`). It can retrieve data from anywhere.

</details>

<details>
  <summary>Can I use it with GraphQL?</summary>

Yes! Here's an example with [Apollo](/examples/with-apollo).

</details>

<details>
<summary>Can I use it with Redux and thunk?</summary>

Yes! Here's an [example](/examples/with-redux-thunk).

</details>

<details>
<summary>Can I use it with Redux?</summary>

Yes! Here's an [example](/examples/with-redux).

</details>

<details>
<summary>Can I use Next with my favorite Javascript library or toolkit?</summary>

Since our first release we've had **many** example contributions, you can check them out in the [examples](/examples) directory.

</details>

<details>
<summary>What is this inspired by?</summary>

Many of the goals we set out to accomplish were the ones listed in [The 7 principles of Rich Web Applications](http://rauchg.com/2014/7-principles-of-rich-web-applications/) by Guillermo Rauch.

The ease-of-use of PHP is a great inspiration. We feel Next.js is a suitable replacement for many scenarios where you otherwise would use PHP to output HTML.

Unlike PHP, we benefit from the ES6 module system and every file exports a **component or function** that can be easily imported for lazy evaluation or testing.

As we were researching options for server-rendering React that didn’t involve a large number of steps, we came across [react-page](https://github.com/facebookarchive/react-page) (now deprecated), a similar approach to Next.js by the creator of React Jordan Walke.

</details>

## Contributing

Please see our [contributing.md](/contributing.md).

## Authors

- Arunoda Susiripala ([@arunoda](https://twitter.com/arunoda)) – [ZEIT](https://zeit.co)
- Tim Neutkens ([@timneutkens](https://twitter.com/timneutkens)) – [ZEIT](https://zeit.co)
- Naoyuki Kanezawa ([@nkzawa](https://twitter.com/nkzawa)) – [ZEIT](https://zeit.co)
- Tony Kovanen ([@tonykovanen](https://twitter.com/tonykovanen)) – [ZEIT](https://zeit.co)
- Guillermo Rauch ([@rauchg](https://twitter.com/rauchg)) – [ZEIT](https://zeit.co)
- Dan Zajdband ([@impronunciable](https://twitter.com/impronunciable)) – Knight-Mozilla / Coral Project

```

```
