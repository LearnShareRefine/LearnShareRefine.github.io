---
layout: post
title: Python Refresher 2 (For Loop/While Loop)
categories: Python
published: true
---

### Learning Objective
To refresh and practice on Python codes, focusing on loops this time round.

### Result
Did some practice exercises on For Loop and While Loop

#### Basic While Loop
![While Loop1](https://user-images.githubusercontent.com/85727619/126041097-69346aa1-19ae-49c8-8461-8af5b5f5d683.jpg)
![While Loop2](https://user-images.githubusercontent.com/85727619/126041100-ea170fe9-8da9-4f3e-a42e-9ea7a730214f.jpg)

#### Basic For Loop
![For loop 1](https://user-images.githubusercontent.com/85727619/126041116-71c0c380-74a7-4956-87ad-f118a827213a.jpg)
![For loop 2](https://user-images.githubusercontent.com/85727619/126041122-75ef3e30-0e1d-4c0f-9161-70c1f2227f60.jpg)
![For loop 3](https://user-images.githubusercontent.com/85727619/126041159-073d82eb-a7dd-47c1-91a1-c00ff1b3386a.jpg)

#### For Loop Practice Exercises
![For loop 4](https://user-images.githubusercontent.com/85727619/126041137-14382554-cdbf-4ebd-8984-adf7be26937c.jpg)
![For loop 5](https://user-images.githubusercontent.com/85727619/126041163-99111fde-44b9-4bd3-b10f-2face95f075d.jpg)

#### Fizz-Buzz Test
![FizzBuzz](https://user-images.githubusercontent.com/85727619/126041168-39c95448-1a03-47fe-9986-280bdfd86a90.jpg)

I am quite surprised that I can still remember most of the things I have learnt before and can now improve on the codes I have written previously!

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
