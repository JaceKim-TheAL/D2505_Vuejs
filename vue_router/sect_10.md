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
    <td><a href="sect_09.md"> [페이지전환]   </a></td>  
    <td><b href="sect_10.md"> [배포]        </b></td>  
  </tr>
</table>

---
# 10. 배포
- [Vercel](#vercel) 
- [Netlify](#netlify) 
- [Firebase](#firebase)

---

HTML5 Mode(`createWebHistory`)를 사용하는 Vue Router 프로젝트를 배포할 때는, 모든 요청을 `index.html`로 리다이렉트해야 하는 서버 설정이 필요합니다.<br/>
이를 통해 사용자가 직접 URL을 입력해 특정 페이지에 접근하거나 페이지를 새로고침해도 애플리케이션이 정상적으로 작동하도록 만들어야 합니다.<br/>
간단하게 배포할 수 있는 주요 호스팅 서비스별로 필요한 설정을 살펴봅시다.<br/>
<br/>

### Vercel

Vercel 서비스로 배포하는 경우, 프로젝트의 루트 경로에 vercel.json 파일을 생성하고 다음 구성을 추가합니다.<br/>
<br/>

`[/vercel.json]`
```json
{
  "rewrites": [{ "source": "/:path*", "destination": "/index.html" }]
}
```
<br/>

[[TOP]](#index)

---
### Netlify

Netlify 서비스로 배포하는 경우, 프로젝트의 공개 폴더(/public) 경로에 _redirects 파일을 생성하고 다음 구성을 추가합니다.<br/>

`[/public/_redirects]`
```
/*  /index.html  200
```
<br/>

또는 프로젝트의 루트 경로에 netlify.toml 파일을 생성하고 다음 구성을 추가합니다.<br/>
<br/>

`[/netlify.toml]`
```toml
[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```
<br/>

[[TOP]](#index)

---
### Firebase

Firebase 서비스로 배포하는 경우, 프로젝트의 루트 경로에 firebase.json 파일을 생성하고 다음 구성을 추가합니다.<br/>
<br/>

`[/firebase.json]`
```json
{
  "hosting": {
    "public": "dist",
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}
```
<br/>

[[TOP]](#index)

---