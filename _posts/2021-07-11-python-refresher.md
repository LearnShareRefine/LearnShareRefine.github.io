---
layout: post
title: Python Refresher 1
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

```python
# addition
print("5 + 3 =", 5 + 3)

# subtraction
print("5 - 3 =", 5 - 3)

# multiplication
print("5 x 3 =", 5 * 3)

# division
print("5 / 3 =", 5 / 3)

# to the power of (exponential)
print("5 to the power of 3 =", 5 ** 3)

# Remainder/Modulus
print("The remainder when 5 is divided by 3 is", 7 % 3)

# Floor Division, division without decimal place
print("5 / 3 =", 5 // 3, "(floor division, remove all decimal)")
```

#### String Slicing
![Python2](https://user-images.githubusercontent.com/85727619/125202367-e5ee1a00-e2a5-11eb-8f25-e2666a87b14a.jpg)
![Python3](https://user-images.githubusercontent.com/85727619/125202370-ebe3fb00-e2a5-11eb-9907-7301410d860d.jpg)

```python
sentence = "The quick brown fox jumps over the lazy dog"

#Expected Outputs:
#The quick
#quick
#lazy dog
#god yzal eht revo spmuj xof nworb kciuq ehT
#Teqikbonfxjmsoe h aydg

print (sentence[0:9])
print (sentence[4:9])
print (sentence[-8:])
print (sentence[::-1])
print (sentence[::2])
```

#### List Operation
![Python4](https://user-images.githubusercontent.com/85727619/125202378-f3a39f80-e2a5-11eb-94e3-24e19bdeb4df.jpg)
![Python5](https://user-images.githubusercontent.com/85727619/125202403-04541580-e2a6-11eb-8f44-9d258195bdb2.jpg)

```python
list1 = [0, 1]
data = ['a','b']

#append()
#Expected Output = [0, 5, 2, ['a', 'b']]
list1[1] += 4
list1.append (2)
list1.append (data)
print (list1)

#extend()
list2 = [1,2,3,4]
list1.extend(list2)
print (list1)

#remove()
list1.remove(2)
print (list1)

#del 
del list1[2]
print (list1)

#sorted()
print (sorted (list1))
print (sorted (list1, reverse = True))

#sum of all numbers in a list
number_list = [1, 2, 3, 4]
print (sum(number_list))

#length of a string/list
string = 'ABCDEFG'
print (len(string))

alist = [10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
print (len(alist))

#min/max
print (max(alist))
print (min(alist))
```

#### Dictionary
![Python6](https://user-images.githubusercontent.com/85727619/125202408-07e79c80-e2a6-11eb-866e-6f959956e86c.jpg)

```python
video = {'video_title': 'im_yours',
        'views_per_day': [2500, 1400, 14100]}

avg_view = sum(video['views_per_day'])//len(video['views_per_day'])
print (avg_view)


video_views = [
    {'video_title': 'despacito',
        'all_views': [15000, 9800, 10100]},
    {'video_title': 'im_yours',
        'all_views': [2500, 1400, 14100]},
]

for i in video_views:
    avg_view = sum(i["all_views"])//len(i["all_views"])
    i["avg_view"] = avg_view

print (video_views)
```

#### If, elif, else
![Python7](https://user-images.githubusercontent.com/85727619/125202419-15048b80-e2a6-11eb-9181-873822190946.jpg)
![Python8](https://user-images.githubusercontent.com/85727619/125202421-1635b880-e2a6-11eb-8723-e67a894c20f0.jpg)

```python
if score >= 76:
    grade = "A"
elif score >= 60:
    grade = "B"
elif score >= 50:
    grade = "C"
else:
    grade = "Fail"

print ("Your Grade is", grade)


views = 85000
healthy_living_video = True
pay = 0

if healthy_living_video:
    if views > 50000:
        pay = (50000 * 0.01) + ((views - 50000) * 0.1)
    else:
        pay = views * 0.01
        
print ("You will earn", str(pay), "from this video")
```

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