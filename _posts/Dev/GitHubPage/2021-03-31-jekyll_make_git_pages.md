---
layout: post
title: Jekyll GitHub Page 제작하기
categories: GitHubPage

---

Jekyll을 사용한 github page 제작하기

먼저 

아래 그림과같이 Repositories name 에 username.github.io 형태로 입력해주세요

### <img src="https://snaklad.github.io/assets/img/GitHubPage/2021-03-31-jekyll_make_git_pages-1.png" width="600" height="300"> 

다음 Set up in Desktop을 통해서 Clone하고 Clone해준 경로에 미리 받아놓은 테마파일을 붙여넣기한후 Ruby를 통해   `chcp65001` 실행

```
C:\>chcp65001
```

다음 Clone 한 경로로 이동하여 gem 명령어를 통하여 필요한 라이브러리를 설치했습니다.

````
C:\>cd C:\Run-Giraffe.github.io
gem install bundler jekyll minima jekyll-feed tzinfo-data rdiscount
````

다음으로 repositories에 아래 코드와 같이 초기화합니다( 저는 Clone한 경로에서 설치된 파일을 상위폴더로 잘라놓기 하였습니다)

```
C:\Run-Giraffe.github.io>jekyll new Run-Giraffe.github.io
```

이제 다음과 같은 명령어로 jekyll 서버를 구동하고

```
C:\Run-Giraffe.github.io>bundle exec jekyll serve
```

`http://127.0.0.1:4000/`로이동하여 작동을 확인해줍니다.

<img src="https://snaklad.github.io/assets/img/GitHubPage/2021-03-31-jekyll_make_git_pages-2.png" width="600" height="300"> 다음으로 Github DeskTop을 통하여 원격저장소에 저장을 합니다.







