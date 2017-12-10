

```python
import pandas as pd
```


```python
url = 'purchase_data.json'
purchase_df = pd.read_json(url, orient = 'columns')
purchase_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
# Calculating total_players
total_players =purchase_df['SN'].nunique()
total_players_df = pd.DataFrame({'Total Players' : [total_players]})
total_players_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis (Total)
unique_items = purchase_df['Item ID'].nunique()
average_price = purchase_df['Price'].sum()/purchase_df['Price'].count()
average_price = "$"+ format(average_price, '.2f')
total_purchases = purchase_df['Price'].count()
total_revenue = purchase_df['Price'].sum()
total_revenue = "$" + format(total_revenue, '.2f')
purchase_analysis_df = pd.DataFrame({'Number of Unique Items' : [unique_items],
                                'Average Purchase Price' : average_price,
                                 'Total Number of Purchases' : total_purchases,
                                 'Total Revenue' : total_revenue
                                })
purchase_analysis_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Price</th>
      <th>Number of Unique Items</th>
      <th>Total Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$2.93</td>
      <td>183</td>
      <td>780</td>
      <td>$2286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Gender Demographics
gander_values = purchase_df['Gender'].value_counts()
gender_df = pd.DataFrame({'Count': gander_values,
                         'Total Percent' : gander_values/gander_values.sum()})
gender_df['Total Percent'] = pd.Series(["{0:.2f}".format(val * 100) for val in gender_df['Total Percent']], index = gender_df.index)
gender_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Count</th>
      <th>Total Percent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>81.15</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>17.44</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>1.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchasing Analysis (Gender)
purchasing_group = purchase_df.groupby('Gender')
unique_persons = purchasing_group['SN'].nunique()
price_counts = purchasing_group['Price'].count()
total_purchase_price = purchasing_group['Price'].sum()
purchasing_df = pd.DataFrame({'Purchase Count': price_counts,
                         'Average Purchase Price' : total_purchase_price/price_counts ,
                          'Total Purchase Value': total_purchase_price,
                           'Normalized Totals' : total_purchase_price/unique_persons
                             })
purchasing_df['Average Purchase Price'] = pd.Series(["$"+"{0:.2f}".format(val) for val in purchasing_df['Average Purchase Price']], index = purchasing_df.index)
purchasing_df['Total Purchase Value'] = pd.Series(["$"+"{0:.2f}".format(val) for val in purchasing_df['Total Purchase Value']], index = purchasing_df.index)
purchasing_df['Normalized Totals'] = pd.Series(["$"+"{0:.2f}".format(val) for val in purchasing_df['Normalized Totals']], index = purchasing_df.index)


purchasing_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Price</th>
      <th>Normalized Totals</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
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
      <td>$2.82</td>
      <td>$3.83</td>
      <td>136</td>
      <td>$382.91</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>$2.95</td>
      <td>$4.02</td>
      <td>633</td>
      <td>$1867.68</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>$3.25</td>
      <td>$4.47</td>
      <td>11</td>
      <td>$35.74</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Age Demographics
max_age = purchase_df['Age'].max()
age_bins = [0, 10, 14, 19, 24, 29, 34, 39, 44, 49 ]
group_names = ["Less than 10", "10 to 14", '15 to 19', "20 to 24", "25 to 29", "30 to 34", "35 to 39", "40 to 44", "Greater than 44"]
categories = pd.cut(purchase_df["Age"], age_bins, labels = group_names)

age_demo_df = purchase_df
age_demo_df['Age Categories'] = categories
age_demo_group = age_demo_df.groupby('Age Categories')
unique_persons = age_demo_group['SN'].nunique()
purchase_count = age_demo_group['Price'].count()
total_purchase_price = age_demo_group['Price'].sum()
age_demographics_df = pd.DataFrame({'Purchase Count': purchase_count,
                         'Average Purchase Price' : total_purchase_price/purchase_count ,
                          'Total Purchase Value': total_purchase_price,
                           'Normalized Totals' : total_purchase_price/unique_persons
                             })
age_demographics_df['Average Purchase Price'] = pd.Series(["$"+"{0:.2f}".format(val) for val in age_demographics_df['Average Purchase Price']], index = age_demographics_df.index)
age_demographics_df['Total Purchase Value'] = pd.Series(["$"+"{0:.2f}".format(val) for val in age_demographics_df['Total Purchase Value']], index = age_demographics_df.index)
age_demographics_df['Normalized Totals'] = pd.Series(["$"+"{0:.2f}".format(val) for val in age_demographics_df['Normalized Totals']], index = age_demographics_df.index)


age_demographics_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Price</th>
      <th>Normalized Totals</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Age Categories</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Less than 10</th>
      <td>$3.02</td>
      <td>$4.39</td>
      <td>32</td>
      <td>$96.62</td>
    </tr>
    <tr>
      <th>10 to 14</th>
      <td>$2.70</td>
      <td>$4.19</td>
      <td>31</td>
      <td>$83.79</td>
    </tr>
    <tr>
      <th>15 to 19</th>
      <td>$2.91</td>
      <td>$3.86</td>
      <td>133</td>
      <td>$386.42</td>
    </tr>
    <tr>
      <th>20 to 24</th>
      <td>$2.91</td>
      <td>$3.78</td>
      <td>336</td>
      <td>$978.77</td>
    </tr>
    <tr>
      <th>25 to 29</th>
      <td>$2.96</td>
      <td>$4.26</td>
      <td>125</td>
      <td>$370.33</td>
    </tr>
    <tr>
      <th>30 to 34</th>
      <td>$3.08</td>
      <td>$4.20</td>
      <td>64</td>
      <td>$197.25</td>
    </tr>
    <tr>
      <th>35 to 39</th>
      <td>$2.84</td>
      <td>$4.42</td>
      <td>42</td>
      <td>$119.40</td>
    </tr>
    <tr>
      <th>40 to 44</th>
      <td>$3.19</td>
      <td>$5.10</td>
      <td>16</td>
      <td>$51.03</td>
    </tr>
    <tr>
      <th>Greater than 44</th>
      <td>$2.72</td>
      <td>$2.72</td>
      <td>1</td>
      <td>$2.72</td>
    </tr>
  </tbody>
</table>
</div>




```python


```


```python
# Top Spenders
spenders_group = purchase_df.groupby('SN')
purchase_price = spenders_group['Price'].sum()
purchase_count = spenders_group['Price'].count()
purchase_avg = spenders_group['Price'].mean()
top_spenders_df = pd.DataFrame({'Total Purchase Value': purchase_price,
                                'Purchase Count' : purchase_count,
                                'Average Purchase Price' : purchase_avg
                               })
final_top_spenders_df = top_spenders_df.sort_values(by='Total Purchase Value', ascending=False).head()
final_top_spenders_df['Average Purchase Price'] = pd.Series(["$"+"{0:.2f}".format(val) for val in final_top_spenders_df['Average Purchase Price']], index = final_top_spenders_df.index)
final_top_spenders_df['Total Purchase Value'] = pd.Series(["$"+"{0:.2f}".format(val) for val in final_top_spenders_df['Total Purchase Value']], index = final_top_spenders_df.index)

final_top_spenders_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Price</th>
      <th>Purchase Count</th>
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
      <th>Undirrala66</th>
      <td>$3.41</td>
      <td>5</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>$3.39</td>
      <td>4</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>$3.18</td>
      <td>4</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>$4.24</td>
      <td>3</td>
      <td>$12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>$3.86</td>
      <td>3</td>
      <td>$11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Most Popular Items
popular_items_group = purchase_df.groupby(['Item ID', 'Item Name'])
item_count= popular_items_group['Price'].count()
total_purchase_value = popular_items_group['Price'].sum()

popular_items_df = pd.DataFrame({
                                'Total Purchase Value': total_purchase_value,
                                'Purchase Count' : item_count,
                                'Item Price' : total_purchase_value/item_count
                               })

final_popular_items_df = popular_items_df.sort_values(by='Purchase Count', ascending=False).head()
final_popular_items_df['Item Price'] = pd.Series(["$"+"{0:.2f}".format(val) for val in final_popular_items_df['Item Price']], index = final_popular_items_df.index)
final_popular_items_df['Total Purchase Value'] = pd.Series(["$"+"{0:.2f}".format(val) for val in final_popular_items_df['Total Purchase Value']], index = final_popular_items_df.index)


final_popular_items_df

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Item Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>$2.35</td>
      <td>11</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>$2.23</td>
      <td>11</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>$2.07</td>
      <td>9</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>$1.24</td>
      <td>9</td>
      <td>$11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>$1.49</td>
      <td>9</td>
      <td>$13.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Most Profitable Items
final_profitable_items_df = popular_items_df.sort_values(by='Total Purchase Value', ascending=False).head()

final_profitable_items_df['Item Price'] = pd.Series(["$"+"{0:.2f}".format(val) for val in final_profitable_items_df['Item Price']], index = final_profitable_items_df.index)
final_profitable_items_df['Total Purchase Value'] = pd.Series(["$"+"{0:.2f}".format(val) for val in final_profitable_items_df['Total Purchase Value']], index = final_profitable_items_df.index)

final_profitable_items_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Item Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>$4.14</td>
      <td>9</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>$4.25</td>
      <td>7</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>$4.95</td>
      <td>6</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>$4.87</td>
      <td>6</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>$3.61</td>
      <td>8</td>
      <td>$28.88</td>
    </tr>
  </tbody>
</table>
</div>






Observable Trends:

1. People in the Age Group 15 -30 years have shown more interest in purchasing the optional items of the play.
2. Men have shown more interest in purchasing the optional items of the play when compared to Women.
3. Although Betrayal, Arcane Gem are most frquently bought items, more profitable items are Retribution Axe and Spectral Diamond Doomblade.

