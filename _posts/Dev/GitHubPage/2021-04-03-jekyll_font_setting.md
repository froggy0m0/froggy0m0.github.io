---
layout: post
title: Jekyll 폰트변경하기
categories: GitHubPage
---



구글에서 제공하는 웹폰트를 사용해서 Github 블로그에 적용해보았다.





먼저 [Google Fonts](https://fonts.google.com/) 에 접속하여 원하는 폰트를 선택한후 아래화면과 같이 `scss`파일에 import 해줄수있는 문장을 얻어왔다.

<img src="https://Froggy0m0.github.io/assets/img/GitHubPage/2021-04-03-jekyll_font_setting-1.png"> 

이후 `scss/style.scss` import하고

구글에서 얻어온 CSS rules to specify families에 있는 문장처럼 아래와 같이 새로운 css를 만들었다.

``` css
.font-basic{
font-family: 'Noto Sans KR', sans-serif;
}
```

