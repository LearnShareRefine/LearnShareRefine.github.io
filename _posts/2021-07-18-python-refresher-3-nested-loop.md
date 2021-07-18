---
layout: post
title: Python Refresher 3 (Nested For Loop & More Exercises)
categories: Python
published: true
---

### Learning Objective
Practice makes perfect! Challenge myself with more complicated problems!

### Result
Did some practice exercises on nested For Loop with real-world data
Oh, also printed a tree!

#### Practice 1
![Sales comm q](https://user-images.githubusercontent.com/85727619/126063578-06eba304-1e1a-49d7-946f-6972633d160f.jpg)
![Sales comm a](https://user-images.githubusercontent.com/85727619/126063579-aa8787d2-2ea3-4db6-841c-43ce8e7e1d85.jpg)

#### Practice 2
![q2](https://user-images.githubusercontent.com/85727619/126063589-3f37f1ca-1bc1-4423-ba78-4ba822db5bfd.jpg)

#### Practice 3
![q3](https://user-images.githubusercontent.com/85727619/126063641-f2831cf5-5632-483d-aa8f-5e41b6fe356d.jpg)

#### Practice 4 - Tree!
![q4](https://user-images.githubusercontent.com/85727619/126063664-f909d559-09c8-44a0-88be-9b591a9ce4ee.jpg)


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
