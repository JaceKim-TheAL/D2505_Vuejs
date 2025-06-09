[vuejs]: readme.md
[![적용기술](https://skillicons.dev/icons?i=vue,vercel,ts,vscode)][vuejs]

### INDEX

<table>
  <tr>
    <td><a href="sect_01.md"> [설치]        </a></td>
    <td><b href="sect_02.md"> [레이아웃]    </b></td>
    <td><a href="sect_03.md"> [스크롤]      </a></td>
    <td><a href="sect_04.md"> [탐색]        </a></td>
    <td><a href="sect_05.md"> [동적경로]    </a></td>
    <td><a href="sect_06.md"> [중첩경로]     </a></td>
    <td><a href="sect_07.md"> [없는페이지]    </a></td>
    <td><a href="sect_08.md"> [내비게이션]   </a></td>
    <td><a href="sect_09.md"> [페이지전환]   </a></td>
    <td><a href="sect_10.md"> [배포]        </a></td>
  </tr>
</table>

---
# 2. 레이아웃
- [기본레이아웃 추가](#기본레이아웃-추가)
- [빈 레이아웃 추가](#빈-레이아웃-추가)
- [라우터 객체의 속성 지정](#라우터-객체의-속성-지정)
- [레이아웃 제공자 추가](#레이아웃-제공자-추가)

---
### 기본레이아웃 추가
```shell
├─src/
│  ├─components/
│  │  └─TheHeader.vue
│  ├─routes/
│  │  ├─layouts/
│  │  │  ├─DefaultLayout.vue
│  │  │  ├─EmptyLayout.vue
│  │  │  └─LayoutProvider.vue
│  │  ├─pages/
│  │  │  ├─AboutPage.vue
│  │  │  └─HomePage.vue
│  │  └─index.ts
|  ├─App.vue
│  └─main.ts
```
<br/>

헤더를 만들어 각 페이지로 이동할 수 있는 내비게이션 버튼을 제공하고 필요한 모든 페이지에서 표시할 수 있도록 해봅시다. <br/>
먼저 다음과 같이 <TheHeader> 컴포넌트를 만듭니다.<br/>

<pre>
ⓘ 헤더의 내비게이션 버튼은 일단 <a> 태그로 만들지만, 이후에 <RouterLink> 컴포넌트로 대체할 것입니다.
</pre>

`[/src/components/TheHeader.vue]`
```vue
<template>
  <header>
    <nav>
      <a href="/">Home</a>
      <a href="/about">About</a>
    </nav>
  </header>
</template>
```
<br/>

헤더나 푸터 같이 대부분의 페이지에서 공통적으로 사용되는 구조를 각 페이지 컴포넌트에서 직접 추가하면, 페이지 전환 시마다 불필요한 리렌더링이 발생하게 됩니다.<br/>
그래서 공통 구조를 다시 렌더링하지 않도록 별도의 레이아웃 구조를 제공해야 합니다.<br/>
<br/>

다음과 같이 `기본 레이아웃(DefaultLayout.vue)`을 추가해서 앞서 만든 헤더를 표시합니다.<br/>
그리고 접근 경로에 맞게 페이지가 출력될 위치를 `<slot> 컴포넌트`로 지정합니다.<br/>

`[/src/routes/layouts/DefaultLayout.vue]`
```vue
<script setup lang="ts">
import TheHeader from '@/components/TheHeader.vue'
</script>

<template>
  <TheHeader />
  <slot />
</template>
```
<br/>

[[TOP]](#index)

---
### 빈 레이아웃 추가
헤더나 푸터 등의 공통 레이아웃이 없는 페이지도 있을 수 있으니, 다음과 같이 `빈 레이아웃(EmptyLayout.vue)`도 추가하겠습니다.

`[/src/routes/layouts/EmptyLayout.vue]`
```vue
<template>
  <slot />
</template>
```
<br/>

[[TOP]](#index)

---
### 라우터 객체의 속성 지정

페이지에서 어떤 레이아웃을 사용할 것인지는 다음과 같이 라우트 객체의 `meta.layout 속성`에서 지정합니다.<br/>
다만 이 과정에서는 기본 레이아웃만 사용할 것이므로 따로 명시하지 않습니다.<br/>
<br/>

`[/src/routes/index.ts]`
```ts
import { createRouter, createWebHistory } from 'vue-router'
import HomePage from './pages/HomePage.vue'
import AboutPage from './pages/AboutPage.vue'

const router = createRouter({
  history: createWebHistory(),
  routes: [
    {
      path: '/',
      component: HomePage,
      meta: {
        layout: 'Default'
      }
    },
    {
      path: '/about',
      component: AboutPage,
      meta: {
        layout: 'Empty'
      }
    }
  ]
})

export default router
```
<br/>

[[TOP]](#index)

---
### 레이아웃 제공자 추가

이제 각 페이지가 레이아웃 안에서 출력되도록 `레이아웃 제공자(LayoutProvider.vue)`를 추가하겠습니다.<br/>
<br/>
프로젝트에서 사용할 레이아웃을 목록(layouts)으로 정의하고, meta.layout 속성 값에 맞게 레이아웃을 출력하도록 동적 컴포넌트(`<Component>`)를 사용합니다.<br/>
현재 라우트 정보는 컴포넌트에서 `useRoute()` 훅을 호출해 얻을 수 있고, `meta.layout` 속성이 없으면 기본 레이아웃(DefaultLayout.vue)을 사용하도록 작성합니다.<br/>
<br/>
혹시 meta.layout 속성에서 정의된 레이아웃 이름(`Default`나 `Empty`)이 아닌 잘못된 이름이 사용되지 않도록 라우터 인터페이스(`RouteMeta`)를 확장하면 안전한 타입으로 관리할 수 있습니다.<br/>
<br/>

`[/src/routes/layouts/LayoutProvider.vue]`
```vue
<script setup lang="ts">
import { RouterView, useRoute } from 'vue-router'
import Default from './DefaultLayout.vue'
import Empty from './EmptyLayout.vue'

// 라우터 인터페이스 확장
declare module 'vue-router' {
  interface RouteMeta {
    layout?: keyof typeof layouts
  }
}

// 사용할 레이아웃 목록 지정
const layouts = {
  Default,
  Empty
} as const

// 현재 라우트 객체(정보) 가져오기
const route = useRoute()
</script>

<template>
  <!-- 동적 레이아웃 출력 및 기본 레이아웃 지정 -->
  <Component :is="layouts[route.meta.layout || 'Default']">
    <RouterView />
  </Component>
</template>
```
<br/>

마지막으로, 생성한 레이아웃 제공자 컴포넌트(`<LayoutProvider>`)로 페이지 출력 위치를 감싸면 이제 모든 페이지가 레이아웃을 거쳐 출력됩니다.<br/>
<br/>

`[/src/App.vue]`
```vue
<script setup lang="ts">
import LayoutProvider from '@/routes/layouts/LayoutProvider.vue'
</script>

<template>
  <LayoutProvider>
    <RouterView />
  </LayoutProvider>
</template>
```
<br/>

[[TOP]](#index)

---