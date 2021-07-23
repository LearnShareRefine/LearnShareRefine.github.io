---
layout: post
title: Python Refresher 4 (Functions)
categories: Python
published: true
---

### Learning Objective
Training my brain to define various functions!

#### Practice 1 (Variance)
![Function1](https://user-images.githubusercontent.com/85727619/126760974-c17b5255-62d8-4c22-89c9-c5205d404e4a.jpg)

#### Practice 2 (Mode, Mean, Median)
![Function2](https://user-images.githubusercontent.com/85727619/126761006-a393cf00-84f2-4331-9449-1b225d7268fd.jpg)

#### Practice 3 (sort nested list)
![Function3](https://user-images.githubusercontent.com/85727619/126761172-1fd005ed-8f5f-42a0-a826-bf28d4f7d17d.jpg)

#### Practice 4 (Transpose nested list)
![Function4](https://user-images.githubusercontent.com/85727619/126761379-b9cf6bb4-1d4f-4b69-93e9-2bb0b7037679.jpg)

Woohoo, getting to Python libraries next!

<div class="post-categories">
  {% if post %}
    {% assign categories = post.categories %}
  {% else %}
    {% assign categories = page.categories %}
  {% endif %}
  {% for category in categories %}
  <a href="{{site.baseurl}}/categories/#{{category|slugize}}">{{category}}</a>
  {% unless forloop.last %}&nbsp;{% endunless %}
  {% endfor %}
</div>
