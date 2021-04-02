---
layout: post
title: Jekyll sass사용하기 (css style)
categories: GitHubPage
---





사이드바의 레이아웃을 설정하는도중에 css의설정이필요해서 관리가 더편할 것같은 Sass설정하기로했다.

# 1. Sass Stylesheet 생성

먼저 루트디렉토리에 `css` 디렉토리를 생성하여 `.scss` 의확장자 파일을 다음과같은 형태로 저장되었다. (YAML의 내용이없더라도 꼭 작성되야된다.)

ex) style.scss

```
---

---
```



# 2. StyleSheet 링크넣기

StyleSheet를생성한후 Jekyll 서버를 구동하면 `styles.css`파일이 `_site/css` 디렉토리에 생성된다.



`_layout` 디렉토리의 `default.html` 즉, 메인레이아웃의 html에 다음과같이 링크를 작성한다

```html
<link rel= "stylesheet" href= "/css/style.css"
```



# 3. scss에 import 하기

`_sass/` 디렉토리에 모듈화된 `.scss`파일을 저장하고 루트디렉토리의 `css/style.scss` 에 import해준다.



ex) `_sass/` 디렉토리에 `abc.scss` `test.scss` 를 생성했다면 `css/style.scss`에서 다음과같이 import해주면된다

```
---

---

@import "abc";
@import "test";
```





<br>

REFERENCE : <https://kgmyh.github.io/blog/2017/12/23/Jekyll_Using_Sass/>

