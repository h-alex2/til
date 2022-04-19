# Create a Next.js and MDX blog
<https://blog.logrocket.com/create-next-js-mdx-blog/>

## Create your first Next.js page in MDX
pages 폴더 밑에 about.mdx 파일을 만드세요. 아무 마크다운이나 jsx콘텐트를 써보시거나 단순히 밑을 복사하셔도 됩니다.

```
import Head from 'next/head';

<Head>
  <title>Hello World - PressBlog</title>
</Head>

# Hello World
```

`npm run dev` 명령과 함께 Next.js 서버를 시작하면  브라우저의 `/about` 페이지를 방문할 수 있습니다.
제목이 `Hello Word - PressBlog`이고 본문에는 `Hello World`가 나와야합니다.

보통은 404 에러가 나오게되지만 우리가 `next.config-mjs`파일에 추가한 구성 덕분에 MDX로 페이지를 만들 수 있습니다.

이젠 모든 셋업을 했고 블로그를 만들어볼까요?


## Organize our app
파일을 어떻게 정리하고 구성해야할까요? 방금 MDX로 페이지를 만들었어요. 맞죠? 그럼 모든 페이지를 MDX로 만들고 자바스크립트로 사용자 구성요소(widgets, you might say)를 만들어야만 할까요?
MDX로도 사용자 구성요소를 만들 수 있다면 MDX로 무엇이든 만들지 못하는 이유는 뭔가요? 

Well, 진실은 MDX로 블로그를 구성하는 것이 처음엔 혼란스러울 수 있다는 것입니다. 이제 거의 동일한 작업을 할 수 있는 두 가지 도구를 가졌으므로 문제는 어느 것을 사용해야 하고 언제 사용해야 할까요? 
이것에 답하기 위해, MDX의 목적이 JSX의 모든 사용 케이스를 대체하는 것이 아니라는 것을 이해해야 합니다. MDX의 목적은 우리가 대화형 및 동적으로 풍부한 콘텐츠를 마크다운으로 쉽게 생성할 수 있다는 것입니다. 

MDX를 사용하여 일부 링크가 있는 블로그의 이름과 로고만을 가진 `header` 컴포넌트를 만들 수 있지만 도구 낭비일 뿐이므로 사용해서는 안됩니다.

많은 양의 콘텐츠가 담긴 것을 쓸 때는 JSX보다 MDX를 사용해야 합니다. 우리 기사에는 많은 콘텐츠만 있습니다. 이것이 대부분의 MDX 사용사례에는 필요하지 않기 때문에 구성요소 파일에서 `.mdx` 파일을 거의 찾을 수 없는 이유입니다.

이제 기사에 MDX만 사용하기로 결정했으므로 어떻게 기사를 구성할 것인지 결정해야합니다. 우린 일찍이 MDX로 페이지를 만드는 것이 가능하다는 것을 보았습니다. MDX에로 모든 포스트/기사를 페이지로서 사용하는 것이 더 간단하지 않을까요?

이 말은 `Next.js app`의 `pages` 폴더안에 `posts`폴더가 있고 이 디렉토리에서 MDX로 모든 포스트를 간단하게 작성할 수 있다는 것입니다.

예를 들어, this would look like an article as pages/posts/what-is-react-native.mdx and another article as pages/posts/where-to-learn-react-native.mdx.

전통적으로 또는 대안적으로 Next.js에서는, `pages` 폴더 외부에 `posts` 폴더를 가지고 하나의 동적 파일(route)만 가진`pages`디렉토리 안에 다른 `posts` 디렉토리를 가질 것입니다.

그건 예를 들어, That is, for example, pages/posts/[slug].js and fetching the required post in [slug].js based on the slug.

이것은 여전히 가능하지만 MDX의 유용성 중 하나를 죽입니다. 우리는 오직 첫 번째 접근 방식만 사용할 것이지만 기본적으로 두 번째 접근 방식이 어떻게 디렉토리에서 MDX files를 fetch하고 parse하는지 보여드리겠습니다.

튜토리얼이 끝나면 프로젝트 구조가 밑과 같이 됩니다. (not including unchanged directories and files that come with Next.js by default):

```js
PressBlog
|
|___ next.config.mjs
|
|___ components
|     |___ Header.js
|     |___ MDXComponents.js
|     |___ MeetMe.js
|     |___ Meta.js
|     |___ PostItem.js
|
|___ layouts
|     |___ Layout.js
|
|___ pages
|     |___ index.js
|     |___ about.mdx
|     |___ _app.js
|     |___ posts
|           |___ index.js
|           |___ learn-react-navigation.mdx
|           |___ what-is-react-native.mdx
|
|___ scripts
|     |___ fileSystem.js
|
|___ styles
|     |___ globals.css
|     |___ Header.module.css
|     |___ Home.module.css
|     |___ Markdown.module.css
```

## Using CSS for styling 
이 튜토리얼에서는, Tailwind, Bootstrap 없이 기본적인 Next.js 스타일을 사용할 것입니다. 하지만 styled-components 또는 다른 프레임워크를 사용하는 것도 가능합니다.

이 블로그에 적용된 모든 스타일을 지금 적용해 보겠습니다.

First, 헤더에만 적용되는 `Header.module.css`를 추가해보겠습니다. 다음 스타일을 복사하여 붙여넣으세요.

```cs
.header {
  padding: 10px 0;
  background-color: rgb(164, 233, 228);
}

.header > div {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.header li {
  list-style: none;
  display: inline-block;
  margin: 0 10px;
}
```

다음으로, 이미 존재하고 있는 `global.css`를 사용할 것이니 만들지 않아도 됩니다. 다음 스타일만 복사해서 붙여넣으면 됩니다.

```cs
a {
  color: inherit;
  text-decoration: underline transparent;
  transition: 0.25s ease-in-out;
}

a:hover {
  text-decoration-color: currentColor;
}

.max-width-container {
  max-width: 680px;
  margin-left: auto;
  margin-right: auto;
  padding: 0 10px;
}

.max-width-container.main {
  padding: 50px 30px;
}

p {
  line-height: 1.6;
}
```

다음으로, 홈페이지에만 적용되는 `Home.module.css`에 밑 코드를 넣습니다.

```cs
.mainContainer {
  padding: 15px 30px;
  padding-top: 45px;
}

.articleList {
  margin-top: 55px;
}

.desc {
  color: #3b3b3b;
  font-style: italic;
  font-size: 0.85rem;
}

.articleList > div {
  margin-bottom: 50px;
}

.img {
  max-width: 100%;
  height: auto;
  border-radius: 50%;
}
```

마지막으로, 기사 내용에만 적용되는 Markdown.module.css를 만듭니다.

```cs
.postTitle {
  border-bottom: 2px solid rgb(164, 233, 228);
  padding-bottom: 13px;
}

.link {
  color: rgb(0, 110, 255);
}

.tooltipText {
  background: hsla(0, 0%, 0%, 0.75);
  color: white;
  border: none;
  border-radius: 4px;
  padding: 0.5em 1em;
}
```


## Creating the about page
For now, `about` 페이지에서 작업해 보겠습니다. 이미 `about.mdx` 페이지를 만들었다는 것 기억하세요. Lorem text를 삽입해봅시다. 하지만 그 전에 `MeetMe.js` 라는 구성 요소를 만들고 싶습니다. 
이 구성 요소에는 작성자의 간단한 설명과 이미지가 포함됩니다.  

컴포넌트로 생성해서 홈페이지에서도 사용할 수 있습니다. app의 root 디렉토리로 `components` 디렉토리를 만듭니다. 그곳에 `MeetMe.js` 라는 파일을 만들고 밑 코드를 붙여넣어주세요.

여기서는 많은 일이 일어나지 않습니다. 단순히 next/image로부터 Image 컴포넌트를 불러오고 단락과 이미지에 이미 만들어놨던 `Home.module.css`를 사용하여 스타일을 적용시킵니다.

이제 MeetMe 구성요소 설정을 했으므로 `about.mdx`로 가져옵시다.

```js
import MeetMe from '../components/MeetMe.js';

<MeetMe />

#### Let's dive into more of me

Lorem, ipsum dolor sit amet consectetur adipisicing elit. Ea deserunt ab maiores eligendi nemo, ipsa, pariatur blanditiis, ullam exercitationem beatae incidunt deleniti ut sit est accusantium dolorum temporibus ipsam quae.

Lorem, ipsum dolor sit amet consectetur adipisicing elit. Ea deserunt ab maiores eligendi nemo, ipsa, pariatur blanditiis, ullam exercitationem beatae incidunt deleniti ut sit est accusantium dolorum temporibus ipsam quae.

Lorem, ipsum dolor sit amet consectetur adipisicing elit. Ea deserunt ab maiores eligendi nemo, ipsa, pariatur blanditiis, ullam exercitationem beatae incidunt deleniti ut sit est accusantium dolorum temporibus ipsam quae.
```

마지막으로 이 페이지를 check out 하기 전에 메타 태그와 제목을 포함할 `Meta` 구성요소를 만듭시다.  
`components` 디렉토리로 가서 `Meta.js`라는 파일을 만들고 밑을 붙여넣습니다.

```js
import Head from 'next/head';

const Meta = ({ title }) => {
  return (
    <Head>
      <title>{title}</title>
      <meta
        name='keywords'
        content='react native, blog, John Doe, tutorial, react navigation'
      />
    </Head>
  );
};

export default Meta;

// let's set a default title
```

이것을 단순하게 유지하기 위해서는 정적인 meta 키워드와 동적 제목만 사용해야합니다. 실제 블로그에서는 검색엔진을 더 최적화시킬 필요가 있습니다.

`about.mdx`에 `Meta.js`를 불러옵시다.

```js
{
  /* .. */
}
import Meta from '../components/Meta.js';

<Meta title='About John Doe - PressBlog' />;

{
  /* .. */
}
```
이를 통해 파일을 저장하고 정보 페이지를 테스트할 수 있습니다.

## Creating the layout

다음으로 작업할 것은 블로그 레이아웃입니다. 계속 진행하기 전에 header 컴포넌트가 필요하므로 만들어봅시다. components 디렉토리로 이동하여 `Header.js`를 만들고 밑 코드를 붙여넣으세요.

```js
import Link from 'next/link';
import styles from '../styles/Header.module.css';

const Header = () => {
  return (
    <header className={styles.header}>
      <div className='max-width-container'>
        <h2>
          <Link href='/'>PressBlog</Link>
        </h2>
        <ul>
          <li>
            <Link href='/posts'>Blog</Link>
          </li>
          <li>
            <Link href='/about'>About</Link>
          </li>
        </ul>
      </div>
    </header>
  );
};

export default Header;
```

여기서 `next/link`의 `Link` 컴포넌트를 사용합니다. 또한 미리 만들어놨던 `Header.module.css`를 사용합니다.

그러나 스타일에 대한 약간의 변경이 있습니다. `Header.module.css`가 아닌 `max-width-container`를 사용했음을 주목하세요. 이건 오타가 아닙니다. 클래스 이름은 전역적으로 필요하기 때문에 이전에 `globals.css`에서 사용되었습니다.

다음으로, root 디렉토리에 `layout` 폴더를 만들고 그 안에 `Layout.js` 파일을 만듭시다.

```js
import Header from '../components/Header';

const Layout = ({ children }) => {
  return (
    <div>
      <Header />
      <main className='max-width-container main'>{children}</main>
    </div>
  );
};

export default Layout;
```
여기에서 방금 만든 `Header` 컴포넌트를 불러오고 블로그 레이아웃의 헤더로 전달합니다. footer에서도 동일한 작업을 할 수 있지만 이 블로그에서는 footer를 사용하지 않습니다.

이제 Layout을 적용하기 위해서 `pages/_app.js`로 이동하고 `Component` 컴포넌트 주위에 `Layout` 구성요소를 래핑합니다.

```js
//..
return (
  <Layout>
    <Component {...pageProps} />
  </Layout>
);

//..
```


## Adding the articles 
이번 섹션에서는 두개의 아티클을 만들 것입니다. 이 아티클은 나중에 홈페이지와 블로그 페이지 (all blog posts)에서 사용할 것입니다. 하지만 지금은 아티클을 만들고 싱글 페이지로서 테스트만 할 것입니다.

`pages` 디렉토리 밑에 `posts`라는 새 디렉토리를 만듭시다. 이 디렉토리에서 밑과 같은 MDX 파일을 만듭시다.

`learn-react-navigation.mdx:`
```js
---
title: React Navigation Tutorial
publishedOn: January, 6th. 2022
excerpt: React Navigation is a number one tool to learn in React Native. You would always use them; these are some of the best practices in using React Navigation
---

import Meta from '../../components/Meta';
import { Disclosure, DisclosureButton, DisclosurePanel } from "@reach/disclosure";

<Meta title='React Navigation Tutorial - PressBlog' />

# React Navigation Tutorial

React Navigation is one of the best things that happened to [**React Native**](https://reactnative.org)

## Benefits of using React Navigation

1. It is cool to use
2. It is simple to learn
3. It has an extensive community
4. And more and more

<Disclosure>
  <DisclosureButton as='div'>React Navigation won't be here forever. Click to find why</DisclosureButton>
  <DisclosurePanel>Nothing lasts forever, yeah! That's all I got to say</DisclosurePanel>
</Disclosure>
```

