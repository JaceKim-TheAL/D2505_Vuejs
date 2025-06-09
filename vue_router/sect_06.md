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
    <td><b href="sect_06.md"> [중첩경로]     </b></td>
    <td><a href="sect_07.md"> [없는페이지]    </a></td>  
    <td><a href="sect_08.md"> [네비게이션]   </a></td>  
    <td><a href="sect_09.md"> [페이지전환]   </a></td>  
    <td><a href="sect_10.md"> [배포]        </a></td>  
  </tr>
</table>

---
# 6. 중첩 경로
- [검색페이지 작성](#검색페이지-작성)
- [라우트객체로 등록](#라우트객체로-등록)
- [모달 형태로 표시](#모달-형태로-표시)
- [네비게이션 버튼 추가하고 테스트](#네비게이션-버튼-추가하고-테스트)

---
### 검색페이지 작성

하나의 라우트 컴포넌트 안에서 다른 라우트 컴포넌트를 포함할 수 있습니다.<br/>
<br/>
아래 예제에서는 영화를 검색할 수 있는 페이지를 만들어 검색 결과를 선택해 영화 상세 정보 출력하도록 합니다.<br/>
다만 영화 상세 정보를 별도의 페이지가 아닌 모달 형태로 표시합니다.<br/>

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
│  │  │  └─MoviesPage.vue
│  │  └─index.ts
|  ├─App.vue
│  └─main.ts
```
<br/>

먼저 영화 검색 페이지(`MoviesPage.vue`)를 작성합니다.<br/>
검색된 영화 목록에서 원하는 영화를 선택하면 영화 상세 정보 페이지로 이동합니다.<br/>
그리고 `<RouterView />` 컴포넌트를 사용해, 영화 상세 정보 페이지가 검색 페이지의 일부분으로 표시되도록 중첩 경로를 적용합니다.<br/>

`[/src/routes/pages/MoviesPage.vue]`
```vue
<script setup lang="ts">
import { ref } from 'vue'
import { RouterLink } from 'vue-router'

interface Movie {
  imdbID: string
  Title: string
}

const searchTitle = ref('')
const movies = ref<Movie[]>([])

async function fetchMovies() {
  const res = await fetch(
    `https://omdbapi.com/?apikey=7035c60c&s=${searchTitle.value}`
  )
  const { Search } = await res.json()
  movies.value = Search
}
</script>

<template>
  <div>
    <h1>Movies page!</h1>
    <div>
      <input
        v-model="searchTitle"
        @keydown.enter="fetchMovies" />
      <button @click="fetchMovies">검색</button>
    </div>
    <ul>
      <li
        v-for="movie in movies"
        :key="movie.Title">
        <RouterLink :to="`/movies/${movie.imdbID}`">
          {{ movie.Title }}
        </RouterLink>
      </li>
    </ul>
    <RouterView />
  </div>
</template>
```
<br/>

[[TOP]](#index)

---
### 라우트객체로 등록

작성한 영화 검색 페이지를 라우트 객체로 등록합니다.<br/>
이때 앞서 만든 영화 상세 정보 페이지의 라우트 객체를 검색 페이지의 children 속성으로 이동시켜 중첩 경로를 적용하고, path 속성의 경로를 /movies/:movieId에서 :movieId로 수정합니다.<br/>
부모 라우트(Movies)의 경로가 이미 /movies이므로, 자식 라우트(MovieDetails)에서는 /movies를 생략하고 :movieId만 작성하면 됩니다.<br/>
<br/>
이제 위에서 설명한 대로, 영화 상세 정보 페이지는 영화 검색 페이지의 <RouterView /> 컴포넌트 위치에 출력됩니다.<br/>
<br/>

`[/src/routes/index.ts]`
```ts
// ...
import MoviesPage from './pages/MoviesPage.vue'
import MovieDetailsPage from './pages/MovieDetailsPage.vue'

const router = createRouter({
  history: createWebHistory(),
  routes: [
    // ...
    {
      name: 'Movies',
      path: '/movies',
      component: MoviesPage,
      children: [
        {
          name: 'MovieDetails',
          path: ':movieId',
          component: MovieDetailsPage
        }
      ]
    }
  ]
})

export default router
```
<br/>

[[TOP]](#index)

---
### 모달 형태로 표시

이제 영화 상세 정보 페이지를 모달 형태로 표시해 봅시다.<br/>
모달로 표시될 요소 구조와 함께 스타일을 추가합니다.<br/>
<br/>


`[/src/routes/pages/MovieDetailsPage.vue]`
```vue
<script setup lang="ts">
// ...
</script>

<template>
  <div class="modal">
    <div
      class="overlay"
      @click="closeModal" />
    <div class="content">
      <template v-if="movie">
        <h1>{{ movie.Title }}</h1>
        <img
          :src="movie.Poster"
          :alt="movie.Title" />
      </template>
    </div>
  </div>
</template>

<style scoped>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  cursor: pointer;
}
.content {
  position: relative;
  max-width: 500px;
  padding: 20px 30px;
  border-radius: 10px;
  box-shadow: 0 10px 10px rgba(0, 0, 0, 0.1);
  background-color: white;
}
</style>
```
<br/>

[[TOP]](#index)

---
### 네비게이션 버튼 추가하고 테스트

준비가 끝났으니, 영화 검색 페이지로 이동할 수 있는 네비게이션 버튼을 추가하고 테스트해 봅시다.<br/>

`[/src/components/TheHeader.vue]`
```vue
<!-- ... -->

<template>
  <header>
    <nav>
      <RouterLink to="/">Home</RouterLink>
      <RouterLink to="/about">About</RouterLink>
      <RouterLink to="/movies">Movies</RouterLink>
      <RouterLink to="/movies/tt4154796">Movie(Avengers: Endgame)</RouterLink>
    </nav>
  </header>
</template>

<!-- ... -->
```
<br/>

[[TOP]](#index)

---