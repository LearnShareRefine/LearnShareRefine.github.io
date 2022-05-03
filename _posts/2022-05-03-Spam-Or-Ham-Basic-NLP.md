---
layout: post
title: Spam or Ham - NLP & Classification
categories: Python
published: true
---
<left><img width="280" src="https://user-images.githubusercontent.com/85727619/166442105-65636720-d093-46ab-8eff-443602c03a4f.png"></left>
<right><img width="290" src="https://user-images.githubusercontent.com/85727619/166446162-b092bd1c-b110-48cb-9eb2-3a5634d894fe.png"></right>

## Project Objective
Build a model to classify SMSes into spam or ham (non-spam). 

## Methodology
In this project, Natural Language Toolkit (nltk) is used to perform text processing and vectorization. Vectorized corpus were then fed 
to 3 machine learning models (Naive Bayes, SVC and Random Forest) for classification.

## Dataset Used
Dataset gotten from [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/SMS+Spam+Collection)

## Basic EDA

```python
import nltk
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
sns.set_style('whitegrid')

messages = pd.read_csv('smsspamcollection/SMSSpamCollection', sep='\t', names=["label", "message"])
messages.head(10)
```

<img width="310" alt="image" src="https://user-images.githubusercontent.com/85727619/166443282-fcb0e010-933a-4515-b7aa-820d64a42fcd.png">


```python
sns.countplot(data=messages,x='label')
plt.show()
```

<img width="340" alt="image" src="https://user-images.githubusercontent.com/85727619/166443749-96b0f03b-2b18-495a-80be-8007ca4afc06.png">

```python
# explore the length of messages
messages['length'] = messages['message'].apply(len)
messages.describe()
```

<img width="140" alt="image" src="https://user-images.githubusercontent.com/85727619/166443684-9658825b-fb48-4c10-a661-20c75af358b2.png">

```python
plot = messages[messages['length'] < 250]

plt.figure(figsize=(11,7))
sns.histplot(data=plot,x='length',hue='label')
```

<img width="410" alt="image" src="https://user-images.githubusercontent.com/85727619/166444473-6a4104f3-13ce-4aff-a155-e37cb6ce37e0.png">

## Text Processing & Model Building

```python
# define a function to remove non-words & stopwords
from nltk.corpus import stopwords

def clean_tokenize(string):
    word = [s.lower() for s in string.split() if s.lower() not in stopwords.words('english') 
            and s.isalpha()==True]
    return word
```

Next, we perform train test split and create a pipeline to carry out the workflow of vectorization, TF-IDF transformation, model fitting and prediction.

```python
# train test split
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(messages['message'], messages['label'], test_size=0.3)

# set up a pipeline
from sklearn.pipeline import Pipeline
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.feature_extraction.text import TfidfTransformer
from sklearn.naive_bayes import MultinomialNB
from sklearn.ensemble import RandomForestClassifier
from sklearn.svm import SVC
from sklearn.metrics import classification_report, confusion_matrix

model = [MultinomialNB(), RandomForestClassifier(), SVC()]
result = {}
for m in model:
    pipeline = Pipeline([
        ('bow', CountVectorizer(analyzer=clean_tokenize)), 
        ('tfidf', TfidfTransformer()),
        ('classifier', m)])
    pipeline.fit(X_train, y_train)
    y_pred = pipeline.predict(X_test)
    result[m] = classification_report(y_test,y_pred)

for k, v in result.items():
    print(k,'\n',v)
```

<img width="370" alt="image" src="https://user-images.githubusercontent.com/85727619/166444973-61228524-002d-485d-89b7-c7ad995f2608.png">

Random Forest seems to produce the best result based on accuracy, recall and f1 score.

## Future Improvement
1. The messages in this dataset consists of many 'Singlish' words (Singaporean-English - localized English in Singapore), short-form and mis-spelled words. Other open source tools from nltk or textblob etc can be explored to help fix this issue.
2. More fine-tuning can be done on the models










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

