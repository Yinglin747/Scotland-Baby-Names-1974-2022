# Scotland Baby Names 1974-2022

The National Records of Scotland has made available data on registered baby names from 1974 through the present.
<br>Full List 1974 - 2022 in CSV format can be found here: https://www.nrscotland.gov.uk/statistics-and-data/statistics/statistics-by-theme/vital-events/names/babies-first-names/babies-first-names-2022
<br><br>
In this project, I would like to do some data manipulation in Python and try to explore the following topics:
1. Total births by sex and year
2. Names that show a surging popularity vs consistent popularity over time
3. Trend in naming choices
4. The distribution of baby names by the first letter over time
5. Trending gender-neutral baby names


```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

%matplotlib inline
```


```python
names = pd.read_csv("babies-first-names-all-names-all-years.csv")
```


```python
names=names[["yr","sex","FirstForename","number","rank"]]
```


```python
names.dtypes
```




    yr                int64
    sex              object
    FirstForename    object
    number            int64
    rank              int64
    dtype: object




```python
names.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 264149 entries, 0 to 264148
    Data columns (total 5 columns):
     #   Column         Non-Null Count   Dtype 
    ---  ------         --------------   ----- 
     0   yr             264149 non-null  int64 
     1   sex            264149 non-null  object
     2   FirstForename  264149 non-null  object
     3   number         264149 non-null  int64 
     4   rank           264149 non-null  int64 
    dtypes: int64(3), object(2)
    memory usage: 10.1+ MB
    


```python
names
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>yr</th>
      <th>sex</th>
      <th>FirstForename</th>
      <th>number</th>
      <th>rank</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1974</td>
      <td>B</td>
      <td>Aamir</td>
      <td>1</td>
      <td>472</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1974</td>
      <td>B</td>
      <td>Aaron</td>
      <td>17</td>
      <td>139</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1974</td>
      <td>B</td>
      <td>Abdul</td>
      <td>5</td>
      <td>248</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1974</td>
      <td>B</td>
      <td>Abdullahi</td>
      <td>1</td>
      <td>472</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1974</td>
      <td>B</td>
      <td>Abdulrazak</td>
      <td>1</td>
      <td>472</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>264144</th>
      <td>2022</td>
      <td>G</td>
      <td>Zunaira</td>
      <td>1</td>
      <td>1515</td>
    </tr>
    <tr>
      <th>264145</th>
      <td>2022</td>
      <td>G</td>
      <td>Zuriel</td>
      <td>1</td>
      <td>1515</td>
    </tr>
    <tr>
      <th>264146</th>
      <td>2022</td>
      <td>G</td>
      <td>Zuzzanna</td>
      <td>1</td>
      <td>1515</td>
    </tr>
    <tr>
      <th>264147</th>
      <td>2022</td>
      <td>G</td>
      <td>Zya</td>
      <td>1</td>
      <td>1515</td>
    </tr>
    <tr>
      <th>264148</th>
      <td>2022</td>
      <td>G</td>
      <td>Zyva</td>
      <td>1</td>
      <td>1515</td>
    </tr>
  </tbody>
</table>
<p>264149 rows × 5 columns</p>
</div>



Total number of registered births by sex from 1974 to 2022


```python
names.groupby("sex")["number"].sum()
```




    sex
    B    1497467
    G    1421143
    Name: number, dtype: int64



Number of registered births by year and sex


```python
total_number_by_year = names.pivot_table("number",index="yr",columns="sex",aggfunc=sum)
total_number_by_year.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>sex</th>
      <th>B</th>
      <th>G</th>
    </tr>
    <tr>
      <th>yr</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1974</th>
      <td>35773</td>
      <td>34228</td>
    </tr>
    <tr>
      <th>1975</th>
      <td>34965</td>
      <td>32897</td>
    </tr>
    <tr>
      <th>1976</th>
      <td>33465</td>
      <td>31364</td>
    </tr>
    <tr>
      <th>1977</th>
      <td>31954</td>
      <td>30318</td>
    </tr>
    <tr>
      <th>1978</th>
      <td>33031</td>
      <td>31221</td>
    </tr>
    <tr>
      <th>1979</th>
      <td>35323</td>
      <td>33004</td>
    </tr>
    <tr>
      <th>1980</th>
      <td>35367</td>
      <td>33474</td>
    </tr>
    <tr>
      <th>1981</th>
      <td>35266</td>
      <td>33755</td>
    </tr>
    <tr>
      <th>1982</th>
      <td>33895</td>
      <td>32270</td>
    </tr>
    <tr>
      <th>1983</th>
      <td>33651</td>
      <td>31419</td>
    </tr>
  </tbody>
</table>
</div>



Another way to achieve the same result as above


```python
total_number_by_year_2 = names.groupby(["yr","sex"])["number"].sum().unstack()
total_number_by_year_2.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>sex</th>
      <th>B</th>
      <th>G</th>
    </tr>
    <tr>
      <th>yr</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1974</th>
      <td>35773</td>
      <td>34228</td>
    </tr>
    <tr>
      <th>1975</th>
      <td>34965</td>
      <td>32897</td>
    </tr>
    <tr>
      <th>1976</th>
      <td>33465</td>
      <td>31364</td>
    </tr>
    <tr>
      <th>1977</th>
      <td>31954</td>
      <td>30318</td>
    </tr>
    <tr>
      <th>1978</th>
      <td>33031</td>
      <td>31221</td>
    </tr>
    <tr>
      <th>1979</th>
      <td>35323</td>
      <td>33004</td>
    </tr>
    <tr>
      <th>1980</th>
      <td>35367</td>
      <td>33474</td>
    </tr>
    <tr>
      <th>1981</th>
      <td>35266</td>
      <td>33755</td>
    </tr>
    <tr>
      <th>1982</th>
      <td>33895</td>
      <td>32270</td>
    </tr>
    <tr>
      <th>1983</th>
      <td>33651</td>
      <td>31419</td>
    </tr>
  </tbody>
</table>
</div>



Visualise the above table to investigate the birth rate trend of Scotland in the last five decades


```python
total_number_by_year.plot.line(title="Total births by year and sex",
                            style={'B': "#89CFF0", 'G': 'pink'},
                            xlabel="year",ylabel="births",grid=True,figsize=(10,8)) 
```




    <Axes: title={'center': 'Total births by year and sex'}, xlabel='year', ylabel='births'>




    
![png](output_15_1.png)
    


At first glance, I thought something was off, as it appears there are always more boys than girls in Scotland, which seems a little strange to me.
I then performed a check online and can confirm that it indeed is the case in Scotland.

Fun fact: The disparity shrank to 949 in 2010, the year with the smallest difference to date.

Next, I am interested to know which boy's name ranked most frequently in the top 10 over the years.


```python
most_popular_b = names[(names["rank"]<=10) & (names["sex"]=="B")].groupby(["sex","FirstForename"])["number"].count().reset_index(name="count")
most_popular_b.nlargest(1,["count"],keep="all")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sex</th>
      <th>FirstForename</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>21</th>
      <td>B</td>
      <td>James</td>
      <td>49</td>
    </tr>
  </tbody>
</table>
</div>



The dataset contains data from 1974–2022 (49 years), and James is the only name that has been ranked in the top 10 boy names every year. 

Which girl's name ranked most frequently on the top 10 over the years?


```python
most_popular_g = names[(names["rank"]<=10) & (names["sex"]=="G")].groupby(["sex","FirstForename"])["number"].count().reset_index(name="count")
most_popular_g.nlargest(1,["count"],keep="all")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sex</th>
      <th>FirstForename</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>13</th>
      <td>G</td>
      <td>Emma</td>
      <td>31</td>
    </tr>
  </tbody>
</table>
</div>



Between 1974 and 2022, the name Emma appeared 31 times in the top 10 girl names.

Another approach to the above questions


```python
most_popular = names[names["rank"]<=10].groupby(["sex","FirstForename"],as_index=False)["number"].count()
most_popular.groupby("sex").apply(lambda x:x.FirstForename[x.number.idxmax()])
```




    sex
    B    James
    G     Emma
    dtype: object



Next, let's insert a column that shows the percentage of babies given a particular name in a given year.


```python
def add_pct(group):
    group["pct"]=100.0*group["number"]/group["number"].sum()
    return group

names=names.groupby(["yr","sex"],as_index=False).apply(add_pct).droplevel(0)
```


```python
names.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>yr</th>
      <th>sex</th>
      <th>FirstForename</th>
      <th>number</th>
      <th>rank</th>
      <th>pct</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1974</td>
      <td>B</td>
      <td>Aamir</td>
      <td>1</td>
      <td>472</td>
      <td>0.002795</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1974</td>
      <td>B</td>
      <td>Aaron</td>
      <td>17</td>
      <td>139</td>
      <td>0.047522</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1974</td>
      <td>B</td>
      <td>Abdul</td>
      <td>5</td>
      <td>248</td>
      <td>0.013977</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1974</td>
      <td>B</td>
      <td>Abdullahi</td>
      <td>1</td>
      <td>472</td>
      <td>0.002795</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1974</td>
      <td>B</td>
      <td>Abdulrazak</td>
      <td>1</td>
      <td>472</td>
      <td>0.002795</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1974</td>
      <td>B</td>
      <td>Abid</td>
      <td>2</td>
      <td>346</td>
      <td>0.005591</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1974</td>
      <td>B</td>
      <td>Abraham</td>
      <td>1</td>
      <td>472</td>
      <td>0.002795</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1974</td>
      <td>B</td>
      <td>Adam</td>
      <td>75</td>
      <td>78</td>
      <td>0.209655</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1974</td>
      <td>B</td>
      <td>Adebayo</td>
      <td>1</td>
      <td>472</td>
      <td>0.002795</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1974</td>
      <td>B</td>
      <td>Adel</td>
      <td>1</td>
      <td>472</td>
      <td>0.002795</td>
    </tr>
  </tbody>
</table>
</div>



Take Adam as an example: there were 35773 baby boys registered in 1974, and 75 of them were named Adam. That accounts for 0.209655%.

Just to ensure the pct column adds to 100% across all groupings.


```python
names.groupby(["yr","sex"])["pct"].sum()
```




    yr    sex
    1974  B      100.0
          G      100.0
    1975  B      100.0
          G      100.0
    1976  B      100.0
                 ...  
    2020  G      100.0
    2021  B      100.0
          G      100.0
    2022  B      100.0
          G      100.0
    Name: pct, Length: 98, dtype: float64



Next, I'd like to slightly alter the question to see if I can make any new discoveries.

<br>Top 10 name(s) given to a boy relative to the total number of boys in a particular year


```python
names[names["sex"]=="B"].nlargest(10,"pct",keep="all")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>yr</th>
      <th>sex</th>
      <th>FirstForename</th>
      <th>number</th>
      <th>rank</th>
      <th>pct</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>16550</th>
      <td>1979</td>
      <td>B</td>
      <td>David</td>
      <td>2024</td>
      <td>1</td>
      <td>5.729978</td>
    </tr>
    <tr>
      <th>13147</th>
      <td>1978</td>
      <td>B</td>
      <td>David</td>
      <td>1861</td>
      <td>1</td>
      <td>5.634101</td>
    </tr>
    <tr>
      <th>20039</th>
      <td>1980</td>
      <td>B</td>
      <td>David</td>
      <td>1988</td>
      <td>1</td>
      <td>5.621059</td>
    </tr>
    <tr>
      <th>30657</th>
      <td>1983</td>
      <td>B</td>
      <td>David</td>
      <td>1891</td>
      <td>1</td>
      <td>5.619447</td>
    </tr>
    <tr>
      <th>23525</th>
      <td>1981</td>
      <td>B</td>
      <td>David</td>
      <td>1965</td>
      <td>1</td>
      <td>5.571939</td>
    </tr>
    <tr>
      <th>9837</th>
      <td>1977</td>
      <td>B</td>
      <td>David</td>
      <td>1761</td>
      <td>1</td>
      <td>5.511047</td>
    </tr>
    <tr>
      <th>27134</th>
      <td>1982</td>
      <td>B</td>
      <td>David</td>
      <td>1856</td>
      <td>1</td>
      <td>5.475734</td>
    </tr>
    <tr>
      <th>34228</th>
      <td>1984</td>
      <td>B</td>
      <td>David</td>
      <td>1785</td>
      <td>1</td>
      <td>5.386890</td>
    </tr>
    <tr>
      <th>6586</th>
      <td>1976</td>
      <td>B</td>
      <td>David</td>
      <td>1744</td>
      <td>1</td>
      <td>5.211415</td>
    </tr>
    <tr>
      <th>37875</th>
      <td>1985</td>
      <td>B</td>
      <td>David</td>
      <td>1769</td>
      <td>1</td>
      <td>5.185706</td>
    </tr>
  </tbody>
</table>
</div>



David was the most popular name for baby boys between the 70s and the 80s. Over 5 out of every 100 baby boys were given this particular name.

Top 10 name(s) given to a girl relative to the total number of girls in a particular year


```python
names[names["sex"]=="G"].nlargest(10,"pct",keep="all")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>yr</th>
      <th>sex</th>
      <th>FirstForename</th>
      <th>number</th>
      <th>rank</th>
      <th>pct</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>40156</th>
      <td>1985</td>
      <td>G</td>
      <td>Laura</td>
      <td>1266</td>
      <td>1</td>
      <td>3.889401</td>
    </tr>
    <tr>
      <th>32778</th>
      <td>1983</td>
      <td>G</td>
      <td>Laura</td>
      <td>1215</td>
      <td>1</td>
      <td>3.867087</td>
    </tr>
    <tr>
      <th>36375</th>
      <td>1984</td>
      <td>G</td>
      <td>Laura</td>
      <td>1185</td>
      <td>1</td>
      <td>3.708224</td>
    </tr>
    <tr>
      <th>25724</th>
      <td>1981</td>
      <td>G</td>
      <td>Laura</td>
      <td>1181</td>
      <td>1</td>
      <td>3.498741</td>
    </tr>
    <tr>
      <th>47810</th>
      <td>1987</td>
      <td>G</td>
      <td>Laura</td>
      <td>1119</td>
      <td>1</td>
      <td>3.467724</td>
    </tr>
    <tr>
      <th>29298</th>
      <td>1982</td>
      <td>G</td>
      <td>Laura</td>
      <td>1111</td>
      <td>1</td>
      <td>3.442826</td>
    </tr>
    <tr>
      <th>96173</th>
      <td>1998</td>
      <td>G</td>
      <td>Chloe</td>
      <td>953</td>
      <td>1</td>
      <td>3.425224</td>
    </tr>
    <tr>
      <th>100920</th>
      <td>1999</td>
      <td>G</td>
      <td>Chloe</td>
      <td>917</td>
      <td>1</td>
      <td>3.408795</td>
    </tr>
    <tr>
      <th>51800</th>
      <td>1988</td>
      <td>G</td>
      <td>Laura</td>
      <td>1095</td>
      <td>1</td>
      <td>3.406016</td>
    </tr>
    <tr>
      <th>43911</th>
      <td>1986</td>
      <td>G</td>
      <td>Laura</td>
      <td>1038</td>
      <td>1</td>
      <td>3.250352</td>
    </tr>
  </tbody>
</table>
</div>



And for girls, it's the name Laura that was shared by many baby girls throughout the 80s.

Now we have two pairs of names:
1) James and Emma appeared most frequently in the top 10 boy names and top 10 girl names, respectively
2) David and Laura were once the names shared by most boys and most girls, respectively at a given year  

How does the trend for these names go?


```python
trend = names.pivot_table("number",index="yr",columns="FirstForename",aggfunc=sum)
popular_names = trend[["James","Emma","David","Laura"]]
popular_names.plot(subplots=True,figsize=(10,8),
                   title="Number of births per year",
                   sharey=True,
                   sharex=True,
                   xlabel="year",
                   ylabel="births",
                   color={'James': "#89CFF0", "Emma": 'pink','David': "#89CFF0", "Laura": 'pink'})
```




    array([<Axes: xlabel='year', ylabel='births'>,
           <Axes: xlabel='year', ylabel='births'>,
           <Axes: xlabel='year', ylabel='births'>,
           <Axes: xlabel='year', ylabel='births'>], dtype=object)




    
![png](output_38_1.png)
    


*Please note that the above data manipulation neglected a very small number of individuals with the same name but the opposite sex. For example, for every 2917 boys named James, only one girl also named James. Similarly, for every 4010 girls named Emma, only one boy also named Emma. Given the significant  disparity, I believe it has no impact on the outcomes.*

James and Emma, the names that had the most appearances on the top 10 ranking list over the years, show a more stable and consistent trend than David and Laura, whose popularity surged in the 80s and 90s and then fell out of favour ever since.

The decline in popularity of names above led me to consider another hypothesis: could it be that parents of the post-'90s generation tend to choose unique names for their babies?


```python
cumsum=names.sort_values(by=["yr","pct"],ascending=[True,False])
cumsum["cumsum"]=cumsum.groupby(["yr","sex"])["pct"].transform(pd.Series.cumsum)
cumsum
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>yr</th>
      <th>sex</th>
      <th>FirstForename</th>
      <th>number</th>
      <th>rank</th>
      <th>pct</th>
      <th>cumsum</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>272</th>
      <td>1974</td>
      <td>B</td>
      <td>David</td>
      <td>1794</td>
      <td>1</td>
      <td>5.014955</td>
      <td>5.014955</td>
    </tr>
    <tr>
      <th>553</th>
      <td>1974</td>
      <td>B</td>
      <td>John</td>
      <td>1528</td>
      <td>2</td>
      <td>4.271378</td>
      <td>9.286333</td>
    </tr>
    <tr>
      <th>856</th>
      <td>1974</td>
      <td>B</td>
      <td>Paul</td>
      <td>1260</td>
      <td>3</td>
      <td>3.522209</td>
      <td>12.808543</td>
    </tr>
    <tr>
      <th>724</th>
      <td>1974</td>
      <td>B</td>
      <td>Mark</td>
      <td>1234</td>
      <td>4</td>
      <td>3.449529</td>
      <td>16.258072</td>
    </tr>
    <tr>
      <th>514</th>
      <td>1974</td>
      <td>B</td>
      <td>James</td>
      <td>1202</td>
      <td>5</td>
      <td>3.360076</td>
      <td>19.618148</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>259737</th>
      <td>2022</td>
      <td>B</td>
      <td>Zubayr</td>
      <td>1</td>
      <td>1328</td>
      <td>0.004123</td>
      <td>99.983509</td>
    </tr>
    <tr>
      <th>259738</th>
      <td>2022</td>
      <td>B</td>
      <td>Zuhair</td>
      <td>1</td>
      <td>1328</td>
      <td>0.004123</td>
      <td>99.987631</td>
    </tr>
    <tr>
      <th>259739</th>
      <td>2022</td>
      <td>B</td>
      <td>Zureb</td>
      <td>1</td>
      <td>1328</td>
      <td>0.004123</td>
      <td>99.991754</td>
    </tr>
    <tr>
      <th>259740</th>
      <td>2022</td>
      <td>B</td>
      <td>Zuriel</td>
      <td>1</td>
      <td>1328</td>
      <td>0.004123</td>
      <td>99.995877</td>
    </tr>
    <tr>
      <th>259741</th>
      <td>2022</td>
      <td>B</td>
      <td>Zyle</td>
      <td>1</td>
      <td>1328</td>
      <td>0.004123</td>
      <td>100.000000</td>
    </tr>
  </tbody>
</table>
<p>264149 rows × 7 columns</p>
</div>




```python
cumsum=cumsum[cumsum["cumsum"]<=30]
cumsum_per_year = cumsum.groupby(["yr","sex"])["cumsum"].count()+1
cumsum_per_year
```




    yr    sex
    1974  B       9
          G      17
    1975  B       9
          G      18
    1976  B      10
                 ..
    2020  G      39
    2021  B      36
          G      40
    2022  B      36
          G      41
    Name: cumsum, Length: 98, dtype: int64




```python
cumsum_per_year.unstack().plot(color={'B': "#89CFF0", 'G': 'pink'},
                              title="Number of popular names in the top 30%",
                              xlabel="year",
                              figsize=(12,8))
```




    <Axes: title={'center': 'Number of popular names in the top 30%'}, xlabel='year'>




    
![png](output_44_1.png)
    


As we can see, girls' names have consistently been more varied than boys' names. Additionally, it seems that there indeed has been a rise in name diversity since the 90s. It could be attributed to parents opting for unique variations of classic names or perhaps being influenced by popular culture at that time.

Next, we look at the distribution of baby names by first letter.


```python
def get_first_letter(x):
    return x[0]

first_letters = names["FirstForename"].map(get_first_letter)
first_letters.name="first_letter"

table=names.pivot_table("number",index=first_letters,columns=["sex","yr"],aggfunc=sum)

subset = table.reindex(columns=[1980,1990,2000,2010,2020],level="yr")
subset
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>sex</th>
      <th colspan="5" halign="left">B</th>
      <th colspan="5" halign="left">G</th>
    </tr>
    <tr>
      <th>yr</th>
      <th>1980</th>
      <th>1990</th>
      <th>2000</th>
      <th>2010</th>
      <th>2020</th>
      <th>1980</th>
      <th>1990</th>
      <th>2000</th>
      <th>2010</th>
      <th>2020</th>
    </tr>
    <tr>
      <th>first_letter</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>A</th>
      <td>3637.0</td>
      <td>2931.0</td>
      <td>2508.0</td>
      <td>3342.0</td>
      <td>2757.0</td>
      <td>2662.0</td>
      <td>2976.0</td>
      <td>2810.0</td>
      <td>3786.0</td>
      <td>3398.0</td>
    </tr>
    <tr>
      <th>B</th>
      <td>1424.0</td>
      <td>835.0</td>
      <td>1180.0</td>
      <td>1036.0</td>
      <td>994.0</td>
      <td>181.0</td>
      <td>209.0</td>
      <td>725.0</td>
      <td>704.0</td>
      <td>624.0</td>
    </tr>
    <tr>
      <th>C</th>
      <td>2806.0</td>
      <td>3579.0</td>
      <td>3071.0</td>
      <td>2988.0</td>
      <td>2068.0</td>
      <td>3244.0</td>
      <td>2782.0</td>
      <td>3231.0</td>
      <td>1989.0</td>
      <td>1013.0</td>
    </tr>
    <tr>
      <th>D</th>
      <td>3574.0</td>
      <td>3534.0</td>
      <td>2036.0</td>
      <td>1589.0</td>
      <td>730.0</td>
      <td>1621.0</td>
      <td>1208.0</td>
      <td>464.0</td>
      <td>548.0</td>
      <td>464.0</td>
    </tr>
    <tr>
      <th>E</th>
      <td>425.0</td>
      <td>433.0</td>
      <td>734.0</td>
      <td>1018.0</td>
      <td>819.0</td>
      <td>1574.0</td>
      <td>1752.0</td>
      <td>2429.0</td>
      <td>3348.0</td>
      <td>2711.0</td>
    </tr>
    <tr>
      <th>F</th>
      <td>324.0</td>
      <td>409.0</td>
      <td>536.0</td>
      <td>946.0</td>
      <td>1250.0</td>
      <td>745.0</td>
      <td>464.0</td>
      <td>222.0</td>
      <td>544.0</td>
      <td>671.0</td>
    </tr>
    <tr>
      <th>G</th>
      <td>3069.0</td>
      <td>2228.0</td>
      <td>654.0</td>
      <td>392.0</td>
      <td>394.0</td>
      <td>940.0</td>
      <td>822.0</td>
      <td>434.0</td>
      <td>577.0</td>
      <td>660.0</td>
    </tr>
    <tr>
      <th>H</th>
      <td>155.0</td>
      <td>171.0</td>
      <td>369.0</td>
      <td>945.0</td>
      <td>1179.0</td>
      <td>669.0</td>
      <td>1007.0</td>
      <td>1007.0</td>
      <td>1053.0</td>
      <td>1163.0</td>
    </tr>
    <tr>
      <th>I</th>
      <td>789.0</td>
      <td>469.0</td>
      <td>173.0</td>
      <td>261.0</td>
      <td>356.0</td>
      <td>135.0</td>
      <td>110.0</td>
      <td>336.0</td>
      <td>961.0</td>
      <td>1072.0</td>
    </tr>
    <tr>
      <th>J</th>
      <td>3734.0</td>
      <td>3900.0</td>
      <td>3931.0</td>
      <td>3614.0</td>
      <td>2367.0</td>
      <td>2937.0</td>
      <td>2424.0</td>
      <td>1402.0</td>
      <td>937.0</td>
      <td>546.0</td>
    </tr>
    <tr>
      <th>K</th>
      <td>1573.0</td>
      <td>1210.0</td>
      <td>1401.0</td>
      <td>1654.0</td>
      <td>893.0</td>
      <td>3064.0</td>
      <td>3347.0</td>
      <td>2035.0</td>
      <td>2008.0</td>
      <td>633.0</td>
    </tr>
    <tr>
      <th>L</th>
      <td>492.0</td>
      <td>1069.0</td>
      <td>2088.0</td>
      <td>2853.0</td>
      <td>2138.0</td>
      <td>6133.0</td>
      <td>3918.0</td>
      <td>2220.0</td>
      <td>2713.0</td>
      <td>1731.0</td>
    </tr>
    <tr>
      <th>M</th>
      <td>2650.0</td>
      <td>3074.0</td>
      <td>1819.0</td>
      <td>2037.0</td>
      <td>1648.0</td>
      <td>2027.0</td>
      <td>1678.0</td>
      <td>1956.0</td>
      <td>2763.0</td>
      <td>2315.0</td>
    </tr>
    <tr>
      <th>N</th>
      <td>735.0</td>
      <td>606.0</td>
      <td>529.0</td>
      <td>682.0</td>
      <td>663.0</td>
      <td>1233.0</td>
      <td>1926.0</td>
      <td>1089.0</td>
      <td>894.0</td>
      <td>635.0</td>
    </tr>
    <tr>
      <th>O</th>
      <td>67.0</td>
      <td>110.0</td>
      <td>277.0</td>
      <td>928.0</td>
      <td>939.0</td>
      <td>18.0</td>
      <td>34.0</td>
      <td>252.0</td>
      <td>763.0</td>
      <td>633.0</td>
    </tr>
    <tr>
      <th>P</th>
      <td>1708.0</td>
      <td>1047.0</td>
      <td>377.0</td>
      <td>261.0</td>
      <td>229.0</td>
      <td>800.0</td>
      <td>304.0</td>
      <td>195.0</td>
      <td>487.0</td>
      <td>560.0</td>
    </tr>
    <tr>
      <th>Q</th>
      <td>4.0</td>
      <td>11.0</td>
      <td>13.0</td>
      <td>22.0</td>
      <td>29.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>11.0</td>
      <td>58.0</td>
    </tr>
    <tr>
      <th>R</th>
      <td>2822.0</td>
      <td>2731.0</td>
      <td>2552.0</td>
      <td>2229.0</td>
      <td>1833.0</td>
      <td>781.0</td>
      <td>1777.0</td>
      <td>1902.0</td>
      <td>1231.0</td>
      <td>1249.0</td>
    </tr>
    <tr>
      <th>S</th>
      <td>3969.0</td>
      <td>4402.0</td>
      <td>1858.0</td>
      <td>1331.0</td>
      <td>787.0</td>
      <td>3081.0</td>
      <td>4150.0</td>
      <td>2134.0</td>
      <td>2283.0</td>
      <td>1578.0</td>
    </tr>
    <tr>
      <th>T</th>
      <td>649.0</td>
      <td>576.0</td>
      <td>672.0</td>
      <td>885.0</td>
      <td>1113.0</td>
      <td>688.0</td>
      <td>440.0</td>
      <td>574.0</td>
      <td>554.0</td>
      <td>293.0</td>
    </tr>
    <tr>
      <th>U</th>
      <td>5.0</td>
      <td>16.0</td>
      <td>20.0</td>
      <td>21.0</td>
      <td>14.0</td>
      <td>7.0</td>
      <td>8.0</td>
      <td>14.0</td>
      <td>23.0</td>
      <td>22.0</td>
    </tr>
    <tr>
      <th>V</th>
      <td>23.0</td>
      <td>27.0</td>
      <td>22.0</td>
      <td>69.0</td>
      <td>77.0</td>
      <td>573.0</td>
      <td>440.0</td>
      <td>118.0</td>
      <td>145.0</td>
      <td>206.0</td>
    </tr>
    <tr>
      <th>W</th>
      <td>699.0</td>
      <td>480.0</td>
      <td>222.0</td>
      <td>301.0</td>
      <td>213.0</td>
      <td>112.0</td>
      <td>30.0</td>
      <td>23.0</td>
      <td>84.0</td>
      <td>213.0</td>
    </tr>
    <tr>
      <th>X</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.0</td>
      <td>38.0</td>
      <td>37.0</td>
      <td>NaN</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>13.0</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>Y</th>
      <td>15.0</td>
      <td>16.0</td>
      <td>16.0</td>
      <td>66.0</td>
      <td>87.0</td>
      <td>170.0</td>
      <td>82.0</td>
      <td>55.0</td>
      <td>81.0</td>
      <td>50.0</td>
    </tr>
    <tr>
      <th>Z</th>
      <td>19.0</td>
      <td>32.0</td>
      <td>130.0</td>
      <td>360.0</td>
      <td>354.0</td>
      <td>78.0</td>
      <td>178.0</td>
      <td>247.0</td>
      <td>419.0</td>
      <td>330.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
first_letter_pct = 100.0*subset/subset.sum()
first_letter_pct
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>sex</th>
      <th colspan="5" halign="left">B</th>
      <th colspan="5" halign="left">G</th>
    </tr>
    <tr>
      <th>yr</th>
      <th>1980</th>
      <th>1990</th>
      <th>2000</th>
      <th>2010</th>
      <th>2020</th>
      <th>1980</th>
      <th>1990</th>
      <th>2000</th>
      <th>2010</th>
      <th>2020</th>
    </tr>
    <tr>
      <th>first_letter</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>A</th>
      <td>10.283598</td>
      <td>8.647038</td>
      <td>9.221944</td>
      <td>11.189233</td>
      <td>11.502837</td>
      <td>7.952441</td>
      <td>9.279411</td>
      <td>10.857805</td>
      <td>13.091739</td>
      <td>14.879362</td>
    </tr>
    <tr>
      <th>B</th>
      <td>4.026352</td>
      <td>2.463418</td>
      <td>4.338873</td>
      <td>3.468595</td>
      <td>4.147196</td>
      <td>0.540718</td>
      <td>0.651679</td>
      <td>2.801391</td>
      <td>2.434386</td>
      <td>2.732408</td>
    </tr>
    <tr>
      <th>C</th>
      <td>7.933950</td>
      <td>10.558768</td>
      <td>11.292102</td>
      <td>10.004018</td>
      <td>8.628171</td>
      <td>9.691104</td>
      <td>8.674503</td>
      <td>12.484544</td>
      <td>6.877831</td>
      <td>4.435784</td>
    </tr>
    <tr>
      <th>D</th>
      <td>10.105466</td>
      <td>10.426009</td>
      <td>7.486395</td>
      <td>5.320075</td>
      <td>3.045728</td>
      <td>4.842564</td>
      <td>3.766643</td>
      <td>1.792890</td>
      <td>1.894948</td>
      <td>2.031791</td>
    </tr>
    <tr>
      <th>E</th>
      <td>1.201685</td>
      <td>1.277437</td>
      <td>2.698926</td>
      <td>3.408330</td>
      <td>3.417056</td>
      <td>4.702157</td>
      <td>5.462879</td>
      <td>9.385626</td>
      <td>11.577164</td>
      <td>11.871086</td>
    </tr>
    <tr>
      <th>F</th>
      <td>0.916108</td>
      <td>1.206632</td>
      <td>1.970878</td>
      <td>3.167269</td>
      <td>5.215287</td>
      <td>2.225608</td>
      <td>1.446790</td>
      <td>0.857805</td>
      <td>1.881116</td>
      <td>2.938214</td>
    </tr>
    <tr>
      <th>G</th>
      <td>8.677581</td>
      <td>6.573047</td>
      <td>2.404765</td>
      <td>1.312441</td>
      <td>1.643858</td>
      <td>2.808150</td>
      <td>2.563063</td>
      <td>1.676971</td>
      <td>1.995228</td>
      <td>2.890047</td>
    </tr>
    <tr>
      <th>H</th>
      <td>0.438262</td>
      <td>0.504484</td>
      <td>1.356817</td>
      <td>3.163921</td>
      <td>4.919059</td>
      <td>1.998566</td>
      <td>3.139908</td>
      <td>3.891036</td>
      <td>3.641205</td>
      <td>5.092613</td>
    </tr>
    <tr>
      <th>I</th>
      <td>2.230893</td>
      <td>1.383644</td>
      <td>0.636123</td>
      <td>0.873845</td>
      <td>1.485314</td>
      <td>0.403298</td>
      <td>0.342989</td>
      <td>1.298300</td>
      <td>3.323075</td>
      <td>4.694137</td>
    </tr>
    <tr>
      <th>J</th>
      <td>10.557865</td>
      <td>11.505782</td>
      <td>14.454332</td>
      <td>12.099906</td>
      <td>9.875668</td>
      <td>8.773974</td>
      <td>7.558230</td>
      <td>5.417311</td>
      <td>3.240084</td>
      <td>2.390857</td>
    </tr>
    <tr>
      <th>K</th>
      <td>4.447649</td>
      <td>3.569743</td>
      <td>5.151493</td>
      <td>5.537699</td>
      <td>3.725801</td>
      <td>9.153373</td>
      <td>10.436220</td>
      <td>7.863215</td>
      <td>6.943532</td>
      <td>2.771818</td>
    </tr>
    <tr>
      <th>L</th>
      <td>1.391127</td>
      <td>3.153764</td>
      <td>7.677600</td>
      <td>9.552029</td>
      <td>8.920227</td>
      <td>18.321682</td>
      <td>12.216644</td>
      <td>8.578053</td>
      <td>9.381376</td>
      <td>7.579805</td>
    </tr>
    <tr>
      <th>M</th>
      <td>7.492861</td>
      <td>9.068917</td>
      <td>6.688484</td>
      <td>6.820008</td>
      <td>6.875834</td>
      <td>6.055446</td>
      <td>5.232141</td>
      <td>7.557960</td>
      <td>9.554272</td>
      <td>10.137058</td>
    </tr>
    <tr>
      <th>N</th>
      <td>2.078208</td>
      <td>1.787822</td>
      <td>1.945139</td>
      <td>2.283380</td>
      <td>2.766188</td>
      <td>3.683456</td>
      <td>6.005425</td>
      <td>4.207883</td>
      <td>3.091393</td>
      <td>2.780575</td>
    </tr>
    <tr>
      <th>O</th>
      <td>0.189442</td>
      <td>0.324522</td>
      <td>1.018532</td>
      <td>3.107004</td>
      <td>3.917724</td>
      <td>0.053773</td>
      <td>0.106015</td>
      <td>0.973725</td>
      <td>2.638404</td>
      <td>2.771818</td>
    </tr>
    <tr>
      <th>P</th>
      <td>4.829361</td>
      <td>3.088860</td>
      <td>1.386233</td>
      <td>0.873845</td>
      <td>0.955441</td>
      <td>2.389915</td>
      <td>0.947897</td>
      <td>0.753478</td>
      <td>1.684014</td>
      <td>2.452161</td>
    </tr>
    <tr>
      <th>Q</th>
      <td>0.011310</td>
      <td>0.032452</td>
      <td>0.047801</td>
      <td>0.073657</td>
      <td>0.120995</td>
      <td>0.002987</td>
      <td>0.006236</td>
      <td>NaN</td>
      <td>0.038037</td>
      <td>0.253974</td>
    </tr>
    <tr>
      <th>R</th>
      <td>7.979190</td>
      <td>8.056998</td>
      <td>9.383733</td>
      <td>7.462836</td>
      <td>7.647697</td>
      <td>2.333154</td>
      <td>5.540831</td>
      <td>7.349304</td>
      <td>4.256717</td>
      <td>5.469195</td>
    </tr>
    <tr>
      <th>S</th>
      <td>11.222326</td>
      <td>12.986783</td>
      <td>6.831887</td>
      <td>4.456274</td>
      <td>3.283545</td>
      <td>9.204158</td>
      <td>12.940039</td>
      <td>8.245750</td>
      <td>7.894464</td>
      <td>6.909839</td>
    </tr>
    <tr>
      <th>T</th>
      <td>1.835044</td>
      <td>1.699316</td>
      <td>2.470952</td>
      <td>2.963037</td>
      <td>4.643692</td>
      <td>2.055327</td>
      <td>1.371956</td>
      <td>2.217929</td>
      <td>1.915696</td>
      <td>1.283006</td>
    </tr>
    <tr>
      <th>U</th>
      <td>0.014137</td>
      <td>0.047203</td>
      <td>0.073540</td>
      <td>0.070309</td>
      <td>0.058411</td>
      <td>0.020912</td>
      <td>0.024945</td>
      <td>0.054096</td>
      <td>0.079532</td>
      <td>0.096335</td>
    </tr>
    <tr>
      <th>V</th>
      <td>0.065032</td>
      <td>0.079655</td>
      <td>0.080894</td>
      <td>0.231016</td>
      <td>0.321262</td>
      <td>1.711776</td>
      <td>1.371956</td>
      <td>0.455951</td>
      <td>0.501400</td>
      <td>0.902045</td>
    </tr>
    <tr>
      <th>W</th>
      <td>1.976419</td>
      <td>1.416096</td>
      <td>0.816297</td>
      <td>1.007768</td>
      <td>0.888685</td>
      <td>0.334588</td>
      <td>0.093542</td>
      <td>0.088872</td>
      <td>0.290466</td>
      <td>0.932697</td>
    </tr>
    <tr>
      <th>X</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.029416</td>
      <td>0.127226</td>
      <td>0.154372</td>
      <td>NaN</td>
      <td>0.009354</td>
      <td>0.023184</td>
      <td>0.044953</td>
      <td>0.039410</td>
    </tr>
    <tr>
      <th>Y</th>
      <td>0.042412</td>
      <td>0.047203</td>
      <td>0.058832</td>
      <td>0.220972</td>
      <td>0.362984</td>
      <td>0.507857</td>
      <td>0.255683</td>
      <td>0.212519</td>
      <td>0.280093</td>
      <td>0.218943</td>
    </tr>
    <tr>
      <th>Z</th>
      <td>0.053722</td>
      <td>0.094406</td>
      <td>0.478011</td>
      <td>1.205303</td>
      <td>1.476969</td>
      <td>0.233017</td>
      <td>0.555019</td>
      <td>0.954405</td>
      <td>1.448874</td>
      <td>1.445023</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig,axes = plt.subplots(2,1,figsize=(18,12))
first_letter_pct["B"].plot(kind="bar",rot=0,ax=axes[0],title="Proportion of boy names starting in each letter",width=0.8)
first_letter_pct["G"].plot(kind="bar",rot=0,ax=axes[1],title="Proportion of girl names starting in each letter",width=0.8)

fig.subplots_adjust(wspace=0.1,hspace=0.3)
```


    
![png](output_49_0.png)
    


For boys, the first letters F, H, and O have experienced significant growth, while letters D, G, P, and S have experienced a significant decline.

For girls, the first letters A, E, and I have experienced significant growth, while letters D, J, K, and S have experienced a significant decline.

I'll choose a subset of letters for the girl's names and the boy's names separately and chart their historical trends.


```python
first_letter_pct_full = 100.0*table / table.sum()
trend_lines_b = first_letter_pct_full.loc[["H","D"],"B"].T
trend_lines_b.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>first_letter</th>
      <th>H</th>
      <th>D</th>
    </tr>
    <tr>
      <th>yr</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1974</th>
      <td>0.598217</td>
      <td>10.124955</td>
    </tr>
    <tr>
      <th>1975</th>
      <td>0.514801</td>
      <td>10.118690</td>
    </tr>
    <tr>
      <th>1976</th>
      <td>0.531899</td>
      <td>10.410877</td>
    </tr>
    <tr>
      <th>1977</th>
      <td>0.485072</td>
      <td>10.239720</td>
    </tr>
    <tr>
      <th>1978</th>
      <td>0.454119</td>
      <td>10.487118</td>
    </tr>
  </tbody>
</table>
</div>




```python
trend_lines_g = first_letter_pct_full.loc[["A","S"],"G"].T
trend_lines_g.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>first_letter</th>
      <th>A</th>
      <th>S</th>
    </tr>
    <tr>
      <th>yr</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1974</th>
      <td>9.763936</td>
      <td>10.541077</td>
    </tr>
    <tr>
      <th>1975</th>
      <td>9.730371</td>
      <td>10.192419</td>
    </tr>
    <tr>
      <th>1976</th>
      <td>9.383369</td>
      <td>9.756409</td>
    </tr>
    <tr>
      <th>1977</th>
      <td>9.004552</td>
      <td>9.601557</td>
    </tr>
    <tr>
      <th>1978</th>
      <td>8.551936</td>
      <td>9.484001</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig,axes=plt.subplots(2,1,figsize=(12,10))
trend_lines_b.plot(kind="line",rot=0,ax=axes[0],title="Proportion of boys born with names starting in H vs D over time",xlabel="year")
trend_lines_g.plot(kind="line",rot=0,ax=axes[1],title="Proportion of girls born with names starting in A vs S over time",xlabel="year")
```




    <Axes: title={'center': 'Proportion of girls born with names starting in A vs S over time'}, xlabel='year'>




    
![png](output_54_1.png)
    


Gender-neutral names have become more popular recently, reflecting shifting views on gender roles.

To find out popular unisex names, we first select those registered by at least, say, 500 boys and 500 girls between 1974 and 2022.


```python
names_subtable=names[["yr","sex","FirstForename","number"]]
b_names = names_subtable[names_subtable["sex"]=="B"]
b_names = b_names.groupby("FirstForename").filter(lambda x: x["number"].sum()>=500).drop_duplicates(subset="FirstForename")["FirstForename"]
b_names.count()
```




    291




```python
g_names = names_subtable[names_subtable["sex"]=="G"]
g_names = g_names.groupby("FirstForename").filter(lambda x: x["number"].sum()>=500).drop_duplicates(subset="FirstForename")["FirstForename"]
g_names.count()
```




    409




```python
unisex_names = pd.merge(b_names,g_names,how="inner",on=["FirstForename"])["FirstForename"]
unisex_names
```




    0       Alex
    1      Ellis
    2      Jamie
    3        Lee
    4     Morgan
    5      Rowan
    6     Taylor
    7    Charlie
    8     Jordan
    9     Harley
    Name: FirstForename, dtype: object



Now we have 10 names and each of the names was at least registered by 500 boys and 500 girls. 

Going back to the full table with yearly numbers, we can dig further into the story.


```python
names_subtable = names_subtable[names_subtable["FirstForename"].isin(unisex_names)]
names_subtable
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>yr</th>
      <th>sex</th>
      <th>FirstForename</th>
      <th>number</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>37</th>
      <td>1974</td>
      <td>B</td>
      <td>Alex</td>
      <td>4</td>
    </tr>
    <tr>
      <th>345</th>
      <td>1974</td>
      <td>B</td>
      <td>Ellis</td>
      <td>1</td>
    </tr>
    <tr>
      <th>515</th>
      <td>1974</td>
      <td>B</td>
      <td>Jamie</td>
      <td>88</td>
    </tr>
    <tr>
      <th>659</th>
      <td>1974</td>
      <td>B</td>
      <td>Lee</td>
      <td>209</td>
    </tr>
    <tr>
      <th>768</th>
      <td>1974</td>
      <td>B</td>
      <td>Morgan</td>
      <td>4</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>260075</th>
      <td>2022</td>
      <td>G</td>
      <td>Ellis</td>
      <td>10</td>
    </tr>
    <tr>
      <th>260122</th>
      <td>2022</td>
      <td>G</td>
      <td>Alex</td>
      <td>8</td>
    </tr>
    <tr>
      <th>260251</th>
      <td>2022</td>
      <td>G</td>
      <td>Morgan</td>
      <td>6</td>
    </tr>
    <tr>
      <th>260268</th>
      <td>2022</td>
      <td>G</td>
      <td>Taylor</td>
      <td>6</td>
    </tr>
    <tr>
      <th>260309</th>
      <td>2022</td>
      <td>G</td>
      <td>Jamie</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
<p>856 rows × 4 columns</p>
</div>



Since the dataset contains data for 49 years, we conveniently split them into seven periods.


```python
names_subtable["period"] = names_subtable["yr"].apply(lambda x: x//7*7)
names_subtable["period"] =np.where(names_subtable["period"]==1974,"1974-1980",
    np.where(names_subtable["period"]==1981,"1981-1987",
    np.where(names_subtable["period"]==1988,"1988-1994",
    np.where(names_subtable["period"]==1995,"1995-2001",
    np.where(names_subtable["period"]==2002,"2002-2008",
    np.where(names_subtable["period"]==2009,"2009-2015",
    np.where(names_subtable["period"]==2016,"2016-2022","")))))))

names_subtable
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>yr</th>
      <th>sex</th>
      <th>FirstForename</th>
      <th>number</th>
      <th>period</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>37</th>
      <td>1974</td>
      <td>B</td>
      <td>Alex</td>
      <td>4</td>
      <td>1974-1980</td>
    </tr>
    <tr>
      <th>345</th>
      <td>1974</td>
      <td>B</td>
      <td>Ellis</td>
      <td>1</td>
      <td>1974-1980</td>
    </tr>
    <tr>
      <th>515</th>
      <td>1974</td>
      <td>B</td>
      <td>Jamie</td>
      <td>88</td>
      <td>1974-1980</td>
    </tr>
    <tr>
      <th>659</th>
      <td>1974</td>
      <td>B</td>
      <td>Lee</td>
      <td>209</td>
      <td>1974-1980</td>
    </tr>
    <tr>
      <th>768</th>
      <td>1974</td>
      <td>B</td>
      <td>Morgan</td>
      <td>4</td>
      <td>1974-1980</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>260075</th>
      <td>2022</td>
      <td>G</td>
      <td>Ellis</td>
      <td>10</td>
      <td>2016-2022</td>
    </tr>
    <tr>
      <th>260122</th>
      <td>2022</td>
      <td>G</td>
      <td>Alex</td>
      <td>8</td>
      <td>2016-2022</td>
    </tr>
    <tr>
      <th>260251</th>
      <td>2022</td>
      <td>G</td>
      <td>Morgan</td>
      <td>6</td>
      <td>2016-2022</td>
    </tr>
    <tr>
      <th>260268</th>
      <td>2022</td>
      <td>G</td>
      <td>Taylor</td>
      <td>6</td>
      <td>2016-2022</td>
    </tr>
    <tr>
      <th>260309</th>
      <td>2022</td>
      <td>G</td>
      <td>Jamie</td>
      <td>5</td>
      <td>2016-2022</td>
    </tr>
  </tbody>
</table>
<p>856 rows × 5 columns</p>
</div>




```python
names_subtable= names_subtable.reset_index(drop=True)
names_subtable.sort_values(by=["FirstForename","period"],inplace=True)
names_subtable
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>yr</th>
      <th>sex</th>
      <th>FirstForename</th>
      <th>number</th>
      <th>period</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1974</td>
      <td>B</td>
      <td>Alex</td>
      <td>4</td>
      <td>1974-1980</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1974</td>
      <td>G</td>
      <td>Alex</td>
      <td>1</td>
      <td>1974-1980</td>
    </tr>
    <tr>
      <th>10</th>
      <td>1975</td>
      <td>B</td>
      <td>Alex</td>
      <td>9</td>
      <td>1974-1980</td>
    </tr>
    <tr>
      <th>20</th>
      <td>1976</td>
      <td>B</td>
      <td>Alex</td>
      <td>3</td>
      <td>1974-1980</td>
    </tr>
    <tr>
      <th>27</th>
      <td>1976</td>
      <td>G</td>
      <td>Alex</td>
      <td>1</td>
      <td>1974-1980</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>819</th>
      <td>2020</td>
      <td>G</td>
      <td>Taylor</td>
      <td>16</td>
      <td>2016-2022</td>
    </tr>
    <tr>
      <th>829</th>
      <td>2021</td>
      <td>B</td>
      <td>Taylor</td>
      <td>11</td>
      <td>2016-2022</td>
    </tr>
    <tr>
      <th>837</th>
      <td>2021</td>
      <td>G</td>
      <td>Taylor</td>
      <td>12</td>
      <td>2016-2022</td>
    </tr>
    <tr>
      <th>845</th>
      <td>2022</td>
      <td>B</td>
      <td>Taylor</td>
      <td>12</td>
      <td>2016-2022</td>
    </tr>
    <tr>
      <th>854</th>
      <td>2022</td>
      <td>G</td>
      <td>Taylor</td>
      <td>6</td>
      <td>2016-2022</td>
    </tr>
  </tbody>
</table>
<p>856 rows × 5 columns</p>
</div>




```python
popular_unisex_names = names_subtable.pivot_table(index=["FirstForename","period"],columns=["sex"],values="number",aggfunc="sum",fill_value=0)
popular_unisex_names["% B"]=popular_unisex_names["B"]/(popular_unisex_names["B"]+popular_unisex_names["G"])*100.0
popular_unisex_names["% G"]=popular_unisex_names["G"]/(popular_unisex_names["B"]+popular_unisex_names["G"])*100.0
popular_unisex_names["% diff"]=abs(popular_unisex_names["% B"]-popular_unisex_names["% G"])
popular_unisex_names.reset_index(inplace=True)
popular_unisex_names
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>sex</th>
      <th>FirstForename</th>
      <th>period</th>
      <th>B</th>
      <th>G</th>
      <th>% B</th>
      <th>% G</th>
      <th>% diff</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alex</td>
      <td>1974-1980</td>
      <td>31</td>
      <td>3</td>
      <td>91.176471</td>
      <td>8.823529</td>
      <td>82.352941</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alex</td>
      <td>1981-1987</td>
      <td>68</td>
      <td>2</td>
      <td>97.142857</td>
      <td>2.857143</td>
      <td>94.285714</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Alex</td>
      <td>1988-1994</td>
      <td>109</td>
      <td>64</td>
      <td>63.005780</td>
      <td>36.994220</td>
      <td>26.011561</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Alex</td>
      <td>1995-2001</td>
      <td>229</td>
      <td>218</td>
      <td>51.230425</td>
      <td>48.769575</td>
      <td>2.460850</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Alex</td>
      <td>2002-2008</td>
      <td>464</td>
      <td>371</td>
      <td>55.568862</td>
      <td>44.431138</td>
      <td>11.137725</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>64</th>
      <td>Taylor</td>
      <td>1988-1994</td>
      <td>145</td>
      <td>163</td>
      <td>47.077922</td>
      <td>52.922078</td>
      <td>5.844156</td>
    </tr>
    <tr>
      <th>65</th>
      <td>Taylor</td>
      <td>1995-2001</td>
      <td>536</td>
      <td>825</td>
      <td>39.382807</td>
      <td>60.617193</td>
      <td>21.234386</td>
    </tr>
    <tr>
      <th>66</th>
      <td>Taylor</td>
      <td>2002-2008</td>
      <td>710</td>
      <td>718</td>
      <td>49.719888</td>
      <td>50.280112</td>
      <td>0.560224</td>
    </tr>
    <tr>
      <th>67</th>
      <td>Taylor</td>
      <td>2009-2015</td>
      <td>398</td>
      <td>432</td>
      <td>47.951807</td>
      <td>52.048193</td>
      <td>4.096386</td>
    </tr>
    <tr>
      <th>68</th>
      <td>Taylor</td>
      <td>2016-2022</td>
      <td>118</td>
      <td>106</td>
      <td>52.678571</td>
      <td>47.321429</td>
      <td>5.357143</td>
    </tr>
  </tbody>
</table>
<p>69 rows × 7 columns</p>
</div>



Unisex names that are trending (2016-2022)


```python
recently_popular = popular_unisex_names[(popular_unisex_names["period"]=="2016-2022") & (popular_unisex_names["% diff"]<=25)]
recently_popular.sort_values(by="% diff")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>sex</th>
      <th>FirstForename</th>
      <th>period</th>
      <th>B</th>
      <th>G</th>
      <th>% B</th>
      <th>% G</th>
      <th>% diff</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>61</th>
      <td>Rowan</td>
      <td>2016-2022</td>
      <td>284</td>
      <td>286</td>
      <td>49.824561</td>
      <td>50.175439</td>
      <td>0.350877</td>
    </tr>
    <tr>
      <th>68</th>
      <td>Taylor</td>
      <td>2016-2022</td>
      <td>118</td>
      <td>106</td>
      <td>52.678571</td>
      <td>47.321429</td>
      <td>5.357143</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Harley</td>
      <td>2016-2022</td>
      <td>197</td>
      <td>319</td>
      <td>38.178295</td>
      <td>61.821705</td>
      <td>23.643411</td>
    </tr>
  </tbody>
</table>
</div>



Let's look at the name Taylor.


```python
Taylor=popular_unisex_names[popular_unisex_names["FirstForename"]=="Taylor"][["period","% B","% G"]]
Taylor.plot.bar(x="period",
                color={'% B': "#89CFF0", '% G': 'pink'},
                title="Proportion of boys/girls named Taylor over time")

```




    <Axes: title={'center': 'Proportion of boys/girls named Taylor over time'}, xlabel='period'>




    
![png](output_70_1.png)
    


In the two decades since 1974, almost all newborn babies named Taylor were boys. After that, the number of girls named Taylor began to increase sharply and once surpassed boys in 1988-2001. Now, it is heading back towards a gender balance.

<br>Let's look at the name Rowan.


```python
Rowan=names_subtable[names_subtable["FirstForename"]=="Rowan"].pivot_table("number",index="yr",columns="sex",aggfunc="sum").fillna(0)
Rowan.plot(color={'B': "#89CFF0", 'G': 'pink'},
          title="Number of boys/girls named Rowan over time",
          xlabel="year")
```




    <Axes: title={'center': 'Number of boys/girls named Rowan over time'}, xlabel='year'>




    
![png](output_73_1.png)
    


Rowan, another popular unisex name that is becoming increasingly popular with both sexes.

Let's look at the name Morgan.


```python
Morgan=popular_unisex_names[popular_unisex_names["FirstForename"]=="Morgan"][["period","% B","% G"]]

Morgan.plot.bar(x="period",
                 stacked=True,color={'% B': "#89CFF0", '% G': 'pink'},
                 title="Proportion of boys/girls named Morgan over time").legend(loc='center left',bbox_to_anchor=(1.0, 0.5))
```




    <matplotlib.legend.Legend at 0x2759994c100>




    
![png](output_76_1.png)
    


Like Taylor, Morgan began as primarily a boy's name. It is now primarily a girl's name.
