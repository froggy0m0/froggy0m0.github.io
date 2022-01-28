---
layout: post
title: Jekyll GitHub Page 제작하기
categories: GitHubPage

---

Jekyll을 사용한 github page 제작하기

먼저 

아래 Fig 1과 같이 Repositories name 에 username.github.io 형태로 입력해 주세요

<figure>
    <img src="https://Froggy0m0.github.io/assets/img/GitHubPage/2021-03-31-jekyll_make_git_pages-1.png" width="600" height="300">
    <figcaption>Fig 1. 블로그이름 설정</figcaption>
</figure> 

다음 Set up in Desktop을 통해서 Clone하고 Clone해준 경로에 미리 받아놓은 테마 파일을 붙여넣은후 이후 설치된 __Start Command Prompt with Ruby__ 를 통해   `chcp65001` 실행

<figure>
    <img src="https://Froggy0m0.github.io/assets/img/GitHubPage/2021-03-31-jekyll_make_git_pages-2.png">
    <figcaption>Fig 2. chcp 65001</figcaption>
</figure>


이후 Fig3과 같이 Clone 한 경로(블로그 경로)로 이동하여 gem 명령어를 통하여 필요한 라이브러리를 설치했습니다.

<figure>
    <img src="https://Froggy0m0.github.io/assets/img/GitHubPage/2021-03-31-jekyll_make_git_pages-3.png">
    <figcaption>Fig 3. 라이브러리 설치</figcaption>
</figure>

````
gem install bundler jekyll minima jekyll-feed tzinfo-data rdiscount
````

다음으로 repositories에 아래의 예시와 같이 초기화합니다

※username은 자신의 블로그 명으로 변경해야 됩니다.※

<figure>
    <img src="https://Froggy0m0.github.io/assets/img/GitHubPage/2021-03-31-jekyll_make_git_pages-3.png">
    <figcaption>Fig 3. 라이브러리 설치</figcaption>
</figure>

```
jekyll new username.github.io 
```

이제 다음과 같은 명령어로 jekyll 서버를 구동하고

```
bundle exec jekyll serve
```

`http://127.0.0.1:4000/`이동하여 작동을 확인해 줍니다.

<img src="https://Froggy0m0.github.io/assets/img/GitHubPage/2021-03-31-jekyll_make_git_pages-4.png" width="600" height="300"> 이후 cmd를 사용하여 clone한 폴더로 이동하여`bundle exec jekyll serve` 명령어를 사용하여 로컬에서 자신의 블로그를 확인할 수 있습니다.

이후 파일 업로드는 Git, Git Desktop등을 사용하여 원격 저장소에 저장을 합니다.







