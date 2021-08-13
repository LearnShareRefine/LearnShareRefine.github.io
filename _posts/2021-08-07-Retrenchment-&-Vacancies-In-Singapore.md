---
layout: post
title: Retrenchment & Vacancies In Singapore - using Numpy, Pandas, Matplotlib
categories: Python
published: true
---

### Learning Objective
Using the modules to clean, transform and visualise Singapore retrenchment and job vacancies data from 1998 - 2020.

### Dataset
Data from: [data.gov.sg](https://data.gov.sg/)

[Retrenchment Dataset](https://data.gov.sg/dataset/retrenched-employees-by-industry-and-occupational-group-quarterly?resource_id=39fd4186-8a2b-441d-8de4-8ece239c5f39)

<iframe width="600" height="400" src="https://data.gov.sg/dataset/retrenched-employees-by-industry-and-occupational-group-quarterly/resource/39fd4186-8a2b-441d-8de4-8ece239c5f39/view/d626d058-924f-4d9f-9c96-91d8adee6496" frameBorder="0"> </iframe>

[Vacancy Dataset](https://data.gov.sg/dataset/job-vacancy-by-industry-and-occupational-group-annual?resource_id=c9aa2db3-99f8-45cf-a0f3-7a86fced62df)

<iframe width="600" height="400" src="https://data.gov.sg/dataset/job-vacancy-by-industry-and-occupational-group-annual/resource/c9aa2db3-99f8-45cf-a0f3-7a86fced62df/view/25d8cab6-2ac3-4ba3-ba58-ae7de8d55e9d" frameBorder="0"> </iframe>

### Importing data as Pandas dataframe and cleaning data

![1](https://user-images.githubusercontent.com/85727619/128605237-4145cc6a-5ee7-42a6-bb7e-c107440bd454.jpg)

![Output 1jpg](https://user-images.githubusercontent.com/85727619/128590108-357d727e-d15e-48e7-a6a0-6319fa6541cb.jpg)

<H3><font color='#003399'> Q: Did Singapore face the most severe retrenchments in 2020? </font></H3>

![2](https://user-images.githubusercontent.com/85727619/128605244-2f81e5cd-a46b-413b-bd1d-5b8ce4c1adeb.jpg)

![Output 2](https://user-images.githubusercontent.com/85727619/128590222-9ad844c5-ab5e-4731-80bc-0bd0bd152b82.jpg)

From the above graph, we can tell that there were more retrenchment in 1998 and 2001 than in 2020.


### <font color='#003399'> Q: Which are the most badly impacted industry in these 3 years (1998, 2001, 2020)? </font>

![3](https://user-images.githubusercontent.com/85727619/128605253-6c66126c-9daa-4566-a3a3-fbb2ac25a3ba.jpg)

![Output 2 5](https://user-images.githubusercontent.com/85727619/128590789-88dcb35a-6352-473b-ae79-ace3482b21b8.jpg)

![4 1](https://user-images.githubusercontent.com/85727619/128605257-e9fba771-0aac-4ddc-a6da-883228f1ad8e.jpg)

![Output 3](https://user-images.githubusercontent.com/85727619/128590561-5b99ced2-43e6-4319-b7bc-b1d1bacae443.jpg)

Unlike the 2 earlier years, where manufacturing was the industry facing most retrenchments, in 2020, services was the most badly impacted industry.


### <font color='#003399'> Q: Which line of service in specific has the highest retrenchment in 2020? </font>

![5](https://user-images.githubusercontent.com/85727619/128605260-adb85e83-088a-40fa-a468-6757f87d3e9c.jpg)

![Output 3 5](https://user-images.githubusercontent.com/85727619/128590766-0cd5dfcd-bcb5-4aa8-84c0-e85b9c8df6bb.jpg)

![6](https://user-images.githubusercontent.com/85727619/128605262-bbabd2a1-e4ae-4e82-9a5f-dc2a2f4e7be4.jpg)

![Output 4](https://user-images.githubusercontent.com/85727619/128590728-14e258c6-6610-4b2d-b9d0-79b3c7c3f3d7.jpg)


### <font color='#003399'> Q: Are there enough vacancies in the service industry? </font>

![7](https://user-images.githubusercontent.com/85727619/128605266-efcaa627-1221-46ff-b67e-8507cb5c38c7.jpg)

![Output 4 5](https://user-images.githubusercontent.com/85727619/128601689-079ad328-4bc6-4649-ae7d-62dc7d5454de.jpg)

![8](https://user-images.githubusercontent.com/85727619/128605267-641fc9d6-aea6-4555-a503-0902668d98a7.jpg)

![Output 5](https://user-images.githubusercontent.com/85727619/128601787-7336fb31-f378-4863-ae93-dcc980e98c2f.jpg)

Seems like there was quite a number of vacancies unfilled despite sharp increase in retrenchment?

Let's see where in service industry do the unfilled vacancies lie.

![9](https://user-images.githubusercontent.com/85727619/128605272-d6d158ff-1519-4a59-ad8d-a17b82b23261.jpg)

![Output 5 5](https://user-images.githubusercontent.com/85727619/128601872-fce372dd-6bb8-49b3-b3f8-215fc5f0dfa4.jpg)

![10](https://user-images.githubusercontent.com/85727619/128605277-5443bbdb-d774-4ad7-9e59-8efec17658e7.jpg)

![Output 6](https://user-images.githubusercontent.com/85727619/128601901-5e64a83c-d677-4715-b783-9ba998b08048.jpg)

It appears that *Community, Social and Personal Services* has the highest unfilled vacancies in 2020.

According to [data.gov.sg](https://data.gov.sg/), below groups of occupations belong to the field of Community, Social and Personal Services:
1. Public administration and education
2. Health and social services
3. Arts, entertainment and recreation
4. Other community, social and personal services

So are we right to say that there was a workforce shortage in this field in year 2020?


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