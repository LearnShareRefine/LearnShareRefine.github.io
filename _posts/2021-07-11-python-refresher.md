---
layout: post
title: Python Refresher 1 (Basic Iteration)
categories: Python
published: true
---

### Learning Objective
To refresh and practice on Python codes, starting from the real basic!
I have done a few Python courses and projects in 2019 & 2020 but ever since then I didn't touch any of those codes. It's time to revisit them!

### Result
I revisited some of the Python basics, did some exercises and collated a reference note in Jupyter Notebook.

#### Basic Operations
![Python1](https://user-images.githubusercontent.com/85727619/125202363-e25a9300-e2a5-11eb-8b8f-f9103788b5c0.jpg)

#### String Slicing
![Python2](https://user-images.githubusercontent.com/85727619/125202367-e5ee1a00-e2a5-11eb-8f25-e2666a87b14a.jpg)
![Python3](https://user-images.githubusercontent.com/85727619/125202370-ebe3fb00-e2a5-11eb-9907-7301410d860d.jpg)

#### List Operation
![Python4](https://user-images.githubusercontent.com/85727619/125202378-f3a39f80-e2a5-11eb-94e3-24e19bdeb4df.jpg)
![Python5](https://user-images.githubusercontent.com/85727619/125202403-04541580-e2a6-11eb-8f44-9d258195bdb2.jpg)

#### Dictionary
![Python6](https://user-images.githubusercontent.com/85727619/125202408-07e79c80-e2a6-11eb-866e-6f959956e86c.jpg)

#### If, elif, else
![Python7](https://user-images.githubusercontent.com/85727619/125202419-15048b80-e2a6-11eb-9181-873822190946.jpg)
![Python8](https://user-images.githubusercontent.com/85727619/125202421-1635b880-e2a6-11eb-8723-e67a894c20f0.jpg)

Revisiting the basic is more interesting that I thought. It definitely helps me to build a strong foundation and get used to the codes again after so long. 
Gonna find more time to practise!

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
