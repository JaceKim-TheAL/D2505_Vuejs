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
    <td><a href="sect_07.md"> [없는페이지]    </a></td>  
    <td><b href="sect_08.md"> [네비게이션]   </b></td>  
    <td><a href="sect_09.md"> [페이지전환]   </a></td>  
    <td><a href="sect_10.md"> [배포]        </a></td>  
  </tr>
</table>

---
# 8. 네비게이션 가드
- [네비게이션 가드](#네비게이션-가드)
- [로그인 페이지 작성](#로그인-페이지-작성)
- [라우트 객체로 등록](#라우트-객체로-등록)
- [네비게이션 가드 작성](#네비게이션-가드-작성)

---
### 네비게이션 가드

네비게이션 가드는 페이지 이동 전에 특정 조건 여부에 맞게 이동을 취소하거나 다른 경로로 리다이렉트할 수 있는 기능을 말합니다.<br/>
주로 로그인 등의 사용자 인증 여부를 확인하는 용도로 사용합니다.<br/>

```shell
├─src/
│  ├─components/
│  │  └─TheHeader.vue
│  ├─routes/
│  │  ├─guards/
│  │  │  ├─guestOnly.ts
│  │  │  ├─requiresAuth.ts
│  │  │  └─index.ts
│  │  ├─layouts/
│  │  │  ├─DefaultLayout.vue
│  │  │  ├─EmptyLayout.vue
│  │  │  └─LayoutProvider.vue
│  │  ├─pages/
│  │  │  ├─AboutPage.vue
│  │  │  ├─HomePage.vue
│  │  │  ├─MovieDetailsPage.vue
│  │  │  ├─MoviesPage.vue
│  │  │  ├─NotFoundPage.vue
│  │  │  └─SignInPage.vue
│  │  └─index.ts
|  ├─App.vue
│  └─main.ts
```
<br/>

[[TOP]](#index)

---
### 로그인 페이지 작성

먼저 로그인 페이지(`SignInPage.vue`)를 작성합니다.<br/>
테스트를 위해, 사용자가 아이디(이메일)와 비밀번호를 모두 입력하면 로그인 성공으로 처리하고 접근 토큰(`accessToken`)을 로컬 스토리지에 저장합니다.<br/>
<br/>

`[/src/routes/pages/SignInPage.vue]`
```vue
<script setup lang="ts">
import { useRoute, useRouter } from 'vue-router'

const route = useRoute()
const router = useRouter()

function handleSubmit(event: Event) {
  // `<form>`의 데이터를 가져와 사용하기 쉽게 객체로 변환합니다.
  const formData = new FormData(event.target as HTMLFormElement)
  const { email, password } = Object.fromEntries(formData) as Record<string, string>
  // 로그인 정보가 모두 있으면, 임시로 로그인 처리합니다.
  if (email && password) {
    localStorage.setItem('accessToken', 'abcd1234')
    const redirectTo = (route.query.redirectTo as string) || '/'
    router.push(redirectTo)
  }
}
</script>

<template>
  <div>
    <h1>Sign In page!</h1>
    <form @submit.prevent="handleSubmit">
      <input
        name="email"
        type="email"
        placeholder="Email" />
      <input
        name="password"
        type="password"
        placeholder="Password" />
      <button type="submit">로그인</button>
    </form>
  </div>
</template>
```
<br/>

[[TOP]](#index)

---
### 라우트 객체로 등록

로그인 페이지를 라우트 객체로 등록합니다.<br/>
이때 라우트 객체의 meta.guestOnly 속성을 지정해서 로그인 페이지에는 로그인하지 않은 사용자만 접근할 수 있도록 합니다.<br/>
반대로 영화 검색 및 상세 정보 페이지는 로그인한 사용자만 접근할 수 있도록 meta.auth 속성을 지정합니다.<br/>
<br/>

`[/src/routes/index.ts]`
```ts
// ...

const router = createRouter({
  // ...
  routes: [
    {
      name: 'SignIn',
      path: '/signin',
      component: SignInPage,
      meta: {
        guestOnly: true
      }
    },
    {
      name: 'Movies',
      path: '/movies',
      component: MoviesPage,
      meta: {
        auth: true
      },
      children: [
        // ...
      ]
    }
    // ...
  ]
})

export default router
```
<br/>

[[TOP]](#index)

---
### 네비게이션 가드 작성

앞서 지정한 `meta` 속성의 값에 따라 동작할 네비게이션 가드를 작성합니다.<br/>
<br/>

`requiresAuth`는 로그인한 사용자만 접근할 수 있도록 하는 가드이고, `guestOnly`는 로그인하지 않은 사용자만 접근할 수 있도록 하는 가드입니다.<br/>
<br/>

`[/src/routes/guards/requiresAuth.ts]`
```ts
import type { RouteGuard } from '.'

export const requiresAuth: RouteGuard = {
  guard(to) {
    if (to.meta.auth) {
      const token = localStorage.getItem('accessToken')
      // 토큰이 유효한지 확인!
      if (!token) {
        return false
      }
    }
    return true
  },
  redirect: '/signin'
}
```
<br/>

`[/src/routes/guards/guestOnly.ts]`
```ts
import type { RouteGuard } from '.'

export const guestOnly: RouteGuard = {
  guard(to) {
    if (to.meta.noAuth) {
      const token = localStorage.getItem('accessToken')
      // 유효 토큰이 있는지 확인!
      if (token) {
        return false
      }
    }
    return true
  },
  redirect: '/'
}
```
<br/>

이제 작성한 내비게이션 가드를 적용해 봅시다.<br/>
router.beforeEach 메서드의 콜백은 모든 페이지의 접근 직전에 호출되며, 매개변수 to는 이동할 페이지의 라우트 객체입니다.<br/>
이를 통해 각 가드에서 라우트 객체의 meta 속성의 값을 확인할 수 있습니다.<br/>
<br/>

`[/src/routes/guards/index.ts]`
```ts
import type { RouteLocationNormalizedGeneric } from 'vue-router'
import router from '@/routes'
import { requiresAuth } from './requiresAuth'
import { guestOnly } from './requiresNoAuth'

router.beforeEach(to => {
  if (!requiresAuth.guard(to)) return requiresAuth.redirect
  if (!guestOnly.guard(to)) return guestOnly.redirect
  return true
})

export interface RouteGuard {
  guard(to: RouteLocationNormalizedGeneric): boolean
  redirect?: string
}
```
<br/>

[[TOP]](#index)

---