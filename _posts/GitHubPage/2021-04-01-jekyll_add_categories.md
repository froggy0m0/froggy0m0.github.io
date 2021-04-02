---
layout: post
title: Jekyll 카테고리 설정과 사이드바 설정
categories: GitHubPage


---

## 카테고리 설정과 사이드바 설정

1. 카테고리 설정

먼저 _layouts 파일에 `category.html` 파일  을 생성하였다.

```html
---
layout: default
---
<ul class="posts-list">
  
  {% assign category = page.category | default: page.title %}
  {% for post in site.categories[category] %}
    <li>
      <h3>
        <a href="{{ site.baseurl }}{{ post.url }}">
          {{ post.title }}
        </a>
        <small>{{ post.date | date_to_string }}</small>
      </h3>
    </li>
  {% endfor %}
  
</ul>
```

다음으로 categories 파일을 루트디렉터리에 생성하고 `title.md` 파일을 생성해주고 아래와 같이 설정해줬다.

이페이지링크는 `https://username.github.io/categories/Python/` 과 같은 주소가되고

`https://username.github.io/` 부분은 _config.yml 에 저장된 url변수에 의해 사용될것이다

title 과 제목은 동일해야된다

```markdown
---
layout: category
title: Python
permalink: 'categories/Python'
---
```

이제부터 _post에 저장될  `title.md` 에 아래와 같이 설정해준다.

`categories: Python` 으로 포스팅하면  Python으로 분류될것이다.

```markdown
---
layout: post
title: Python
categories: Python
---
테스트용 포스팅 입니다 ~
```



---

2. 사이드바 설정

`_includes` 파일에 `sidear_category_list.html` 이라는 파일을 아래와같이 작성했다.

```html
<div>
  <div class="sidebar-nav-item">Python</div>
  <nav class="content-ul">
    {% for category in site.categories %}
      {% if category[0] == "Python" %}
      <a href="{{ root_url }}/{{ site.category_dir }}#{{ category | first }}"
        class="sidebar-nav-item pl-3rem">
                            <span class="name">
                                {{ category | first }}
                            </span>
        <span class="badge">{{ category | last | size }}</span>
      </a>
      {% endif %}
    {% endfor %}
  </nav>
</div>
```

아래는 사이드바에 위치할 대분류의 제목정도? 될것같다 

```html
<div class="sidebar-nav-item">Python</div>
```



for 루프를 통해 `site.categories` 의 항목들을 `category` 변수에 배치하고

category[0] == category의 title 로접근하여 `Python` 카테고리만 출력하게 만들었다.

> 해당코드에대해서는 자세한건 모르겠지만 for문이 의미없이 돌아가는것 같아서 다른방법을 찾으려다 포기 ㅠ

    {% for category in site.categories %}
      {% if category[0] == "Python" %} 
      {% endif %}
    {% endfor %}

__추가로__

이런식으로 or을 활용해서 중분류 소분류 해서 카테고리를 활용해서 사용할 수 있을 것같다.

```
{% if category[0] == "Python" or 
	  category[0] == "ruby" %}
{% endif %}
```





`site.category_dir` 는 _config.yml 에 저장된 category_dir 변수

`{{ category | first }}`는 for문을 돌면서 if문에 만족하는 category의 이름

즉,`https://username.github.io/categories/Python/` 로 접근될것이다.

`{{ category | last | size }}`는 카테고리에 포함된 포스트 갯수가 나타난다.

```html
  <a href="{{ root_url }}/{{ site.category_dir }}{{ category | first }}"
    class="sidebar-nav-item pl-3rem">
                        <span class="name">
                            {{ category | first }}
                        </span>
    <span class="badge">{{ category | last | size }}</span>
```


다음으로 `_include` 파일의 `sidebar.html` 에 아래와같이 추가해줬다.

`sidebar.html` 에 작성해도 되지만 분류 수정하기 쉽게 따로`sidear_category_list.html`  에 작성하였다.

기존 `sidebar.html`에 include 하였다 
