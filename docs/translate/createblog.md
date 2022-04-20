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
  
Well, 진실은 MDX로 블로그를 구성하는 것이 처음엔 혼란스러울 수 있다는 것입니다.   
이제 거의 동일한 작업을 할 수 있는 두 가지 도구를 가졌으므로 문제는 어느 것을 사용해야 하고 언제 사용해야 할까요?  
  
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

`Disclosure` 컴포넌트와 그 구성요소들은 MDX 위젯과 구성요소 사용 방법과 이것이 기사에 얼마나 유용한지 보여줍니다. 다음 기사에서는 tooltip을 사용하므로 `what-is-react-native.mdx`
를 체크하러 가봅시다.  
`what-is-react-native.mdx`

```js
---
title: What is React Native?
publishedOn: January, 7th. 2022
excerpt: Lorem, ipsum dolor sit amet consectetur adipisicing elit. Ea deserunt ab maiores eligendi nemo, ipsa, pariatur
---

import Meta from '../../components/Meta.js';
import Tooltip from '@reach/tooltip';
import '@reach/tooltip/styles.css';

<Meta title='What is React Native - PressBlog' />

# What is React Native?

React Native is a lovely language

<Tooltip label="It's not actually a language though">
  <button>Fact about React native</button>
</Tooltip>
```

브라우저에서 각 기사를 테스트하여 작동하는지 확인할 수 있습니다. 메타데이터가 포함된 부분이 브라우저에 표시되지 않는다는 점에 유의하세요. 독자가 이것을 보지 않기를 바라는 것은 좋은 일입니다.  

이것은 `next.config.mjs` 에 설치하고 구성한 `remark-frontmatter`때문에 가능합니다. 

## Adding article-specific styles

블로그 기사에만 스타일을 추가해 보겠습니다. 우리가 사용할 접근방식을 사용하면 MDX의 모든 요소에 스타일을 지정할 수 있습니다.
제가 element라고 하는 것은 JSX로 컴파일할 때를 의미합니다. 예를 들어 MDX의 #Hello world는 JSX의 h1으로 컴파일됩니다. 따라서 이 접근 방식을 사용하면 MDX의 모든 h1 요소에 스타일을 지정할 수 있습니다.

  
이를 수행하는 방법은 @mdx-js/react의 MDXProvider로 앱 구성 요소를 래핑하고 필요한 만큼 많은 요소를 매핑하기 위한 구성 요소 개체를 제공하는 것입니다.
따라서 components 디렉토리로 이동하여 `MDXComponents.js`라고 불리는 새 파일을 만들고 밑 코드를 붙여넣습니다.

```js
import styles from '../styles/Markdown.module.css';

const MDXComponents = {
  p: (props) => <p {...props} className={styles.p} />,
  a: (props) => <a {...props} className={styles.link} />,
  h1: (props) => <h1 {...props} className={styles.postTitle} />,
};

export default MDXComponents;
```
여기서는 Markdown 콘텐츠에 대해서만 이전에 만든 스타일을 가져옵니다. `MDXComponents` 개체의 각 키는 MDX가 컴파일되는 대상에 해당하는 구성 요소입니다.  
이제 모든 JSX 요소에 대한 사용자 정의 구성 요소를 생성하기로 결정할 수 있습니다.

```js
import { Sparky } from 'sparky-text'; // a dummy module - does not exist
import Link from 'next/link';
import styles from '../styles/Markdown.module.css';

const MDXComponents = {
  strong: ({ children }) => <Sparky color='gold'>{children}</Sparky>,
  p: (props) => <p {...props} className={styles.p} />,
  a: (props) => <Link {...props} className={styles.link} />,
  h1: (props) => <h2 {...props} className={styles.postTitle} />, // for some reasons I want h1 to be mapped, styled and displayed as h2
};

// please do not use this example, this is only a demonstration
```


현재 `MDXComponents.js`는 아무런 효과가 없습니다. 따라서 이를 수정하려면 `pages/_app.js`로 이동하여 `@mdx-js/react`에서 `MDXProvider`를 가져오고 방금 만든 `MDXComponents` 구성 요소를 가져옵니다.  

레이아웃 구성 요소 주위에 MDXProvider를 래핑하고 MDXComponents를 MDXProvider에 대한 구성 요소 소품으로 전달합니다. 요약하면 _app.js는 다음과 같아야 합니다.

```js
import '../styles/globals.css';
import { MDXProvider } from '@mdx-js/react';
import MDXComponents from '../components/MDXComponents';
import Layout from '../layouts/Layout';

function MyApp({ Component, pageProps }) {
  return (
    <MDXProvider components={MDXComponents}>
      <Layout>
        <Component {...pageProps} />
      </Layout>
    </MDXProvider>
  );
}

export default MyApp;
```

## Creating the home page
홈 페이지는 소개 섹션과 10개의 기사 목록으로 구성되어 있습니다.  
Node.js 파일 시스템과 경로 모듈을 사용하여 이 10개의 기사를 가져오는 것으로 시작하겠습니다(최대 10개는 아니지만 잘 작동할 것입니다). 이를 위해 루트 디렉토리에 `scripts`라는 디렉토리를 생성하고 그 안에 `fileSystem.js`라는 파일을 생성합니다.  
fileSystem.js에서 생성할 함수는 MDX의 모든 게시물(즉, pages/posts 디렉토리에서)을 가져옵니다.  
이 함수는 Next.js의 getStaticProps 함수에서 사용할 수 있습니다.   
  
이것이 `fileSystem.js`의 모든 게시물을 가져올 수 있는 방법입니다.
1. Node.js 시스템 모듈의 `readdirSync`, `readFileSync` 메서드 사용해서 `pages/posts`에서 `.mdx`파일 가져오기
2. `gray-matter`을 사용하여 각 파일의 Front Matter를 메타데이터 개체로 구문 분석하기
3. 파일 이름을 사용하여 각 파일에 대한 슬러그 생성
  
이제 코드를 봅시다. fileSystem.js에서 아래 코드를 복사하여 붙여넣습니다.

```js
import fs from 'fs';
import path from 'path';
import matter from 'gray-matter';

const getPosts = (limit) => {
  const dirFiles = fs.readdirSync(path.join(process.cwd(), 'pages', 'posts'), {
    withFileTypes: true,
  });

  const posts = dirFiles
    .map((file) => {
      if (!file.name.endsWith('.mdx')) return;

      const fileContent = fs.readFileSync(
        path.join(process.cwd(), 'pages', 'posts', file.name),
        'utf-8'
      );
      const { data, content } = matter(fileContent);

      const slug = file.name.replace(/.mdx$/, '');
      return { data, content, slug };
    })
    .filter((post) => post);

  if (limit) {
    return posts.filter((post, index) => {
      return index + 1 <= limit;
    });
  }

  return posts;
};

export default getPosts;
```

꽤 많지만 각 라인을 살펴보겠습니다.
가장 먼저 하는 일은 `fs`(Node.js에서), `path`(Node.js에서) 및 `matter`(`gray-matter`에서)인 모듈을 가져오는 것입니다.  
다음으로 매개변수 제한이 하나만 있는 `getPost` 함수를 만들고 내보냅니다.    
  
그런 다음 `readdirSync`는 파일 확장명이 있는 `pages/posts` 디렉토리의 모든 파일을 가져와 `dirFiles` 변수에 할당합니다.  
  
이제 `dirFiles`를 반복하여 MDX가 아닌 모든 파일을 먼저 필터링합니다.
루프에 있는 동안 `readFileSync` 메서드를 사용하여 각 파일의 내용을 읽을 수 있으며 내용을 `matter` 함수의 매개변수로 전달할 수 있습니다.  
  
그러면 `matter` 함수는 `data`와 콘텐츠만 필요한 객체를 반환합니다. `data`는 각 기사에서 `Front Matter`를 사용하여 작성한 메타데이터이고 `content`는 기사의 나머지 내용입니다.  
  
마지막으로 루프에서 파일 이름(확장자 제외)에서 슬러그를 생성하고 루프 함수에서 `data`, `content` 및 `slug`의 개체를 반환합니다.  
  
홈페이지 외에도 블로그 페이지에서도 `getPost` 기능을 사용하게 되지만 블로그 페이지에는 제한이 없어 조건이 발생합니다.  
  
모든 게시물을 가져올 때 비동기식으로 수행하고 있지 않은지 확인하고 싶습니다. 이것이 `readdirSync` 및 `readFileSync`의 이유입니다. 모두 동기식이기 때문에  
  
이제 `pages/index.js`를 열고 모든 태그와 import 문을 지울 수 있습니다(모든 Next.js 앱에 기본적으로 제공됨). `index` 함수 외부에 새 함수 `getStaticProps`를 만들고 아래 코드를 복사하여 붙여넣습니다.

```js
export const getStaticProps = () => {
  const posts = getPosts(10);

  return {
    props: {
      posts,
    },
  };
};
```

Next.js로 작업했다면 매우 간단합니다.  
하지만 없다면 Next.js에서 데이터를 가져오는 데 사용하는 함수입니다.  
Next.js는 내보낸 getStaticProps 함수에서 반환된 소품을 사용하여 빌드 시 index.js 페이지를 미리 렌더링합니다. 이 프로세스를 정적 ​​사이트 생성이라고 합니다.  
  
여기에서 많은 필터링 및 정렬을 수행할 수 있습니다. 예를 들어, 10개가 아닌 최근에 게시된 10개의 기사를 가져오려면 각 게시물에서 반환된 `publishedOn`  메타데이터를 사용하여 정렬할 수 있습니다.  
  
그러나 `getStaticProps` 함수를 내보내야 합니다. 그렇지 않으면 다른 함수와 같습니다. 이 함수에서 CMS 또는 API에서 데이터를 fetch 할 수 있습니다.  
  
Next.js는 `index` 함수의 `getStaticProps`에서 반환된 props를 사용하는 것에 대한 편안함을 제공합니다. 이것이 의미하는 바는 이 페이지의 `index` 기능(pages/index.js)에 전달된 소품으로 게시물이 있다는 것입니다.
```js
const index = ({ posts }) => {
  return (
    <>
      <Meta />
      <MeetMe />
      <Link href='/about'>More about me</Link>

      <div className={styles.articleList}>
        <p className={styles.desc}>Newly Published</p>
        {posts.map((post) => (
          <p key={post.slug}>{post.data.title}</p>
        ))}
      </div>
    </>
  );
};
export default index;
```
`Meta` 구성 요소를 사용하여 페이지에 메타 태그를 추가함으로써 `MeetMe` 구성 요소는 작성자에 대한 간단한 설명을 표시하며 여기에는 정보 페이지의 전체 설명에 대한 링크가 있습니다.  
  
마지막으로 `index` 함수에 대한 소품으로 오는 `posts` 배열을 반복합니다. 또한 `index` 및 `getStaticProps`에 사용되는 구성 요소와 함수를 가져와야 합니다.  
  
다음은 `pages/index.js`에서 이 모든 것을 보여주는 코드 스니펫입니다.

```js
import MeetMe from '../components/MeetMe.js';
import Link from 'next/link';
import getPosts from '../scripts/fileSystem';
import styles from '../styles/Home.module.css';
import Meta from '../components/Meta';

const index = ({ posts }) => {
  return (
    <>
      <Meta title='PressBlog - Your one stop blog for anything React Native' />
      <MeetMe />
      <Link href='/about'>More about me</Link>

      <div className={styles.articleList}>
        <p className={styles.desc}>Newly Published</p>
        {posts.map((post) => (
          <p key={post.slug}>{post.data.title}</p>
        ))}
      </div>
    </>
  );
};

export default index;

export const getStaticProps = () => {
  const posts = getPosts(10);

  return {
    props: {
      posts,
    },
  };
};
```
현재로서는 각 기사의 제목만 단락에 표시하고 있는데, 이는 우리가 원하는 것과는 거리가 멉니다. `PostItem`이라는 새 구성 요소를 만들어 보겠습니다.  
이렇게 하려면 `components` 디렉터리에 `PostItem.js`라는 파일을 만들고 아래 코드를 복사하여 붙여넣습니다.

```js
import Link from 'next/link';

const PostItem = ({ post }) => {
  return (
    <div>
      <h3>
        <Link href={`/posts/${post.slug}`}>{post.data.title}</Link>
      </h3>
      <p>{post.data.excerpt}</p>
      <Link href={`/posts/${post.slug}`}>Read more</Link>
    </div>
  );
};

export default PostItem;
```

이제 `pages/index.js`로 돌아가 방금 만든 `PostItem` 구성 요소를 가져옵니다.  
그런 다음 각 게시물의 제목을 표시하는 단락을 `<PostItem key={post.slug} post={post} />`로 바꿉니다.
```js
//..
<div className={styles.articleList}>
  <p className={styles.desc}>Newly Published</p>
  {posts.map((post) => (
    <PostItem key={post.slug} post={post} />
  ))}
</div>
//..
```
마지막으로 파일을 저장하고 브라우저에서 홈 페이지를 테스트합니다. 이것은 블로그 페이지에 적용할 동일한 원칙입니다.

## Creating the blog page
이것이 마지막 페이지입니다. 이 페이지는 한 페이지에 모든 게시물을 표시합니다. 따라서 시작하려면` pages/posts `디렉토리에` index.js`라는 새 파일을 만들고 다음을 붙여넣습니다.  
  
이것은 우리가 홈 페이지에서 본 것과 같으며 index 기능에 약간의 변경만 가했습니다. 하지만 `getStaticProps`의 `getPosts` 함수에 전달된 인수에 유의하십시오. `false` 값을 사용하면 모든 게시물(즉, `pages/posts`의 모든 MDX 파일)이 반환됩니다.

이제 파일을 저장하고 새 블로그 페이지를 테스트합니다(브라우저에서 /posts 방문). 이를 통해 Next.js 및 MDX로 ​​블로그를 성공적으로 구축했습니다.

## Conclusion
Markdown을 사용하면 대화형 블로그나 문서를 만드는 것이 그 어느 때보다 쉬워졌습니다. 당신이 따라했다면 당신은 자신의 블로그를 갖는 것에서 한 발짝 떨어져 있습니다. 배포하기 전에 더 많은 기능을 추가할지 여부를 결정할 수 있습니다.  
  
문서를 작성하려는 경우 이 자습서가 도움이 될 수도 있고 단순히 `Nextra`를 사용할 수도 있습니다. `Nextra`는 Next.js 및 MDX를 사용하여 문서용 웹 사이트를 만들고 물론 모든 콘텐츠를 MDX로 ​​작성할 수 있습니다.  
  
이 모든 것을 정리하기 위해 이 기사에서 우리는 MDX라는 흥미로운 도구에 대해 배웠습니다. 표현식 작성, 소품 전달, MDX 및 Next.js로 블로그 만들기 등 이 도구로 할 수 있는 작업이 있습니다  
  
이 블로그를 구축하면서 우리는 Next.js에서 작동하도록 MDX를 구성하는 방법, 앱을 구조화하는 방법, MDX 파일을 가져와 메타데이터(Front Matter에서) 및 콘텐츠의 개체로 구문 분석하는 방법을 보았습니다.  
  
So, that’s it. Thanks for reading.