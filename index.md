---
layout: home
title: About
---

## 이 블로그는......

> >  공부한 내용을 정리하고 간단한 메모를 하기위해 만들어진 블로그입니다.

+ 개인적으로 공부한 내용을 정리할 블로그입니다.   

  

+ +메모?


<div class="related">
  <h2>Related posts</h2>
  <ul class="related-posts">
    {% for post in site.categories['Pwnable'] limit:5 %}
      <li>
        <h5>
          <a href="{{ site.baseurl }}{{ post.url }}">
            {{ post.title }}
            <small class="font-date">{{ post.date | date : "%Y.%m.%d" }}</small>
          </a>
        </h5>
      </li>
    {% endfor %}
  </ul>
</div>
