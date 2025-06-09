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
    <td><a href="sect_08.md"> [네비게이션]   </a></td>  
    <td><b href="sect_09.md"> [페이지전환]   </b></td>  
    <td><a href="sect_10.md"> [배포]        </a></td>  
  </tr>
</table>

---
# 9. 페이지 전환 애니메이션

Vue `<Transition>` 컴포넌트를 사용하면, 페이지가 바뀔 때마다의 전환 애니메이션을 쉽게 추가할 수 있습니다.<br/>
0.3초에 걸쳐 기존 페이지가 사라지고 새 페이지가 나타나는 애니메이션을 추가합니다.<br/>

<pre>
ⓦ 전환 애니메이션이 정상적으로 적용되려면, <RouterView />로 출력되는 페이지 컴포넌트의 최상위 요소가 단일 요소여야 합니다.
</pre>
<br/>

`[/src/routes/layouts/LayoutProvider.vue]`
```vue
<script setup lang="ts">
import { RouterView, useRoute } from 'vue-router'
import Default from './DefaultLayout.vue'
import Empty from './EmptyLayout.vue'

declare module 'vue-router' {
  interface RouteMeta {
    layout?: keyof typeof layouts
  }
}

const layouts = {
  Default,
  Empty
} as const

const route = useRoute()
</script>

<template>
  <Component :is="layouts[route.meta.layout || 'Default']">
    <Transition
      name="fade"
      mode="out-in">
      <RouterView />
    </Transition>
  </Component>
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 3s;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  position: absolute;
}
.fade-enter-to,
.fade-leave-from {
  opacity: 1;
}
</style>
```
<br/>

[[TOP]](#index)

---