[vuejs]: readme.md
[![적용기술](https://skillicons.dev/icons?i=vue,vercel,ts,vscode)][vuejs]

### INDEX

<table>
  <tr>
    <td><b href="sect_01.md"> [설치]        </b></td>
    <td><a href="sect_02.md"> [레이아웃]    </a></td>
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
# 1. 설치 및 구성
- [vue-router 설치](#vue-router-설치) 
- [컴포넌트 추가](#컴포넌트-추가)
- [라우터 구성](#라우터-구성)
- [플러그인 등록](#플러그인-등록)
- [컴포넌트 지정](#컴포넌트-지정)

---
### Vue Router 설치
다음과 같이 `vue-router`를 설치합니다.

```shell
npm i vue-router
```
<br/>

다음의 폴더 및 파일 구조로 시작합니다.
```shell
├─src/
│  ├─routes/
│  │  ├─pages/
│  │  │  ├─AboutPage.vue
│  │  │  └─HomePage.vue
│  │  └─index.ts
|  ├─App.vue
│  └─main.ts
```
<br/>

[[TOP]](#index)

---
### 컴포넌트 추가
`/src/routes/pages` 폴더에 사용할 페이지 컴포넌트를 추가합니다. <br/>
우선 간단하게 Home과 About 페이지를 추가하겠습니다.<br/>

`[/src/routes/pages/HomePage.vue]`
```vue
<template>
  <h1>Home page!</h1>
</template>
```
<br/>

`[/src/routes/pages/AboutPage.vue]`
```vue
<template>
  <h1>About page!</h1>
</template>
```
<br/>

[[TOP]](#index)

---
### 라우터 구성
보여줄 페이지를 추가했다면 이제 /src/routes/index.ts 파일에서 프로젝트의 라우터를 구성합니다. <br/>
<br/>
createRouter 함수를 사용해 라우터를 생성합니다.<br/>
이때 history 옵션에는 라우팅 모드를 지정하고, routes 옵션에는 라우트 객체(정보)를 배열로 전달합니다.<br/>
각 라우트 객체는 path 속성에 접근 경로를, component 속성에 경로가 일치할 때 표시할 컴포넌트를 지정합니다.<br/>
즉, 다음 예제에서 사용자는 /, /about 주소로 접근할 수 있습니다.<br/>
<br/>
그리고 createRouter 함수로 생성한 라우터 객체(router)를 외부에서 사용할 수 있도록 내보냅니다.<br/>

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
      component: HomePage
    },
    {
      path: '/about',
      component: AboutPage
    }
  ]
})

export default router
```
<br/>

[[TOP]](#index)

---
### 플러그인 등록
이제 `/src/main.ts`로 이동해 생성된 라우터 객체를 가져와 프로젝트의 플러그인으로 등록합니다.

`[/src/main.ts]`
```ts
import { createApp } from 'vue'
import App from './App.vue'
import router from './routes'

createApp(App)
  .use(router)
  .mount('#app')
```
<br/>

[[TOP]](#index)

---
### 컴포넌트 지정
마지막으로 프로젝트의 최상위 컴포넌트(`/src/App.vue`)에서 사용자의 접근 경로에 맞게 각 페이지가 출력될 위치를 `<RouterView />` 컴포넌트로 지정합니다.

`[/src/App.vue]`
```vue
<script setup lang="ts">
import { RouterView } from 'vue-router'
</script>

<template>
  <RouterView />
</template>
```
<br/>

[[TOP]](#index)

---