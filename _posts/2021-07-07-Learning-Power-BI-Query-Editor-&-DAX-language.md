---
layout: post
title: Learning Power BI Query Editor & DAX language
categories: Power BI
published: true
---
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

### What have I learnt?
Focus on managing data in power query and creating DAX measures on **Power BI Desktop**.
- [x] Edit and transform data in Query Editor
- [x] DAX Syntax
- [x] Create DAX measures
- [x] DAX Formulas (eg: Calculate, Generateseries, X-factor functions, Filter, If function and so on)
- [x] Custom Visuals

### Result
Created a report using Power BI DAX measures and tables to detail a loan and it's payment schedule.

**Report**
![Dax exercise (loan dashboard) ](https://user-images.githubusercontent.com/85727619/124745183-1e15f580-df52-11eb-8fae-ee0989c2fd8b.jpg)


DAX formulas and functions are very similar to Excel formulas which I am familiar to but writing them without Excel cells make it complicated.
Definitely need more practice on this! 

**Followed through LinkedIn Learning course [Here](https://www.linkedin.com/learning/advanced-microsoft-power-bi/reducing-data-headaches?u=104800994)**
