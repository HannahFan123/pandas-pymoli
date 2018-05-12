

```python
import pandas as pd

# The path to our CSV file
file = "purchase_data.json"

# Read data into pandas
raw_df = pd.read_json(file)
raw_df.head()
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
#Total Players Count

print("Total Players:" + str(num_players))
```

    Total Players:573
    


```python
#**Purchasing Analysis (Total)**
#* Number of Unique Items
#* Average Purchase Price
#* Total Number of Purchases
#* Total Revenue

number_unique_items = len(raw_df["Item Name"].unique())
num_purchases = raw_df["Item Name"].count()
total_rev = raw_df["Price"].sum()
purchase_price = total_rev / num_purchases 

summary_table = pd.DataFrame({"Number of Unique Items": [number_unique_items],
                              "Average Price": [purchase_price], 
                              "Number of Purchases": [num_purchases],
                              "Total_Revenue": [total_rev],
                             })
summary_table.head()
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
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Number of Unique Items</th>
      <th>Total_Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2.931192</td>
      <td>780</td>
      <td>179</td>
      <td>2286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
#**Gender Demographics**
#* Percentage and Count of Male Players
#* Percentage and Count of Female Players
#* Percentage and Count of Other / Non-Disclosed

gender_count = []
gender_percent = []
genders = raw_df["Gender"].unique()
num_players = len(raw_df["SN"].unique())

for gender in genders:
   new_gender_df = raw_df[raw_df["Gender"]== gender]
   gender_count.append(len(new_gender_df["SN"].unique()))
   gender_percent.append(round(len(new_gender_df["SN"].unique()) / num_players * 100,2))

gender_demo_df = pd.DataFrame({"Genders": genders, 
                               "Percentage of Players": gender_percent, 
                               "Total Count": gender_count})
gender_demo_df
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
      <th>Genders</th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Male</td>
      <td>81.15</td>
      <td>465</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Female</td>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other / Non-Disclosed</td>
      <td>1.40</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
#**Purchasing Analysis (Gender)** 
#* The below each broken by gender
#  * Purchase Count
#  * Average Purchase Price
#  * Total Purchase Value
#  * Normalized Totals

gender_groupby = raw_df.groupby(["Gender"])
purchases = gender_groupby["Price"].count()
total_purchase_value = gender_groupby["Price"].sum()
avg_purchase_price = total_purchase_value / purchases
normalized = total_purchase_value / purchases

gender_summary_table = pd.DataFrame({"Purchase Count": purchases, 
                                     "Average Purchase Price": avg_purchase_price,
                                     "Total Purchase Value": total_purchase_value,
                                     "Normalized Total": normalized})
gender_summary_table
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
      <th>Average Purchase Price</th>
      <th>Normalized Total</th>
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
      <td>2.815515</td>
      <td>2.815515</td>
      <td>136</td>
      <td>382.91</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>2.950521</td>
      <td>2.950521</td>
      <td>633</td>
      <td>1867.68</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>3.249091</td>
      <td>3.249091</td>
      <td>11</td>
      <td>35.74</td>
    </tr>
  </tbody>
</table>
</div>




```python
#**Age Demographics**
#* The below each broken into bins of 4 years (i.e. <10, 10-14, 15-19, etc.) 
#  * Percentage of Players
#  * Total Count

# Create the bins in which Data will be held
bins = [0, 9, 14, 19, 24, 29, 34, 39, 120]

# Create the names for the four bins
group_names = ['<10', '10-14', '15-19', '19-24', '25-29', '30-34', '35-39', '40+']

age_demo_series = pd.cut(raw_df["Age"], bins, labels=group_names)


raw_df["Age Summary"] = age_demo_series

unique_df = raw_df.drop_duplicates("SN")

age_groups = unique_df.groupby("Age Summary")
total_count_age = unique_df.groupby("Age Summary").count()
all_players = len(raw_df["SN"].unique())
percentage_players = (total_count_age / all_players)*100
print(all_players)

new_data_df = pd.DataFrame({"Total Count": total_count_age["SN"],
                           "Percentage of Players": percentage_players["SN"]
                           })
new_data_df
```

    573
    




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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Age Summary</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.315881</td>
      <td>19</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>4.013962</td>
      <td>23</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>17.452007</td>
      <td>100</td>
    </tr>
    <tr>
      <th>19-24</th>
      <td>45.200698</td>
      <td>259</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>15.183246</td>
      <td>87</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>8.202443</td>
      <td>47</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>4.712042</td>
      <td>27</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1.919721</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
#**Top Spenders**
#* Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
#  * SN
#  * Purchase Count
#  * Average Purchase Price
#  * Total Purchase Value

sn_groupby = raw_df.groupby(["SN"])
purchases = sn_groupby["Price"].count()
total_p_value = sn_groupby["Price"].sum()
avg_p_price = total_p_value / purchases

top_spenders_summary_table = pd.DataFrame({#"SN": sn_groupby,
                                           "Purchase Count": purchases, 
                                           "Average Purchase Price": avg_p_price,
                                           "Total Purchase Value": total_p_value})

top_spenders_df = top_spenders_summary_table.sort_values("Total Purchase Value", ascending=False)

top_spenders_df.head()
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
      <td>3.412000</td>
      <td>5</td>
      <td>17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>3.390000</td>
      <td>4</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>3.185000</td>
      <td>4</td>
      <td>12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>4.243333</td>
      <td>3</td>
      <td>12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3.860000</td>
      <td>3</td>
      <td>11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
#**Most Popular Items**
#* Identify the 5 most popular items by purchase count, then list (in a table):
#  * Item ID
#  * Item Name
#  * Purchase Count
#  * Item Price
#  * Total Purchase Value

item_groupby = raw_df.groupby(["Item ID", "Item Name"])
purchases1 = item_groupby["Price"].count()
total_p_value1 = item_groupby["Price"].sum()
avg_p_price1 = total_p_value1 / purchases1

top_items_summary = pd.DataFrame({"Purchase Count": purchases1, 
                                  "Average Purchase Price": avg_p_price1,
                                  "Total Purchase Value": total_p_value1})

top_items_df = top_items_summary.sort_values("Purchase Count", ascending=False)

top_items_df.head()
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
      <th></th>
      <th>Average Purchase Price</th>
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
      <td>2.35</td>
      <td>11</td>
      <td>25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>2.23</td>
      <td>11</td>
      <td>24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>2.07</td>
      <td>9</td>
      <td>18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>1.24</td>
      <td>9</td>
      <td>11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>1.49</td>
      <td>9</td>
      <td>13.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
#**Most Profitable Items**
#* Identify the 5 most profitable items by total purchase value, then list (in a table):
#  * Item ID
#  * Item Name
#  * Item Price
#  * Purchase Count
#  * Total Purchase Value

profitable_groupby = raw_df.groupby(["Item ID", "Item Name", "Price"])
purchases2 = profitable_groupby["Price"].count()
total_p_value2 = profitable_groupby["Price"].sum()
avg_p_price2 = total_p_value2 / purchases2

profitable_summary_table = pd.DataFrame({"Purchase Count": purchases2, 
                                         "Total Purchase Value": total_p_value2})

top_profit_df = profitable_summary_table.sort_values("Total Purchase Value", ascending=False)

top_profit_df.head()
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
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <th>4.14</th>
      <td>9</td>
      <td>37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <th>4.25</th>
      <td>7</td>
      <td>29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <th>4.95</th>
      <td>6</td>
      <td>29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <th>4.87</th>
      <td>6</td>
      <td>29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <th>3.61</th>
      <td>8</td>
      <td>28.88</td>
    </tr>
  </tbody>
</table>
</div>


