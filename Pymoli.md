

```python
# Import Dependencies
import pandas as pd
```


```python
# Import file as df
file = "Resources/purchase_data.csv"
df = pd.read_csv(file)
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
      <th>Purchase ID</th>
      <th>SN</th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Lisim78</td>
      <td>20</td>
      <td>Male</td>
      <td>108</td>
      <td>Extraction, Quickblade Of Trembling Hands</td>
      <td>3.53</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Lisovynya38</td>
      <td>40</td>
      <td>Male</td>
      <td>143</td>
      <td>Frenzied Scimitar</td>
      <td>1.56</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Ithergue48</td>
      <td>24</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>4.88</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Chamassasya86</td>
      <td>24</td>
      <td>Male</td>
      <td>100</td>
      <td>Blindscythe</td>
      <td>3.27</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Iskosia90</td>
      <td>23</td>
      <td>Male</td>
      <td>131</td>
      <td>Fury</td>
      <td>1.44</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Player Count
# Find total number of players
total_players = len(df['SN'].unique())
total_players
```




    576




```python
# Purchasing Analysis (Total)
# Find unique items
unique_items = len(df['Item ID'].unique())
# Find average purchase price
avg_price = '$' + str(round(df['Price'].mean(),2))
# Find count of purchases
purchases = df['Purchase ID'].count()
# Find total revenue
total_rev = '$' + str(round(df['Price'].sum(),2))
# Create summary DataFrame
total_analysis = {
    'Number of Unique Items' : [unique_items],
    'Average Price' : [avg_price],
    'Number of Purchases' : [purchases],
    'Total Revenue' : [total_rev],
}
total_summary = pd.DataFrame(total_analysis, columns = ['Number of Unique Items', 'Average Price', 'Number of Purchases', 'Total Revenue'])
total_summary
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
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>$3.05</td>
      <td>780</td>
      <td>$2379.77</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Gender Demographics
# Create temp df to prevent counting gender of user more than once
temp = df[['SN', 'Gender','Age']]
temp2 = temp.drop_duplicates()
gender_count = temp2['Gender'].value_counts()
gender_demo = pd.DataFrame({'Total Count': gender_count})
gender_demo['Percentage of Players'] = round(100*gender_count/total_players,2)
gender_demo
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
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>484</td>
      <td>84.03</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>81</td>
      <td>14.06</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>1.91</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchasing Analysis (Gender)
purchase_count = df.groupby(['Gender'], as_index=False)['SN'].count()
gender_analysis = pd.DataFrame({'Gender': purchase_count['Gender']})
gender_analysis['Purchase Count'] = purchase_count['SN']
gen_price = df.groupby(['Gender'], as_index=False).mean()['Price']
gender_analysis['Average Purchase Price'] = gen_price.map("${:.2f}".format)
gen_rev = df.groupby(['Gender'], as_index=False).sum()['Price']
gender_analysis['Total Purchase Value'] = gen_rev.map("${:.2f}".format)
gender_analysis['Normalized Totals'] = (gen_rev/gender_analysis['Purchase Count']).map("${:.2f}".format)
gender_analysis.set_index('Gender')
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
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>113</td>
      <td>$3.20</td>
      <td>$361.94</td>
      <td>$3.20</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>652</td>
      <td>$3.02</td>
      <td>$1967.64</td>
      <td>$3.02</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>15</td>
      <td>$3.35</td>
      <td>$50.19</td>
      <td>$3.35</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Establish bins for ages
age_bins = [0, 9.5, 14.5, 19.5, 24.5, 29.5, 34.5, 39.5, 99999]
group_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]
# Use temp df to avoid counting user more than once
temp2['Age Bin'] = pd.cut(temp2['Age'], age_bins, labels=group_names)
df['Age Bin'] = pd.cut(df['Age'], age_bins, labels=group_names)
age_demo = pd.DataFrame({'Total Count': temp2['Age Bin'].value_counts()})
age_demo['Percentage of Players'] = round(100*temp2['Age Bin'].value_counts()/total_players,2)
age_demo.reindex(group_names)
```

    C:\Users\ben16\Anaconda3\lib\site-packages\ipykernel_launcher.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      """
    




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
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>17</td>
      <td>2.95</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>22</td>
      <td>3.82</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>107</td>
      <td>18.58</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>258</td>
      <td>44.79</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>77</td>
      <td>13.37</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>52</td>
      <td>9.03</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>31</td>
      <td>5.38</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>12</td>
      <td>2.08</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchasing Analysis (Age Bin)
purchase_count = df.groupby(['Age Bin'], as_index=False)['SN'].count()
age_analysis = pd.DataFrame({'Age Bin': purchase_count['Age Bin']})
age_analysis['Purchase Count'] = purchase_count['SN']
age_price = df.groupby(['Age Bin'], as_index=False).mean()['Price']
age_analysis['Average Purchase Price'] = age_price.map("${:.2f}".format)
age_rev = df.groupby(['Age Bin'], as_index=False).sum()['Price']
age_analysis['Total Purchase Value'] = age_rev.map("${:.2f}".format)
age_analysis['Normalized Totals'] = (age_rev/age_analysis['Purchase Count']).map("${:.2f}".format)
age_analysis = age_analysis.set_index('Age Bin')
age_analysis.reindex(group_names)
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
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Age Bin</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>23</td>
      <td>$3.35</td>
      <td>$77.13</td>
      <td>$3.35</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>28</td>
      <td>$2.96</td>
      <td>$82.78</td>
      <td>$2.96</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>136</td>
      <td>$3.04</td>
      <td>$412.89</td>
      <td>$3.04</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>365</td>
      <td>$3.05</td>
      <td>$1114.06</td>
      <td>$3.05</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>101</td>
      <td>$2.90</td>
      <td>$293.00</td>
      <td>$2.90</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>73</td>
      <td>$2.93</td>
      <td>$214.00</td>
      <td>$2.93</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>41</td>
      <td>$3.60</td>
      <td>$147.67</td>
      <td>$3.60</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>13</td>
      <td>$2.94</td>
      <td>$38.24</td>
      <td>$2.94</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Top Spenders
purchase_count = df.groupby(['SN'], as_index=False)['Purchase ID'].count()
spender_analysis = pd.DataFrame({'SN': purchase_count['SN']})
spender_analysis['Purchase Count'] = purchase_count['Purchase ID']
spen_price = df.groupby(['SN'], as_index=False).mean()['Price']
spender_analysis['Average Purchase Price'] = spen_price.map("${:.2f}".format)
spen_rev = df.groupby(['SN'], as_index=False).sum()['Price']
spender_analysis['Total Purchase Value'] = spen_rev
spender_analysis = spender_analysis.set_index('SN').sort_values(by='Total Purchase Value',ascending=False)
spender_analysis['Total Purchase Value'] = spender_analysis['Total Purchase Value'].map("${:.2f}".format)
spender_analysis.head()
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
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
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
      <th>Lisosia93</th>
      <td>5</td>
      <td>$3.79</td>
      <td>$18.96</td>
    </tr>
    <tr>
      <th>Idastidru52</th>
      <td>4</td>
      <td>$3.86</td>
      <td>$15.45</td>
    </tr>
    <tr>
      <th>Chamjask73</th>
      <td>3</td>
      <td>$4.61</td>
      <td>$13.83</td>
    </tr>
    <tr>
      <th>Iral74</th>
      <td>4</td>
      <td>$3.40</td>
      <td>$13.62</td>
    </tr>
    <tr>
      <th>Iskadarya95</th>
      <td>3</td>
      <td>$4.37</td>
      <td>$13.10</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Most Popular Items
purchase_count = df.groupby(['Item ID', 'Item Name'], as_index=False)['Purchase ID'].count()
item_analysis = pd.DataFrame({'Item ID': purchase_count['Item ID']})
item_analysis['Item Name'] = purchase_count['Item Name']
item_analysis['Purchase Count'] = purchase_count['Purchase ID']
item_price = df.groupby(['Item ID', 'Item Name'], as_index=False).mean()['Price']
item_analysis['Average Purchase Price'] = item_price.map("${:.2f}".format)
item_rev = df.groupby(['Item ID', 'Item Name'], as_index=False).sum()['Price']
item_analysis['Total Purchase Value'] = item_rev
item_analysis = item_analysis.set_index('Item ID').sort_values(by='Purchase Count',ascending=False)
item_analysis['Total Purchase Value'] = item_analysis['Total Purchase Value'].map("${:.2f}".format)
item_analysis.head()
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
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>178</th>
      <td>Oathbreaker, Last Hope of the Breaking Storm</td>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>145</th>
      <td>Fiery Glass Crusader</td>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>108</th>
      <td>Extraction, Quickblade Of Trembling Hands</td>
      <td>9</td>
      <td>$3.53</td>
      <td>$31.77</td>
    </tr>
    <tr>
      <th>82</th>
      <td>Nirvana</td>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Pursuit, Cudgel of Necromancy</td>
      <td>8</td>
      <td>$1.02</td>
      <td>$8.16</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Most Profitable Items
purchase_count = df.groupby(['Item ID', 'Item Name'], as_index=False)['Purchase ID'].count()
item2_analysis = pd.DataFrame({'Item ID': purchase_count['Item ID']})
item2_analysis['Item Name'] = purchase_count['Item Name']
item2_analysis['Purchase Count'] = purchase_count['Purchase ID']
item2_price = df.groupby(['Item ID', 'Item Name'], as_index=False).mean()['Price']
item2_analysis['Average Purchase Price'] = item2_price.map("${:.2f}".format)
item2_rev = df.groupby(['Item ID', 'Item Name'], as_index=False).sum()['Price']
item2_analysis['Total Purchase Value'] = item2_rev
item2_analysis = item2_analysis.set_index('Item ID').sort_values(by='Total Purchase Value',ascending=False)
item2_analysis['Total Purchase Value'] = item2_analysis['Total Purchase Value'].map("${:.2f}".format)
item2_analysis.head()
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
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>178</th>
      <td>Oathbreaker, Last Hope of the Breaking Storm</td>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>82</th>
      <td>Nirvana</td>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>145</th>
      <td>Fiery Glass Crusader</td>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>92</th>
      <td>Final Critic</td>
      <td>8</td>
      <td>$4.88</td>
      <td>$39.04</td>
    </tr>
    <tr>
      <th>103</th>
      <td>Singed Scalpel</td>
      <td>8</td>
      <td>$4.35</td>
      <td>$34.80</td>
    </tr>
  </tbody>
</table>
</div>


