---
layout: post
title: Spotting Trends in Spotify Top 200 Charted Songs with Pandas, Matplotlib and Seaborn
categories: Python
published: true
---

### Learning Objective
Using the modules to clean, transform and find trends.

### Dataset Used
Spotify Top 200 Weekly Global Charts in 2020 & 2021.

### Importing data as Pandas dataframe and cleaning data


```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv('music.csv', index_col='Index')
df.head()
```
![Output1](https://user-images.githubusercontent.com/85727619/143212301-8e4ad921-a527-4116-84b7-5f5e5704e0db.jpg)


<H2><font color='#003399'> Inspect the data </font></H2>
```python
len(df)
df.info()
```
![Output2](https://user-images.githubusercontent.com/85727619/143212571-de78f5f6-3fbb-48a5-9a09-49ac3435eaa3.jpg)


<H2><font color='#003399'> Data Quality Issues - Missing Values </font></H2>
From df.info() above, we know there are some missing values in Genre, it is confirmed using isna.
```python
df.isna().sum()
```
![Output3](https://user-images.githubusercontent.com/85727619/143216319-82b78e09-ac5a-4d71-ba57-af14220bfbe2.jpg)


However, we know that many times, dataset can also contain empty string or some nonsensical values. 
The below will replace all empty string with 'NaN'
```python
df = df.applymap(lambda x: np.nan if x == ' ' or x== '' else x)

# check the number of missing values again, now we see the 11 rows with empty string
df.isnull().sum()
```


<H2><font color='#003399'> Data Quality Issues - Incorrect dtypes </font></H2>
```python
# in order to change the dtype for Streams and Artist Followers, need to first remove the comma
df["Streams"] = df["Streams"].str.replace(',','').astype('int64')
df["Artist Followers"] = df["Artist Followers"].str.replace(',','').astype('float64')

# Convert the rest of the columns to numeric as well
df[["Popularity",Duration (ms)","Danceability","Energy","Loudness","Speechiness","Acousticness","Liveness","Tempo","Valence"]]= \
df[["Popularity",Duration (ms)","Danceability","Energy","Loudness","Speechiness","Acousticness","Liveness","Tempo","Valence"]].apply(pd.to_numeric)
```


<H2><font color='#003399'> Let's look at the top 50 most charted songs in 2020-2021, do these songs have highest streams also? </font></H2>
Any top charted song which is also in the top 50 streams will be highlighted in salmon color

```python
# filter top 50 songs with highest number of times charted
top50_charted = df.sort_values(by='Number of Times Charted',ascending=False)[:50]

# filter songs with top 50 streams
df['Streams'] = df['Streams'].apply(lambda x:x/1000)
top50_stream = df.sort_values(by='Streams', ascending=False)[:50]

# Plot bar chart and line chart
fig, ax = plt.subplots(figsize=(15,8))
col = top50_charted['Song Name'].apply(lambda x: 'salmon' if x in list(top50_stream['Song Name']) else "lightgray")
bars = ax.bar(top50_charted['Song Name'], top50_charted['Number of Times Charted'], color = col)

ax2=ax.twinx()
ax2.plot(top50_charted['Song Name'], top50_charted['Streams'],
         color = "darkblue", marker = "o", markersize = 8)

ax.set_xlabel('Song Name', size = 12)
ax.set_ylabel('Number of Times Charted', size = 12)
ax2.set_ylabel('Number of Streams (in Thousands)', size = 12)
for tick in ax.get_xticklabels():
    tick.set_rotation(90)

plt.title('Top 50 Most Charted Songs', fontweight="bold", size = 15)
labels = ["Number of Times Charted", "Number of Times Charted-Top 50 Streams"]
ax.legend([bars[0],bars[19]], labels, loc='lower center',bbox_to_anchor=(0.35, -0.7))
ax2.legend(('Total Streams (in Thousands)',),loc='lower center',bbox_to_anchor=(0.65, -0.67))

plt.show()
```

![chart1](https://user-images.githubusercontent.com/85727619/143220675-6a6e7684-c133-41ed-9204-4f83f08dd93a.jpg)


From the chart above, we can see that out of the top 50 most charted songs, only one song is within the top 50 Streams. 
This implies that songs with high streaming volume might not be charted in the Top 200 list. 

Next, let's take a quick look to see if any artist dominates the top 50 most charted song list.


```python
top50_charted.groupby('Artist')['Song Name'].count().sort_values(ascending=True).plot(kind='barh',figsize=(16,9),
                                                                                     title="Artists of Top 50 Charted Songs")
plt.xticks(range(0,5,1))
plt.show()
```

![chart2](https://user-images.githubusercontent.com/85727619/143221767-af825ecf-afc8-413a-9f7c-336c1a79cc02.jpg)

4 out of 50 songs in the top 50 most charted songs belongs to Billie Eilish!


<H2><font color='#003399'> Does the number of times a song get charted has anything to do with the song's characteristics? </font></H2>

```python
# Extract the required columns into a new dataframe
df_relation = df[["Number of Times Charted","Popularity","Danceability","Energy","Loudness","Speechiness","Acousticness","Liveness",
                  "Tempo","Valence","Duration (ms)","Artist Followers", "Highest Charting Position"]]
                  
# Inspected the data and found out that there are 11 missing values for each columns, drop these 11 rows

df_relation = df_relation.dropna()

# check if the values make sense
display(df_relation.describe())

# According to data dictionary, "Loudness" should range between -60 and 0 but we have 1 song with loudness of 1.509
# Assume this is outlier/incorrect data entry and hence exclude this row from analysis
df_relation = df_relation.drop(df_relation[df_relation['Loudness']>=0].index)

# Plot scatter plot with fitted line to illustrate the relationship betwween number of times charted and other variables.
# Also included the correlation coeficient as the legend to help visualise the correlation

from scipy import stats
var=["Popularity","Danceability","Energy","Loudness","Speechiness","Acousticness",
    "Liveness","Tempo","Valence","Duration (ms)","Artist Followers", "Highest Charting Position"]

f = plt.figure(figsize=(16,18))
gs = f.add_gridspec(4,3)
f.suptitle('Scatter Plots and Fitted Line Showing Correlation with Number of Times Charted', va='bottom', size = 15, weight ='bold')

for i in range(4):
    for j in range(3):
        slope, intercept, r_value, p_value, std_err = stats.linregress(df_relation["Number of Times Charted"], df_relation[var[i*3+j]])
        ax = f.add_subplot(gs[i,j])
        ax.set_title('Charted Count vs {}'.format(var[i*3+j]),fontweight ='bold')
        sns.regplot(data=df_relation, y=var[i*3+j], x="Number of Times Charted", ax=ax, ci=95, 
                    line_kws={"color": "red", 'label': "r={:.2f}".format(r_value)})
        ax.legend(loc = 'upper right')

plt.subplots_adjust(top = 0.94, wspace = 0.3, hspace=0.3)
plt.show()

```

![chart3](https://user-images.githubusercontent.com/85727619/143225359-6bfae85e-2547-4b49-9ba8-32e72842d25c.png)


<H2><font color='#003399'> Insights </font></H2>

The correlation coefficient between number of times a song is charted and other variables seem to be generally low. Popularity has the correlation coefficient 0f 0.45.

Statistical test can be performed to confirm this linear relationship. Non-linear fit can also be explored as the relationship might be non-linear.


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
