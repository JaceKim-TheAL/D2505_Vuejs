[vuejs]: readme.md
[![적용기술](https://skillicons.dev/icons?i=vue,vercel,ts,vscode)][vuejs]

### INDEX

<table>
  <tr>
    <td><a href="sect_01.md"> [설치]        </a></td>
    <td><a href="sect_02.md"> [레이아웃]    </a></td>
    <td><b href="sect_03.md"> [스크롤]      </b></td>
    <td><a href="sect_04.md"> [탐색]        </a></td>
    <td><a href="sect_05.md"> [동적경로]    </a></td>
    <td><a href="sect_06.md"> [중첩경로]     </a></td>
    <td><a href="sect_07.md"> [없는페이지]    </a></td>  
    <td><a href="sect_08.md"> [네비게이션]   </a></td>  
    <td><a href="sect_09.md"> [페이지전환]   </a></td>  
    <td><a href="sect_10.md"> [배포]        </a></td>  
  </tr>
</table>

---
# 3. 스크롤 복원

사용자가 뒤로 혹은 앞으로 가기를 할 때 페이지의 스크롤 위치를 복원하거나, 새로운 페이지의 스크롤 위치를 최상단으로 이동시켜 사용자에게 더 나은 페이지 탐색 경험을 제공할 수 있습니다.<br/>
`createRouter` 함수의 `scrollBehavior` 옵션에서 스크롤 위치를 처리할 수 있습니다.<br/>

`[/src/routes/index.ts]`
```ts
// ...

const router = createRouter({
  history: createWebHistory(),
  scrollBehavior(to, from, savedPosition) {
    if (savedPosition) return savedPosition
    return { top: 0, left: 0 }
  },
  routes: [
    // ...
  ]
})

export default router
```
<br/>

[[TOP]](#index)

---