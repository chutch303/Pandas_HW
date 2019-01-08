

```python
import pandas as pd
```

```python
import numpy as np
```

```python
df = pd.read_json('purchase_data.json')
df.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>




```python
# number or unique items
print('number or unique items = ' + str(len(df["Item ID"].unique())))
```

    number or unique items = 183
    


```python
#Average Purchase Price
print('Averager purchase price = ' + str(df['Price'].mean()))
```

    Averager purchase price = 2.931192307692303
    


```python
#Total Number of Purchases
print("Total Number of Purchases = " + str(df['Price'].count()))
```

    Total Number of Purchases = 780
    


```python
#Total Revenue
print('Total Revenue = $' + str(df['Price'].sum()))
```

    Total Revenue = $2286.33
    


```python
#total count of gender
df["Gender"].value_counts()
```




    Male                     633
    Female                   136
    Other / Non-Disclosed     11
    Name: Gender, dtype: int64




```python
#Percentage breakdown by gender
(df["Gender"].value_counts())/(df["Gender"].count())
#How to get total and percentage side by side??????????????
```




    Male                     0.811538
    Female                   0.174359
    Other / Non-Disclosed    0.014103
    Name: Gender, dtype: float64




```python
#average purchase price by gender
grouped_mean = df.groupby('Gender').mean().reset_index()
grouped_mean
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
      <th>Gender</th>
      <th>Age</th>
      <th>Item ID</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Female</td>
      <td>22.558824</td>
      <td>88.110294</td>
      <td>2.815515</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>22.685624</td>
      <td>91.571880</td>
      <td>2.950521</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other / Non-Disclosed</td>
      <td>27.363636</td>
      <td>114.636364</td>
      <td>3.249091</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchase count by gender
grouped_count = df.groupby('Gender').count()
grouped_count['Price']
```




    Gender
    Female                   136
    Male                     633
    Other / Non-Disclosed     11
    Name: Price, dtype: int64




```python
#Total Purchase Value
grouped_sum = df.groupby('Gender').Price.sum()
grouped_sum
#do no reset index because grouped_sum and grouped_sn_gender need to be on the same index
```




    Gender
    Female                    382.91
    Male                     1867.68
    Other / Non-Disclosed      35.74
    Name: Price, dtype: float64




```python
#Normalized Totals by Gender? want total revenue by gender divided by # of unique SN by gender
#unique SN by Gender
grouped_sn_gender = df.groupby(['SN', 'Gender']).count().reset_index()['Gender'].value_counts()
grouped_sn_gender
```




    Male                     465
    Female                   100
    Other / Non-Disclosed      8
    Name: Gender, dtype: int64




```python
#how to code total revenue by gender divided by # of unique SN by gender??????????
grouped_sn_gender/grouped_sum
```




    Female                   0.261158
    Male                     0.248972
    Other / Non-Disclosed    0.223839
    dtype: float64




```python
age_min = df['Age'].min()
age_max = df['Age'].max()
print(age_min, age_max)

bins = [0, 4, 8, 12, 16, 20, 24, 28, 32, 36, 40, 44, 48]
group_names = ['0-4', '4-8', '8-12', '12-16', '16-20', '20-24', '24-28', '28-32', '32-36', '36-40', '40-44', '44-48']
pd.cut(df['Age'], bins, labels=group_names)
df['age_summary'] = pd.cut(df['Age'], bins, labels=group_names)
df.head()
```

    7 45
    




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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
      <th>age_summary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
      <td>36-40</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
      <td>32-36</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
      <td>20-24</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchase count by age
print(df['age_summary'].value_counts())
```

    20-24    238
    16-20    161
    24-28    104
    12-16     87
    28-32     66
    32-36     38
    36-40     37
    8-12      24
    4-8       22
    40-44      2
    44-48      1
    0-4        0
    Name: age_summary, dtype: int64
    


```python
# Average Purchase Price by age group in Price column
grouped_age_avg_price = df.groupby('age_summary').mean().reset_index()
print(grouped_age_avg_price)
```

       age_summary        Age     Item ID     Price
    0          0-4        NaN         NaN       NaN
    1          4-8   7.136364   85.909091  2.788182
    2         8-12  10.541667   87.125000  3.385417
    3        12-16  14.942529   92.816092  2.745862
    4        16-20  19.248447   84.248447  2.907019
    5        20-24  22.647059   89.605042  2.924748
    6        24-28  25.634615   93.855769  2.974712
    7        28-32  30.257576   91.166667  3.061970
    8        32-36  34.394737  105.631579  2.981053
    9        36-40  38.648649  112.972973  2.901351
    10       40-44  42.500000   83.500000  2.960000
    11       44-48  45.000000  124.000000  2.720000
    


```python
# Total Purchase Value by age group
grouped_age_total_purch = df.groupby('age_summary').Price.sum()
print(grouped_age_total_purch)
```

    age_summary
    0-4        0.00
    4-8       61.34
    8-12      81.25
    12-16    238.89
    16-20    468.03
    20-24    696.09
    24-28    309.37
    28-32    202.09
    32-36    113.28
    36-40    107.35
    40-44      5.92
    44-48      2.72
    Name: Price, dtype: float64
    


```python
#Normalized Purchase totals by age bins??????????
grouped_sn_age = df['age_summary'].value_counts()
grouped_sn_age
```




    20-24    238
    16-20    161
    24-28    104
    12-16     87
    28-32     66
    32-36     38
    36-40     37
    8-12      24
    4-8       22
    40-44      2
    44-48      1
    0-4        0
    Name: age_summary, dtype: int64




```python
# Avg. Purchase by age bin
(grouped_age_total_purch/grouped_sn_age).sort_values(ascending=False)
```




    8-12     3.385417
    28-32    3.061970
    32-36    2.981053
    24-28    2.974712
    40-44    2.960000
    20-24    2.924748
    16-20    2.907019
    36-40    2.901351
    4-8      2.788182
    12-16    2.745862
    44-48    2.720000
    0-4           NaN
    dtype: float64




```python
# Top Spenders

# Identify the the top 5 spenders in the game by total purchase value, then list (in a table):

# SN
# Purchase Count
# Average Purchase Price
# Total Purchase Value
```


```python
# Identify the the top 5 spenders in the game by total purchase value
grouped_sn_price = df.groupby(['SN']).sum().sort_values('Price', ascending=False)
grouped_sn_price[:5]
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
      <th>Age</th>
      <th>Item ID</th>
      <th>Price</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>145</td>
      <td>472</td>
      <td>17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>100</td>
      <td>233</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>156</td>
      <td>609</td>
      <td>12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>63</td>
      <td>353</td>
      <td>12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>66</td>
      <td>284</td>
      <td>11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
top_5_buyers = ['Undirrala66', 'Saedue76', 'Mindimnya67', 'Haellysu29', 'Eoda93']
```


```python
mask_spend = df['SN'].isin(top_5_buyers)
```


```python
spend_df = df[mask_spend]
spend_df.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
      <th>age_summary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>15</th>
      <td>21</td>
      <td>Male</td>
      <td>96</td>
      <td>Blood-Forged Skeletal Spine</td>
      <td>4.77</td>
      <td>Haellysu29</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>79</th>
      <td>29</td>
      <td>Male</td>
      <td>144</td>
      <td>Blood Infused Guardian</td>
      <td>2.86</td>
      <td>Undirrala66</td>
      <td>28-32</td>
    </tr>
    <tr>
      <th>107</th>
      <td>29</td>
      <td>Male</td>
      <td>115</td>
      <td>Spectral Diamond Doomblade</td>
      <td>4.25</td>
      <td>Undirrala66</td>
      <td>28-32</td>
    </tr>
    <tr>
      <th>108</th>
      <td>22</td>
      <td>Male</td>
      <td>35</td>
      <td>Heartless Bone Dualblade</td>
      <td>2.63</td>
      <td>Eoda93</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>131</th>
      <td>29</td>
      <td>Male</td>
      <td>62</td>
      <td>Piece Maker</td>
      <td>4.36</td>
      <td>Undirrala66</td>
      <td>28-32</td>
    </tr>
  </tbody>
</table>
</div>




```python
#.agg is looking for functions such as count, mean, sum
top_5_buyers_analysis = spend_df.groupby('SN').Price.agg(['count', 'mean', 'sum'])
top_5_buyers_analysis.columns = ['Purchase Count', 'Avg. Purchase Price', 'Total Purchase Value']
print(top_5_buyers_analysis)
```

                 Purchase Count  Avg. Purchase Price  Total Purchase Value
    SN                                                                    
    Eoda93                    3             3.860000                 11.58
    Haellysu29                3             4.243333                 12.73
    Mindimnya67               4             3.185000                 12.74
    Saedue76                  4             3.390000                 13.56
    Undirrala66               5             3.412000                 17.06
    


```python
# Most Popular Items

# Identify the 5 most popular items by purchase count, then list (in a table):

# Item ID
# Item Name
# Purchase Count
# Item Price
# Total Purchase Value
```


```python
df['Item ID'].value_counts()[:5]
```




    84     11
    39     11
    31      9
    34      9
    175     9
    Name: Item ID, dtype: int64




```python
top_5_items = ['84', '39', '31', '34', '175']
```


```python
mask_item_id = df['Item ID'].isin(top_5_items)
item_id_df = df[mask_item_id]
item_id_df.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
      <th>age_summary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>56</th>
      <td>23</td>
      <td>Male</td>
      <td>31</td>
      <td>Trickster</td>
      <td>2.07</td>
      <td>Marilsasya33</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>57</th>
      <td>24</td>
      <td>Male</td>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>4.14</td>
      <td>Alallo58</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>61</th>
      <td>24</td>
      <td>Male</td>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>2.35</td>
      <td>Rinallorap73</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>62</th>
      <td>19</td>
      <td>Female</td>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>2.35</td>
      <td>Aeri84</td>
      <td>16-20</td>
    </tr>
    <tr>
      <th>75</th>
      <td>31</td>
      <td>Male</td>
      <td>31</td>
      <td>Trickster</td>
      <td>2.07</td>
      <td>Sondastan54</td>
      <td>28-32</td>
    </tr>
  </tbody>
</table>
</div>




```python
top_5_items_analysis = item_id_df.groupby('Item ID').Price.agg(['count', 'mean', 'sum'])
top_5_items_analysis.columns = ['Purchase Count', 'Purchase Price', 'Total Purchase Value']
print(top_5_items_analysis)
```

             Purchase Count  Purchase Price  Total Purchase Value
    Item ID                                                      
    31                    9            2.07                 18.63
    34                    9            4.14                 37.26
    39                   11            2.35                 25.85
    84                   11            2.23                 24.53
    175                   9            1.24                 11.16
    

Most Profitable Items

Identify the 5 most profitable items by total purchase value, then list (in a table):

Item ID
Item Name
Purchase Count
Item Price
Total Purchase Value


```python
most_profit_items = (df
 .groupby('Item Name')
 .Price
 .sum()
 .sort_values(ascending=False)
 .head()
 .index
)
#[:5] shows the top 5
#.tail() to show last 5
#.head() 
```


```python
most_profit_items
```




    Index(['Final Critic', 'Retribution Axe', 'Stormcaller',
           'Spectral Diamond Doomblade', 'Orenmir'],
          dtype='object', name='Item Name')




```python
#iloc is looking for indexes
#loc is looking for value
#methods are functions for an object
```


```python
profit_df = df[df['Item Name'].isin(most_profit_items)]
profit_df
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
      <th>age_summary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>50</th>
      <td>32</td>
      <td>Female</td>
      <td>32</td>
      <td>Orenmir</td>
      <td>4.95</td>
      <td>Saistyphos30</td>
      <td>28-32</td>
    </tr>
    <tr>
      <th>54</th>
      <td>25</td>
      <td>Female</td>
      <td>101</td>
      <td>Final Critic</td>
      <td>4.62</td>
      <td>Minduli80</td>
      <td>24-28</td>
    </tr>
    <tr>
      <th>57</th>
      <td>24</td>
      <td>Male</td>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>4.14</td>
      <td>Alallo58</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>83</th>
      <td>25</td>
      <td>Female</td>
      <td>32</td>
      <td>Orenmir</td>
      <td>4.95</td>
      <td>Frichaststa61</td>
      <td>24-28</td>
    </tr>
    <tr>
      <th>101</th>
      <td>25</td>
      <td>Male</td>
      <td>30</td>
      <td>Stormcaller</td>
      <td>4.15</td>
      <td>Assistasda90</td>
      <td>24-28</td>
    </tr>
    <tr>
      <th>107</th>
      <td>29</td>
      <td>Male</td>
      <td>115</td>
      <td>Spectral Diamond Doomblade</td>
      <td>4.25</td>
      <td>Undirrala66</td>
      <td>28-32</td>
    </tr>
    <tr>
      <th>119</th>
      <td>19</td>
      <td>Male</td>
      <td>180</td>
      <td>Stormcaller</td>
      <td>2.78</td>
      <td>Yasur35</td>
      <td>16-20</td>
    </tr>
    <tr>
      <th>126</th>
      <td>25</td>
      <td>Male</td>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>4.14</td>
      <td>Raesty92</td>
      <td>24-28</td>
    </tr>
    <tr>
      <th>175</th>
      <td>35</td>
      <td>Male</td>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>4.14</td>
      <td>Raillydeu47</td>
      <td>32-36</td>
    </tr>
    <tr>
      <th>193</th>
      <td>19</td>
      <td>Male</td>
      <td>101</td>
      <td>Final Critic</td>
      <td>4.62</td>
      <td>Farusrian86</td>
      <td>16-20</td>
    </tr>
    <tr>
      <th>199</th>
      <td>30</td>
      <td>Female</td>
      <td>180</td>
      <td>Stormcaller</td>
      <td>2.78</td>
      <td>Phadai31</td>
      <td>28-32</td>
    </tr>
    <tr>
      <th>226</th>
      <td>25</td>
      <td>Female</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Chamistast30</td>
      <td>24-28</td>
    </tr>
    <tr>
      <th>227</th>
      <td>20</td>
      <td>Male</td>
      <td>32</td>
      <td>Orenmir</td>
      <td>4.95</td>
      <td>Qiluard68</td>
      <td>16-20</td>
    </tr>
    <tr>
      <th>250</th>
      <td>25</td>
      <td>Male</td>
      <td>115</td>
      <td>Spectral Diamond Doomblade</td>
      <td>4.25</td>
      <td>Ilogha82</td>
      <td>24-28</td>
    </tr>
    <tr>
      <th>251</th>
      <td>19</td>
      <td>Female</td>
      <td>115</td>
      <td>Spectral Diamond Doomblade</td>
      <td>4.25</td>
      <td>Aeri84</td>
      <td>16-20</td>
    </tr>
    <tr>
      <th>307</th>
      <td>26</td>
      <td>Male</td>
      <td>180</td>
      <td>Stormcaller</td>
      <td>2.78</td>
      <td>Aidain51</td>
      <td>24-28</td>
    </tr>
    <tr>
      <th>311</th>
      <td>18</td>
      <td>Male</td>
      <td>115</td>
      <td>Spectral Diamond Doomblade</td>
      <td>4.25</td>
      <td>Raerithsti62</td>
      <td>16-20</td>
    </tr>
    <tr>
      <th>333</th>
      <td>22</td>
      <td>Male</td>
      <td>30</td>
      <td>Stormcaller</td>
      <td>4.15</td>
      <td>Aeliru63</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>359</th>
      <td>20</td>
      <td>Male</td>
      <td>32</td>
      <td>Orenmir</td>
      <td>4.95</td>
      <td>Aeliriam77</td>
      <td>16-20</td>
    </tr>
    <tr>
      <th>380</th>
      <td>22</td>
      <td>Male</td>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>4.14</td>
      <td>Alaephos75</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>388</th>
      <td>15</td>
      <td>Male</td>
      <td>32</td>
      <td>Orenmir</td>
      <td>4.95</td>
      <td>Palurrian69</td>
      <td>12-16</td>
    </tr>
    <tr>
      <th>389</th>
      <td>37</td>
      <td>Male</td>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>4.14</td>
      <td>Iskistasda86</td>
      <td>36-40</td>
    </tr>
    <tr>
      <th>413</th>
      <td>24</td>
      <td>Male</td>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>4.14</td>
      <td>Sondadar26</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>415</th>
      <td>25</td>
      <td>Female</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Chamistast30</td>
      <td>24-28</td>
    </tr>
    <tr>
      <th>431</th>
      <td>37</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Aduephos78</td>
      <td>36-40</td>
    </tr>
    <tr>
      <th>433</th>
      <td>23</td>
      <td>Male</td>
      <td>180</td>
      <td>Stormcaller</td>
      <td>2.78</td>
      <td>Ithesuphos68</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>442</th>
      <td>7</td>
      <td>Male</td>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>4.14</td>
      <td>Liri91</td>
      <td>4-8</td>
    </tr>
    <tr>
      <th>448</th>
      <td>25</td>
      <td>Male</td>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>4.14</td>
      <td>Sausosia74</td>
      <td>24-28</td>
    </tr>
    <tr>
      <th>458</th>
      <td>25</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Jiskassa76</td>
      <td>24-28</td>
    </tr>
    <tr>
      <th>506</th>
      <td>29</td>
      <td>Male</td>
      <td>180</td>
      <td>Stormcaller</td>
      <td>2.78</td>
      <td>Tyalaesu89</td>
      <td>28-32</td>
    </tr>
    <tr>
      <th>509</th>
      <td>28</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Iskista88</td>
      <td>24-28</td>
    </tr>
    <tr>
      <th>535</th>
      <td>23</td>
      <td>Female</td>
      <td>30</td>
      <td>Stormcaller</td>
      <td>4.15</td>
      <td>Undirra73</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>557</th>
      <td>24</td>
      <td>Male</td>
      <td>30</td>
      <td>Stormcaller</td>
      <td>4.15</td>
      <td>Iskimnya76</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>572</th>
      <td>24</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Lisossa63</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>593</th>
      <td>22</td>
      <td>Other / Non-Disclosed</td>
      <td>115</td>
      <td>Spectral Diamond Doomblade</td>
      <td>4.25</td>
      <td>Euna48</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>595</th>
      <td>20</td>
      <td>Male</td>
      <td>30</td>
      <td>Stormcaller</td>
      <td>4.15</td>
      <td>Eusur90</td>
      <td>16-20</td>
    </tr>
    <tr>
      <th>606</th>
      <td>20</td>
      <td>Male</td>
      <td>101</td>
      <td>Final Critic</td>
      <td>4.62</td>
      <td>Lirtossa84</td>
      <td>16-20</td>
    </tr>
    <tr>
      <th>612</th>
      <td>18</td>
      <td>Female</td>
      <td>101</td>
      <td>Final Critic</td>
      <td>4.62</td>
      <td>Isurria36</td>
      <td>16-20</td>
    </tr>
    <tr>
      <th>642</th>
      <td>16</td>
      <td>Male</td>
      <td>115</td>
      <td>Spectral Diamond Doomblade</td>
      <td>4.25</td>
      <td>Reula64</td>
      <td>12-16</td>
    </tr>
    <tr>
      <th>648</th>
      <td>23</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Rithe53</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>657</th>
      <td>28</td>
      <td>Male</td>
      <td>32</td>
      <td>Orenmir</td>
      <td>4.95</td>
      <td>Tyarithn67</td>
      <td>24-28</td>
    </tr>
    <tr>
      <th>663</th>
      <td>38</td>
      <td>Male</td>
      <td>101</td>
      <td>Final Critic</td>
      <td>4.62</td>
      <td>Tyisriphos58</td>
      <td>36-40</td>
    </tr>
    <tr>
      <th>705</th>
      <td>25</td>
      <td>Male</td>
      <td>115</td>
      <td>Spectral Diamond Doomblade</td>
      <td>4.25</td>
      <td>Aeral85</td>
      <td>24-28</td>
    </tr>
    <tr>
      <th>737</th>
      <td>22</td>
      <td>Male</td>
      <td>101</td>
      <td>Final Critic</td>
      <td>4.62</td>
      <td>Sondim68</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>746</th>
      <td>35</td>
      <td>Male</td>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>4.14</td>
      <td>Ralasti48</td>
      <td>32-36</td>
    </tr>
  </tbody>
</table>
</div>




```python
profit_analysis = profit_df.groupby('Item Name').Price.agg(['count', 'mean', 'sum'])
profit_analysis.columns = ['Purchase Count', 'Purchase Price', 'Total Purchase Value']
profit_analysis
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
      <th>Purchase Count</th>
      <th>Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Final Critic</th>
      <td>14</td>
      <td>2.757143</td>
      <td>38.60</td>
    </tr>
    <tr>
      <th>Orenmir</th>
      <td>6</td>
      <td>4.950000</td>
      <td>29.70</td>
    </tr>
    <tr>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>4.140000</td>
      <td>37.26</td>
    </tr>
    <tr>
      <th>Spectral Diamond Doomblade</th>
      <td>7</td>
      <td>4.250000</td>
      <td>29.75</td>
    </tr>
    <tr>
      <th>Stormcaller</th>
      <td>10</td>
      <td>3.465000</td>
      <td>34.65</td>
    </tr>
  </tbody>
</table>
</div>


