[vuejs]: readme.md
[![적용기술](https://skillicons.dev/icons?i=vue,vercel,ts,vscode)][vuejs]

### INDEX

<table>
  <tr>
    <td><a href="sect_01.md"> [설치]        </a></td>
    <td><a href="sect_02.md"> [레이아웃]    </a></td>
    <td><a href="sect_03.md"> [스크롤]      </a></td>
    <td><a href="sect_04.md"> [탐색]        </a></td>
    <td><b href="sect_05.md"> [동적경로]    </b></td>
    <td><a href="sect_06.md"> [중첩경로]     </a></td>
    <td><a href="sect_07.md"> [없는페이지]    </a></td>  
    <td><a href="sect_08.md"> [네비게이션]   </a></td>  
    <td><a href="sect_09.md"> [페이지전환]   </a></td>  
    <td><a href="sect_10.md"> [배포]        </a></td>  
  </tr>
</table>

---
# 5. 동적 경로 일치
- [상세페이지 작성](#상세페이지-작성) 
- [접근 경로 지정](#접근-경로-지정)
- [네비게이션 버튼 추가](#네비게이션-버튼-추가)

---
### 상세페이지 작성

동적 경로(Dynamic Routes)는 URL의 일부를 변수처럼 활용할 수 있는 편리한 기능입니다.<br/>
이를 통해 단일 라우트 컴포넌트로 다양한 페이지를 동적으로 관리할 수 있어 재사용성과 유연성이 크게 증가합니다.<br/>
<br/>
예를 들어, 영화의 상세 정보를 표시하는 페이지를 모두 개별적으로 만들지 않고 하나의 컴포넌트로 모든 영화의 상세 정보를 효과적으로 표시할 수 있습니다.<br/>
아래와 같이 동적 경로를 적용해 봅시다.<br/>

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
│  │  │  └─MovieDetailsPage.vue
│  │  └─index.ts
|  ├─App.vue
│  └─main.ts
```
<br/>

먼저 영화 상세 정보를 표시할 페이지(MovieDetailsPage.vue)를 작성합니다.<br/>
useRoute() 훅을 호출해 얻은 라우트 정보의 params 속성에서 movieId라는 이름의 동적 경로 정보를 얻습니다.<br/>
그리고 이 movieId를 사용해 API 요청으로 영화 정보를 가져오고 출력합니다.<br/>

`[/src/routes/pages/MovieDetailsPage.vue]`
```vue
<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { useRoute } from 'vue-router'

interface Movie {
  imdbID: string
  Title: string
  Poster: string
}

const route = useRoute()
const { movieId } = route.params
const movie = ref<Movie | null>(null)

async function fetchMovieDetails() {
  const res = await fetch(`https://omdbapi.com/?apikey=7035c60c&i=${movieId}`)
  movie.value = await res.json()
}

onMounted(() => {
  fetchMovieDetails()
})
</script>

<template>
  <template v-if="movie">
    <h1>{{ movie.Title }}</h1>
    <img
      :src="movie.Poster"
      :alt="movie.Title" />
  </template>
</template>
```

[[TOP]](#index)

---
### 접근 경로 지정

영화 상세 정보 페이지를 작성했으니 접근 경로를 지정합니다.<br/>
라우트 객체의 `path` 속성에 `'/movies/:movieId'`와 같이 : 기호를 이용해서 동적으로 경로를 일치시키는 변수(movieId)를 지정합니다.<br/>

`[/src/routes/index.ts]`
```ts
// ...
import MovieDetailsPage from './pages/MovieDetailsPage.vue'

const router = createRouter({
  // ...
  routes: [
    // ...
    {
      name: 'MovieDetails',
      path: '/movies/:movieId',
      component: MovieDetailsPage
    }
  ]
})

export default router
```
<br/>

[[TOP]](#index)

---
### 네비게이션 버튼 추가

준비된 페이지로 이동할 수 있도록 헤더에 네비게이션 버튼을 추가합니다.<br/>
다음 예시에 작성된 이동 경로의 tt4154796는 'Avengers: Endgame(2019)' 영화의 ID입니다.<br/>

`[/src/components/TheHeader.vue]`
```vue
<script setup lang="ts">
import { RouterLink } from 'vue-router'
</script>

<template>
  <header>
    <nav>
      <RouterLink to="/">Home</RouterLink>
      <RouterLink to="/about">About</RouterLink>
      <RouterLink to="/movies/tt4154796">Movie(Avengers: Endgame)</RouterLink>
    </nav>
  </header>
</template>
```
<br/>

[[TOP]](#index)

---