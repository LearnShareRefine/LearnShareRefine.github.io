---
layout: post
title: Python Library - Numpy, Pandas, Matplotlib
categories: Python
published: true
---

### Learning Objective
Using the modules to clean, transform and visualise Singapore retrenchment and job vacancies data from 1998 - 2020.

### Dataset
Obtained from: data.gov.sg/dataset/
<iframe width="600" height="400" src="https://data.gov.sg/dataset/job-vacancy-by-industry-and-occupational-group-annual/resource/c9aa2db3-99f8-45cf-a0f3-7a86fced62df/view/25d8cab6-2ac3-4ba3-ba58-ae7de8d55e9d" frameBorder="0"> </iframe>

### Reading and cleaning data
```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# read retrenchment file into dataframe
xls = pd.ExcelFile('retrenchment-by-industry-level-2.xlsx')
df_retrench = xls.parse('retrenchment-by-industry-level')

# Split into year and quarter:
df_retrench["year"] = df_retrench["quarter"].str.split('-').str[0]
df_retrench["quarter"] = df_retrench["quarter"].str.split('-').str[1]

# move year to the first column
df_retrench.insert(0, "year", df_retrench.pop("year"))

# Convert values to flaot & coerce invalid values to NaN, then replace NaN with 0 and convert all to integer
df_retrench['retrench'] = pd.to_numeric(df_retrench['retrench'], errors='coerce').fillna(0).astype('int')

print(df_retrench.shape)
df_retrench.head(20)
```
![Output 1jpg](https://user-images.githubusercontent.com/85727619/128590108-357d727e-d15e-48e7-a6a0-6319fa6541cb.jpg)

### Q: Did Singapore face the most severe retrenchments in 2020?
```python
df_retrench_year = df_retrench.groupby(["year"]).agg([np.sum, np.mean])["retrench"].reset_index()

# Plot a line chart to visualise the retrenchments from 1998 till 2020
fig, ax = plt.subplots(figsize=(12,6))
plt.plot (df_retrench_year["year"], df_retrench_year["sum"],
          color = "orange", marker = "o", 
          markersize = 10, label="All Industry")

plt.xlabel('Year')
plt.ylabel('Retrenchment')
plt.legend(loc='best')
plt.title("Retrenchment by Year (All industry)")
plt.grid(b=True,alpha = 0.3)
plt.show()
```

![Output 2](https://user-images.githubusercontent.com/85727619/128590222-9ad844c5-ab5e-4731-80bc-0bd0bd152b82.jpg)

From the above graph, we can tell that there were more retrenchment in 1998 and 2001 than in 2020.


### Q: Which are the most badly impacted industry in these 3 years (1998, 2001, 2020)?
```python
df_retrench_year_industry = df_retrench.groupby(["year",'industry1']).agg([np.sum])["retrench"].reset_index()

select_year = df_retrench_year_industry["year"].isin(["1998", "2001", "2020"])
df_retrench_3year_industry = df_retrench_year_industry[select_year]

df_retrench_3year_industry.set_index("year", inplace = True)
df_retrench_3year_industry
```

![Output 2 5](https://user-images.githubusercontent.com/85727619/128590789-88dcb35a-6352-473b-ae79-ace3482b21b8.jpg)

```python
# Plot a bar graph to visualise the retrenchment by industry in these 3 years
# set width of bar
barWidth = 0.25
fig, ax = plt.subplots(figsize =(12, 6))

# set height of bar
construction = df_retrench_3year_industry[df_retrench_3year_industry["industry1"]=="construction"]["sum"]
manufacturing = df_retrench_3year_industry[df_retrench_3year_industry["industry1"]=="manufacturing"]["sum"]
services = df_retrench_3year_industry[df_retrench_3year_industry["industry1"]=="services"]["sum"]

# Set position of bar on X axis
bar1 = np.arange(len(construction))
bar2 = [x + barWidth for x in bar1]
bar3 = [x + barWidth for x in bar2]
 
# Make the plot
plt.bar(bar1, construction, color ='#b30000', width = barWidth,
        edgecolor ='grey', label ='Construction')
plt.bar(bar2, manufacturing, color ='#003d99', width = barWidth,
        edgecolor ='grey', label ='Manufacturing')
plt.bar(bar3, services, color ='#00802b', width = barWidth,
        edgecolor ='grey', label ='Services')
 
# Adding Xticks
plt.xlabel('Year')
plt.ylabel('Retrenchment')
plt.title("Years with Highest Retrenchment (Industry Breakdown)",fontweight ='bold', fontsize = 15)
plt.xticks([r + barWidth for r in range(len(construction))],['1998','2001','2020'])

plt.legend(loc='best')
plt.show()
```

![Output 3](https://user-images.githubusercontent.com/85727619/128590561-5b99ced2-43e6-4319-b7bc-b1d1bacae443.jpg)

Unlike the 2 earlier years, where manufacturing was the industry facing most retrenchments, in 2020, services was the most badly impacted industry.


### Q: Which line of service in specific has the highest retrenchment in 2020?
```python
df_retrench_services = df_retrench["industry1"]=="services"
df_retrench_2020 = df_retrench["year"]=="2020"
df_retrench_services_2020 = df_retrench[df_retrench_services & df_retrench_2020]

df_retrench_services_2020 = df_retrench_services_2020.groupby("industry2").agg([np.sum])["retrench"].reset_index()
df_retrench_services_2020 = df_retrench_services_2020.sort_values(by=['sum'],ascending=True)
df_retrench_services_2020
```

![Output 3 5](https://user-images.githubusercontent.com/85727619/128590766-0cd5dfcd-bcb5-4aa8-84c0-e85b9c8df6bb.jpg)

```python
# plot a horizontal bar graph for visualisation
from matplotlib import cm
n = len(df_retrench_services_2020)
colors = cm.Greens(np.linspace(0.3, 1, n))
ax = df_retrench_services_2020.plot.barh(x='industry2', y='sum',
                                figsize=(12,6),
                                legend = False,
                                title = "2020 Retrenchment in Service Industry",
                                color = colors)

ax.set_ylabel('Service Lines')
ax.set_xlabel('Retrenchment')
plt.show()
```

![Output 4](https://user-images.githubusercontent.com/85727619/128590728-14e258c6-6610-4b2d-b9d0-79b3c7c3f3d7.jpg)



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
