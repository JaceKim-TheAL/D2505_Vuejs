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
- []

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
`/src/routes/pages` 폴더에 사용할 페이지 컴포넌트를 추가합니다. <br/>
우선 간단하게 Home과 About 페이지를 추가하겠습니다.

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


<br/>

[[TOP]](#index)

---