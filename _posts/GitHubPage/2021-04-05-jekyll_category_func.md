---
layout: post
title: Jekyll 카테고리기능(제목정렬, 페이지설명)
categories: GitHubPage
message: 날짜순으로 정렬된 페이지들을 제목정렬과 해당 페이지에대한 간략한 설명을 추가했다.

---

## 1. 페이지 제목정렬

<img src="https://run-giraffe.github.io/assets/img/GitHubPage/2021-04-05-jekyll_category_func-1.png"> 

```html
{% raw %}
<ul class="posts-list">
  
  {% assign category = page.category | default: page.title %}
    <!-- 코드 내용 -->
  {% for post in site.categories[category] %}
    <li>
      <h3>
        <a href="{{ site.baseurl }}{{ post.url }}">
          {{ post.title }}
        </a>
        <small class="font-date">
            {{ post.date | date : "%Y.%m.%d" }}</small>
      </h3>
    </li>
  {% endfor %}
  
</ul>
{% endraw %}
```

기존 post들이 sort되는 코드와 출력화면이다.

`중괄호 하나로 이루어진 문장들은 중괄호안에 % 기호를 붙여야 정상으로 작동된다.`

이후에 날짜로 정렬되는 포스팅들을 for문 바로위에

`{% raw %}{% assign sort_page = site.categories[category] | sort: "title" %}{% endraw %}` 

문장을넣어 title을 넣어 title로 정렬을하고

``for문은 위에서만들어진 sort_page를 사용해서 돌리면된다.``

for문으로 sort_page변수에서 가져오게 수정하였다.

`다른 정렬값을 사용하고싶다면 해당 포스트의 YAML 의 변수를 활용하면될것같다 `.

결과내용

-> 정렬은 아스키코드값을 기준으로 정렬되는것으로 보인다.

<img src="https://run-giraffe.github.io/assets/img/GitHubPage/2021-04-05-jekyll_category_func-2.png">    

​        

​    

## 2.포스트 말머리

포스트에 말머리(개요?)을 따로 메시지형태로 나타내보았다.

``` html
  <p class ="post-message">
   {% raw %} {{ page.message }} {% endraw %}
  </p>
```

post-message는 따로 정의한 css style내용이고

{% raw %} `{{ page.message }}` {% endraw %}는 페이지 YAML에서 message변수를 추가하여 그곳에 머릿말을 추가했다.

<img src="https://run-giraffe.github.io/assets/img/GitHubPage/2021-04-05-jekyll_category_func-3.png">