---
layout: home
title: About

---

<h6 style="margin:60px 0px 50px 0px; padding-bottom:10px; font-size:45px; font-family:'Jua', serif; font-weight:bold; letter-spacing: 1px; border-bottom:2px solid #eeeeee">이 블로그는......　　　　　　　　　　　　　</h6>

> >  공부한 내용을 정리하고 간단한 메모를 하기위해 만들어진 블로그입니다.

+ 개인적으로 공부한 내용을 정리할 블로그입니다.   

+ +메모?

<div style="padding-bottom:2rem">
  <h6 style="margin:100px 0px 50px 0px; padding-bottom:10px; font-size:45px; font-family:'Jua', serif; font-weight:bold; letter-spacing: 1px; border-bottom:2px solid #eeeeee">posts　　　　　　　　　　　　　　　　</h6>
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
