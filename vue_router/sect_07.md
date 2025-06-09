[vuejs]: readme.md
[![적용기술](https://skillicons.dev/icons?i=vue,vercel,ts,vscode)][vuejs]

### INDEX

<table>
  <tr>
    <td><a href="sect_01.md"> [설치]        </a></td>
    <td><a href="sect_02.md"> [레이아웃]    </a></td>
    <td><a href="sect_03.md"> [스크롤]      </a></td>
    <td><a href="sect_04.md"> [탐색]        </a></td>
    <td><a href="sect_05.md"> [동적경로]    </a></td>
    <td><a href="sect_06.md"> [중첩경로]     </a></td>
    <td><b href="sect_07.md"> [없는페이지]    </b></td>  
    <td><a href="sect_08.md"> [네비게이션]   </a></td>  
    <td><a href="sect_09.md"> [페이지전환]   </a></td>  
    <td><a href="sect_10.md"> [배포]        </a></td>  
  </tr>
</table>

---
# 7. 찾을 수 없는 페이지

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
│  │  │  ├─HomePage.vue
│  │  │  ├─MovieDetailsPage.vue
│  │  │  ├─MoviesPage.vue
│  │  │  └─NotFoundPage.vue
│  │  └─index.ts
|  ├─App.vue
│  └─main.ts
```
<br/>

사용자가 정의하지 않은 경로로 접근했을 때 표시할 페이지를 만들어 봅시다.<br/>
다음과 같이 NotFoundPage.vue를 작성합니다.<br/>
<br/>

`[/src/routes/pages/NotFoundPage.vue]`
```vue
<template>
  <h1>404 Not Found page!</h1>
</template>
```
<br/>

그리고 모든 경로에 일치하는 라우트 객체를 추가합니다.<br/>
pathMatch 라는 동적 경로 이름으로 모든 하위 경로를 일치시키기 위해 정규식도 같이 작성합니다.<br/>
주의할 점은, 라우트 객체는 모든 라우트 객체의 일치 확인이 끝나는 가장 마지막 위치에 추가해야 합니다.<br/>
<br/>

`[/src/routes/index.ts]`
```ts
// ...
import NotFoundPage from './pages/NotFoundPage.vue'

const router = createRouter({
  // ...
  routes: [
    // ...
    {
      name: 'NotFound',
      path: '/:pathMatch(.*)*',
      component: NotFoundPage
    }
  ]
})

export default router
```
<br/>

[[TOP]](#index)

---