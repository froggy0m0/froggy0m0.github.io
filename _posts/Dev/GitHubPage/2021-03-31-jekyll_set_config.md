---
layout: post
title: Jekyll 디렉터리 구조와 기본설정
categories: GitHubPage
---

---



# jekyll 디렉터리 구조

현재 저의 블로그 디렉토리는 아래와 같습니다. 

```
.
├── _config.yml
├── _includes
│   ├── footer.html
│   └── header.html
├── _layouts
│   ├── default.html
│   └── post.html
├── _posts
│   ├── 2021-03-31-first_posting.md
│   └── 2021-03-31-second_posting.md
├── categories
│   ├── GitHubPage.md
│   └── Python.md
├── public
│   ├── css
│      ├── layon.css
│   ├── js
│   └── favicon.ico
├── about.md
└── index.html # 올바른 머리말을 가진 'index.md' 도 가능
```

`_sass` 등 다양한 디렉토리가 존재하지만 현재 디렉토리가 존재하지만 현재가지고있는 내용만 작성하였습니다.



* _config.yml : 블로그운영에서 사용되는 환경변수들의 정보를 담고있습니다.
* _includes : 재사용하기위한 파일을 담는 디렉토리, 필요한 포스트의 레이아웃을 사용할 수 있습니다.
* _posts : 포스팅될 파일을 담는 디렉토리,보통 파일의 이름은 YEAR-MONTH-DAY-title.md 형식으로 정의됩니다
* categories : 카테고리로 사용될 파일들을 담고있습니다.
* pulic/css/ : css를 사용하기위해 있는것 같습니다.
* favicon.ico : 브라우저창에 표시되는 대표 아이콘
* about.md : 사이드바의 About 의 블로그소개글