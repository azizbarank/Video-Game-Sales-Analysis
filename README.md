# Video-Game-Sales-Analysis
## Overview
This project aims to analyse and generate insight into the video game sales between 1980s-2010s. To achieve this, it uses the Python libraries of pandas, numpy, matplotlib and seaborn. Additionally, to have a clear object, several questions questions were defined beforehand in two ways:

### Project Questions:
#### To be answered during the preliminary analysis with pandas:
* What is the distribution of the global sales per genre?
* What is the trend of global sales throughout the years?
* What are the most popular platforms over the years (top 15)?
#### To be answered with Tableau:
* What are the most sold games so far (top 3)?
* What are the most sold games per genre so far?

## Dataset & Methods

---
### Dataset
The dataset was taken from [Kaggle](https://www.kaggle.com/datasets/gregorut/videogamesales/), which was scraped from [vgchartz](http://www.vgchartz.com/). The general structure of the dataset is as follows:
* **Rank**: Ranking of overall sales
* **Name**: The games name
* **Platform**: Platform of the games release (i.e. PC,PS4, etc.)
* **Year**: Year of the game's release
* **Genre**: Genre of the game
* **Publisher**: Publisher of the game
* **NA_Sales**: Sales in North America (in millions)
* **EU_Sales**: Sales in Europe (in millions)
* **JP_Sales**: Sales in Japan (in millions)
* **Other_Sales**: Sales in the rest of the world (in millions)
* **Global_Sales**: Total worldwide sales.
---
---
### Methods & Steps
Below are the general steps taken for the data analysis. A few example codes are given to have an idea of what has been done. To look at the full codes, you can refer to the [notebook](https://github.com/azizbarank/Video-Game-Sales-Analysis/blob/main/games_analysis.ipynb).
1. Importing the necessary packages:

```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
%matplotlib inline
```

2. First quick glance at the dataset:
```python
df.head()
df.info()
df.value_counts()
```

3. Checking if there is any duplicate values, dropping null values based on the "Year" column and changing its data type from float64 to date:
```python
df[df.duplicated()]
df.dropna(subset= ["Year"], inplace=True)
df['Year'] = df['Year'].astype(int)
df['Year'] = pd.to_datetime(df['Year'], format='%Y')
df['Year'].head()
```
4. Gaining insights:
    ##### 1. Distribution of global sales per genre (*pie and bar plot*)
    ##### 2. Trend of global sales throughout the years (*line plot*)
    ##### 3. Most popular platforms over the years (*bar plot*)
      ```python
      genre_global_sales['Global_Sales'].plot.pie(title= 'Global Game Sales Per Genre in %', autopct = '%1.1f', figsize=(10,10))
      genre_global_sales_2 = pd.DataFrame(df.groupby('Genre')['Global_Sales'].sum()).sort_values('Global_Sales', ascending=True)
      genre_global_sales_2.plot.barh(title= 'Global Game Sales Per Genre', color = 'DarkBlue', figsize=(10,9))
      .
      .
      .
      yearly_counts = df.resample('Y').count()
      plt.plot(yearly_counts.index, yearly_counts['Genre'])
      plt.xlabel('Year')
      plt.ylabel('Count of Genre')
      plt.title('The Count of Genre Sales through Years')
      plt.show()
      .
      .
      .
      top_15_platforms = df['Platform'].value_counts().head(15).index
      platform = df[df['Platform'].isin(top_15_platforms)].groupby('Platform').resample('Y').size().unstack(level=0)
      platform.plot(figsize=(15,6))
      ```
5. Further visualizations with Tableau
---
## Results (Project Questions)
1. What is the distribution of the global sales per genre?
![globalsalespergenre](https://github.com/azizbarank/Video-Game-Sales-Analysis/blob/main/images/pie.png)
![globalsalespergenre](https://github.com/azizbarank/Video-Game-Sales-Analysis/blob/main/images/barplot.png)

As can be seen from the plots, the most played genres over the years seem to be action, sports, shooter and role-playing in descending order.

2. What is the trend of global sales throughout the years?
![trendofglobalsales](https://github.com/azizbarank/Video-Game-Sales-Analysis/blob/main/images/timeseries.png)

The clear thing from this graph is that sales increased as the years go on, especially between 2000-2010, peaking at the latter.

3. What are the most popular platforms over the years (top 15)?
![popularplatforms](https://github.com/azizbarank/Video-Game-Sales-Analysis/blob/main/images/platform.png)

It seems like the dataset indeed shows an expected pattern of sales of each platform in terms of their respective popular periods of time. For instance, between 1994-2003, PS1 seems to be the most popular platform compared to others, which is not surprising given it was released in December of 1994. Furthermore, between 2004-2012, NintendoDS appears to be one of the most sold platforms ever, which was released in November of 2004.

4. What are the most sold games so far (top 3)?
5. What are the most sold games per genre so far?

(To answer both questions at the same time with Tableau)

![Tableau](https://github.com/azizbarank/Video-Game-Sales-Analysis/blob/main/images/dashboard.png)

The top 3 games of all time seem to be Wii Sports, Grand Theft Auto V and Super Mario Bros., respectively. For the most sold games per genre aside from the top three, Tetris is the most popular for puzzle games, while Mario Kart Wii, Pokemon Red/Pokemon Blue and Call of Duty: Modern Warfare 3 are for the racing, role-playing, and the shooter categories, respectively.
