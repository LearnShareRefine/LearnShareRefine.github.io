---
layout: post
title: Visualizing Retrenchment & Vacancies In Singapore using Numpy, Pandas, Matplotlib
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

<H3><font color='#003399'> Q: Did we face the most severe retrenchments in 2020? </font></H3>

```python
df_retrench_year = df_retrench.groupby(["year"]).agg([np.sum, np.mean])["retrench"].reset_index()

# Plot a line chart to visualise the retrenchment from 1998 till 2020
fig, ax = plt.subplots(figsize=(12,6))
plt.plot (df_retrench_year["year"], df_retrench_year["sum"],
          color = "orange", marker = "o", 
          markersize = 10, label="All Industry")

plt.xlabel('Year')
plt.ylabel('Retrenchment')
plt.legend(loc='best')
plt.title("Retrenchment by Year (All industry)",fontweight ='bold', fontsize = 15)
plt.grid(b=True,alpha = 0.3)
plt.show()
```

![Output 2](https://user-images.githubusercontent.com/85727619/128590222-9ad844c5-ab5e-4731-80bc-0bd0bd152b82.jpg)

From the chart above, we identified that Singapore experienced highest retrenchments in year 1998, 2001 and 2020.


### <font color='#003399'> Q: Which were the most badly impacted industries in these 3 years (1998, 2001, 2020)? </font>

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


### <font color='#003399'> Q: Which line of service experienced the highest retrenchment in 2020? </font>

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
# plot a horizontal bar graph to visualize retrenchment in service industry
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


### <font color='#003399'> Q: Were there enough vacancies in the service industry in 2020? </font>

```python
# read vacancy file into dataframe
xls = pd.ExcelFile('job-vacancy-by-industry-level-2.xlsx')
df_vacant = xls.parse('job-vacancy-by-industry-level-2')
print(df_vacant.shape)

df_vacant['job_vacancy'] = pd.to_numeric(df_vacant['job_vacancy'], errors='coerce').fillna(0).astype('int')
df_vacant_industry = df_vacant.groupby(["year","industry1"]).agg([np.sum])["job_vacancy"].reset_index()
df_vacant_1998 = df_vacant_industry[df_vacant_industry["year"]>1997]
df_vacant_1998.head()
```

![Output 4 5](https://user-images.githubusercontent.com/85727619/128601689-079ad328-4bc6-4649-ae7d-62dc7d5454de.jpg)

```python
# Plot a line chart to visualise the relationship between retrenchment and vacancy in service industry
df_retrench_year_industry["year"] = df_retrench_year_industry["year"].astype('int')
df_retrench_ex_2021 = df_retrench_year_industry[df_retrench_year_industry["year"]<2021]
df_retrench_service_ex_2021 = df_retrench_ex_2021[df_retrench_ex_2021["industry1"]=="services"]
df_vacant_service = df_vacant_1998[df_vacant_1998["industry1"]=="services"]

fig, ax = plt.subplots(figsize=(12,6))
plt.plot (df_retrench_service_ex_2021["year"], df_retrench_service_ex_2021["sum"],
          color = "Red", marker = "o", 
          markersize = 10, label="Retrenchment")

plt.plot (df_vacant_service["year"], df_vacant_service["sum"],
          color = "Orange", marker = "o", 
          markersize = 10, label="Vacancy")

plt.xlabel('Year')
plt.ylabel('Total')
plt.legend(loc='best')
plt.title("Retrenchment & Vacancy by Year (Services industry)",fontweight ='bold', fontsize = 15)
plt.grid(b=True,alpha = 0.3)
plt.show()
```

![Output 5](https://user-images.githubusercontent.com/85727619/128601787-7336fb31-f378-4863-ae93-dcc980e98c2f.jpg)

We see lower number of vacancies and increased retrenchments in 2020, but it seems like there were still a big gap of vacancies unfilled. 


### <font color='#003399'> Q: Where in the service industry did the vacancies lie? </font>

```python
df_vacant_services = df_vacant["industry1"]=="services"
df_vacant_2020 = df_vacant["year"]==2020
df_vacant_services_2020 = df_vacant[df_vacant_services & df_vacant_2020]
df_vacant_services_2020.dtypes
df_2020_ret_vac = pd.merge(left = df_retrench_services_2020, 
                           right = df_vacant_services_2020, 
                           how ='inner', 
                           left_on ='industry2', 
                           right_on ='industry2')

df_2020_ret_vac = df_2020_ret_vac[["industry2","sum","job_vacancy"]]
df_2020_ret_vac
```

![Output 5 5](https://user-images.githubusercontent.com/85727619/128601872-fce372dd-6bb8-49b3-b3f8-215fc5f0dfa4.jpg)

```python
# Visualize the retrenchements and vacancies to find out where the vacancies were in service industry.
df_2020_ret_vac.plot.barh(x='industry2',stacked = False,
                       figsize = (12,6),
                       fontsize = 12, color = ["#ff6600","#ffc299"],
                       legend = True,
                       title = "2020 Retrenchment and Vacancy in Service Industry")

plt.legend(["Retrenchement", "Vacancy"])
plt.xlabel("Total")
plt.ylabel("Service Lines")
plt.show()
```

![Output 6](https://user-images.githubusercontent.com/85727619/128601901-5e64a83c-d677-4715-b783-9ba998b08048.jpg)

**8 out of 9** lines in service had higher number of vacancies than number of retrenchments. <br>
**Transportation and Storage** was the 1 line of service that had higher number of retrenchments than vacancies. This is understandable as Covid lockdown in Singapore had restricted movements of individuals, and hence many workers from this line of service were made redundant.

It appears that **Community, Social and Personal Services** had the highest vacancy gap in 2020.

According to [data.gov.sg](https://data.gov.sg/), below groups of occupations belong to the field of Community, Social and Personal Services:
1. Public administration and education
2. Health and social services
3. Arts, entertainment and recreation
4. Other community, social and personal services

Was there a shortage of manpower in the line?



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
