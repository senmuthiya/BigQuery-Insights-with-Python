# Wikipedia Pageviews Analysis Project

## Overview

In this project, you will use Python and Google BigQuery to analyze Wikipedia pageviews for a specific time period. We'll leverage the capabilities of Google Colab for seamless integration with BigQuery and powerful data analysis using Python.

### Objectives

1. Authenticate to Google Colab and BigQuery.
2. Explore the available dataset in BigQuery (e.g., `bigquery-public-data.wikipedia.pageviews_2024`).
3. Write and execute SQL queries to extract relevant data.
4. Perform exploratory data analysis using Pandas and Matplotlib.
5. Create visualizations, such as histograms, time series plots, word clouds, and more.
6. Gain insights into Wikipedia pageviews trends and patterns.


### Set Up Google Colab

1. Open Google Colab (https://colab.research.google.com/) in your web browser.
2. Create a new notebook: `File` > `New Notebook`.
3. Change the notebook name to something meaningful.

### Authenticate Google Colab and BigQuery

```python
from google.colab import auth

# Authenticate Google Colab
auth.authenticate_user()
```

### Import Necessary Libraries

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from wordcloud import WordCloud
from google.colab import auth
from google.cloud import bigquery
```

### Create a BigQuery Client

```python
# Replace 'your-project-id' with your actual Google Cloud project ID
project_id = 'your-project-id'
client = bigquery.Client(project=project_id)
```

### Explore the Available Dataset

Explore the available datasets and tables in BigQuery. Choose a relevant table for your analysis, e.g., `bigquery-public-data.wikipedia.pageviews_2024`.

### Write and Execute SQL Queries

```python
# Reference to the table
table_ref = client.dataset('wikipedia', project='bigquery-public-data').table('pageviews_2024')
```

# Write and execute SQL queries
```
query = '''
SELECT *
FROM `{table_ref}`
WHERE datehour BETWEEN TIMESTAMP('2024-01-01 00:00:00') AND TIMESTAMP('2024-01-31 00:00:00')
LIMIT 1000
'''
df = client.query(query).to_dataframe()
```

### Exploratory Data Analysis

Explore the DataFrame (`df`) to gain insights into the dataset. Use the following commands to understand its structure, summary statistics, and view a sample of the data.

#### Display Basic Info about the DataFrame

```python
# Display basic info about the DataFrame
df.info()
```

### Visualizations

Now that you've explored the dataset, let's create visualizations to gain further insights.

#### Histogram of Article Views

```python
if 'views' in df.columns:
    # Example: Plot a histogram of article views
    plt.hist(df['views'], bins=30, edgecolor='black')
    plt.title('Distribution of Article Views')
    plt.xlabel('Number of Views')
    plt.ylabel('Frequency')
    plt.show()
else:
    print('The "views" column does not exist in the dataframe.')
```

### Find the Most Edited Articles

In this example, we'll identify the top 10 most edited articles based on the count of revision IDs.

```python
# Example: Find the most edited articles
top_edited_articles = df.groupby('title')['revision_id'].count().sort_values(ascending=False).head(10)

print("Top 10 Most Edited Articles:")
print(top_edited_articles)
```

### Word Cloud of Top Viewed Articles

Word clouds are a visually appealing way to represent the most viewed articles in a dataset. In this section, we'll create a word cloud based on the 'views' column to highlight the top viewed articles.

```python
# Group by with the columns 'title' and 'views'
top_viewed_articles = df.groupby('title')['views'].sum().sort_values(ascending=False).head(10)
```

# Convert the pandas Series to a dictionary
``` python
wordcloud_data = top_viewed_articles.to_dict()
```

# Generate a word cloud
``` python
wordcloud = WordCloud(width=800, height=400, background_color='white').generate_from_frequencies(wordcloud_data)
```
# Display the word cloud using Matplotlib
``` python
plt.figure(figsize=(10, 6))
plt.imshow(wordcloud, interpolation='bilinear')
plt.axis('off')
plt.title('Top Viewed Articles')
plt.show()
```
## Conclusion

Congratulations on completing the Wikipedia Pageviews Analysis project! Throughout this analysis, you've utilized Python, BigQuery, and various data visualization techniques to explore and gain insights from the Wikipedia pageviews dataset.

**Key Findings and Insights:**

1. **Exploratory Data Analysis:**
   - You explored the dataset, gaining a better understanding of its structure and characteristics using commands like `df.info()`, `df.describe()`, and `df.head()`.

2. **Visualizations:**
   - Visualized the distribution of article views with a histogram, providing insights into the spread of views across articles.
   - Plotted a time series to observe trends and patterns in Wikipedia pageviews over time.
   - Created a word cloud to visually represent the top viewed articles, offering a creative and intuitive way to highlight popular content.

3. **Top Edited Articles:**
   - Identified and printed the top 10 most edited articles based on the count of revision IDs, providing a perspective on the most actively edited content.

**Additional Tips and Considerations:**

- If encountering issues like missing columns or unexpected data, check the schema of your dataset using `client.get_table(table_ref)`.
- Adapt and modify queries, visualizations, and analyses based on your specific dataset and analysis goals.

**Quota Exceeded Handling:**

```python
from google.api_core.exceptions import Forbidden

try:
    # Your analysis code here

except Forbidden as e:
    print(f"Error: {e}")
    print("Quota exceeded. Please check your BigQuery quota and try again later.")
```

# Happy Coding!











































