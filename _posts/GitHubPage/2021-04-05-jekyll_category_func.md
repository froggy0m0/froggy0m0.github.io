---
layout: post
title: Jekyll 카테고리기능(제목정렬, 페이지설명)
categories: GitHubPage
message: 해당 페이지에대한 간략한 설명과 날짜순으로 정렬된 페이지들을 제목정렬해보았다.

---

## 1. 페이지 제목정렬

<img src="https://run-giraffe.github.io/assets/img/GitHubPage/2021-04-05-jekyll_category_func-1.png"> 

```html
<ul class="posts-list">
  
  { assign category = page.category | default: page.title }
  { for post in sort_page }
    <li>
      <h3>
        <a href="{{ site.baseurl }}{{ post.url }}">
          {{ post.title }}
        </a>
        <small class="font-date">{{ post.date | date : "%Y.%m.%d" }}</small>
      </h3>
    </li>
  { endfor }
  
</ul>
```

기존 post들이 sort되는 코드와 출력화면이다.



이후에 날짜로 정렬되는 포스팅들을 for문 바로위에

`{ assign sort_page = site.categories[category] | sort: "title" }` 

문장을넣어 title을 기준으로 정렬하게된다.  

`다른 정렬값을 사용하고싶다면 해당 포스트의 YAML 의 변수를 활용하면될것같다 `

<img src="https://run-giraffe.github.io/assets/img/GitHubPage/2021-04-05-jekyll_category_func-2.png" width="600" height="300">    

​          

## 2.페이지에대한 부연설명



