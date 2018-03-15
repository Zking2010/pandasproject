

```python
#Dependencies
import pandas as pd
from sklearn import preprocessing
```


```python
#load json
file = "purchase_data.json"
```


```python
#read json
df = pd.read_json(file)
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
#total players
total_players=df["SN"].nunique()
pd.DataFrame({"Total Players":[total_players]})
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
#Purchasing analysis
uniqueitems=df["Item Name"].nunique()
avgprice=df["Price"].mean()
purchasecount=df["Price"].count()
revenue=df["Price"].sum()

Purchasing_Analysis=pd.DataFrame({"Unique Items":[uniqueitems],
                                 "Average Price":[avgprice],
                                 "Purchase Count":[purchasecount],
                                 "Total Revenue":[revenue]})
Purchasing_Analysis["Average Price"]=Purchasing_Analysis["Average Price"].map("${:.2f}".format)
Purchasing_Analysis.head()
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
      <th>Purchase Count</th>
      <th>Total Revenue</th>
      <th>Unique Items</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$2.93</td>
      <td>780</td>
      <td>2286.33</td>
      <td>179</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Gender Demographics
#______________________
#dropping duplicate SNs to get true player count
df1=df.drop_duplicates(["SN"])
#isolating genders
maledf=df1.loc[df1["Gender"]=="Male"]
femaledf=df1.loc[df1["Gender"]=="Female"]
otherdf=df1.loc[df1["Gender"]=="Other / Non-Disclosed"]
#gender counts
malecount=len(maledf)
femalecount=len(femaledf)
othercount=len(otherdf)
#calculating percentages
malepercent=malecount/len(df1)*100
femalepercent=femalecount/len(df1)*100
otherpercent=othercount/len(df1)*100

genderdf=pd.DataFrame({"Gender":["Male","Female","Other/Non-Disclosed"],
                      "Count":[malecount,femalecount,othercount],
                      "Percentages":[malepercent,femalepercent,otherpercent]}, columns=["Gender","Count","Percentages"])

genderdf["Percentages"]=genderdf["Percentages"].map("{:.0f}%".format)
genderdf

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
      <th>Count</th>
      <th>Percentages</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Male</td>
      <td>465</td>
      <td>81%</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Female</td>
      <td>100</td>
      <td>17%</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other/Non-Disclosed</td>
      <td>8</td>
      <td>1%</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Gender Purchasing
maledf1=df.loc[df["Gender"]=="Male"]
femaledf1=df.loc[df["Gender"]=="Female"]
otherdf1=df.loc[df["Gender"]=="Other / Non-Disclosed"]
#purchase counts
malepurch=len(maledf1)
femalepurch=len(femaledf1)
otherpurch=len(otherdf1)
#average purchase amounts
maleavg=maledf1["Price"].mean()
femaleavg=femaledf1["Price"].mean()
otheravg=otherdf1["Price"].mean()
#purchase totals
maletotal=maledf1["Price"].sum()
femaletotal=femaledf1["Price"].sum()
othertotal=otherdf1["Price"].sum()
genderpuchasedf=pd.DataFrame({"Gender":["Male","Female","Other/Non-Disclosed"],
                             "Purchase Amounts":[malepurch,femalepurch,otherpurch],
                             "Average Purchase":[maleavg,femaleavg,otheravg],
                             "Purchase Totals":[maletotal,femaletotal,othertotal]}, 
                             columns=["Gender","Purchase Totals","Purchase Amounts","Average Purchase"])
genderpuchasedf.set_index("Gender")
#normalized totals
x=genderpuchasedf[["Purchase Totals"]].values
min_max_scaler = preprocessing.MinMaxScaler()
normalized= min_max_scaler.fit_transform(x)
normalizedgenderdf=pd.DataFrame({"Gender":["Male","Female","Other/Non-Disclosed"],
                             "Purchase Amounts":[malepurch,femalepurch,otherpurch],
                             "Average Purchase":[maleavg,femaleavg,otheravg],
                             "Purchase Totals":[maletotal,femaletotal,othertotal],
                     "Normalized Value":[normalized[0],normalized[1],normalized[2]]}, 
                     columns=["Gender","Purchase Totals","Purchase Amounts","Average Purchase","Normalized Value"])
normalizedgenderdf["Average Purchase"]=genderpuchasedf["Average Purchase"].map("${:.2f}".format)
normalizedgenderdf["Purchase Totals"]=genderpuchasedf["Purchase Totals"].map("${:.2f}".format)

normalizedgenderdf
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
      <th>Purchase Totals</th>
      <th>Purchase Amounts</th>
      <th>Average Purchase</th>
      <th>Normalized Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Male</td>
      <td>$1867.68</td>
      <td>633</td>
      <td>$2.95</td>
      <td>[1.0]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Female</td>
      <td>$382.91</td>
      <td>136</td>
      <td>$2.82</td>
      <td>[0.18950948175158572]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other/Non-Disclosed</td>
      <td>$35.74</td>
      <td>11</td>
      <td>$3.25</td>
      <td>[0.0]</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Age Analysis
#________________
#Binning Ages
bins = [0,10,14,19,24,29,34,40,45]
groups=[">10","10-14","15-19","20-24","25-29","30-34","35-39",">40"]
df["Age Summary"]=pd.cut(df["Age"],bins,labels=groups)
#Assigning Bins
group1=df.loc[df["Age Summary"]==">10"]
group2=df.loc[df["Age Summary"]=="10-14"]
group3=df.loc[df["Age Summary"]=="15-19"]
group4=df.loc[df["Age Summary"]=="20-24"]
group5=df.loc[df["Age Summary"]=="25-29"]
group6=df.loc[df["Age Summary"]=="30-34"]
group7=df.loc[df["Age Summary"]=="35-39"]
group8=df.loc[df["Age Summary"]==">40"]
#Purchase Count
group1count=len(group1)
group2count=len(group2)
group3count=len(group3)
group4count=len(group4)
group5count=len(group5)
group6count=len(group6)
group7count=len(group7)
group8count=len(group8)
#Average Purchase
group1avg=group1["Price"].mean()
group2avg=group2["Price"].mean()
group3avg=group3["Price"].mean()
group4avg=group4["Price"].mean()
group5avg=group5["Price"].mean()
group6avg=group6["Price"].mean()
group7avg=group7["Price"].mean()
group8avg=group8["Price"].mean()
#Total Purchase
group1total=group1["Price"].sum()
group2total=group2["Price"].sum()
group3total=group3["Price"].sum()
group4total=group4["Price"].sum()
group5total=group5["Price"].sum()
group6total=group6["Price"].sum()
group7total=group7["Price"].sum()
group8total=group8["Price"].sum()

agepurchasedf=pd.DataFrame({"Age":[">10","10-14","15-19","20-24","25-29","30-34","35-39",">40"],
                            "Purchase Count":[group1count,group2count,group3count,group4count,group5count,group6count,group7count,group8count],
                           "Average Purchase":[group1avg,group2avg,group3avg,group4avg,group5avg,group6avg,group7avg,group8avg],
                           "Total Purchase":[group1total,group2total,group3total,group4total,group5total,group6total,group7total,group8total]}, 
                          columns=["Age","Purchase Count","Average Purchase","Total Purchase"])
#Normalized Values
x=agepurchasedf[["Total Purchase"]].values
min_max_scaler = preprocessing.MinMaxScaler()
normalized= min_max_scaler.fit_transform(x)
normalizedagedf=pd.DataFrame({"Age":[">10","10-14","15-19","20-24","25-29","30-34","35-39",">40"],
                            "Purchase Count":[group1count,group2count,group3count,group4count,group5count,group6count,group7count,group8count],
                           "Average Purchase":[group1avg,group2avg,group3avg,group4avg,group5avg,group6avg,group7avg,group8avg],
                           "Total Purchase":[group1total,group2total,group3total,group4total,group5total,group6total,group7total,group8total],
                     "Normalized Value":[normalized[0],normalized[1],normalized[2],normalized[3],normalized[4],normalized[5],normalized[6],normalized[7]]}, 
                     columns=["Age","Purchase Count","Average Purchase","Total Purchase","Normalized Value"])
normalizedagedf["Average Purchase"]=agepurchasedf["Average Purchase"].map("${:.2f}".format)
normalizedagedf["Total Purchase"]=agepurchasedf["Total Purchase"].map("${:.2f}".format)

normalizedagedf
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
      <th>Purchase Count</th>
      <th>Average Purchase</th>
      <th>Total Purchase</th>
      <th>Normalized Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>&gt;10</td>
      <td>32</td>
      <td>$3.02</td>
      <td>$96.62</td>
      <td>[0.09068887674847702]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10-14</td>
      <td>31</td>
      <td>$2.70</td>
      <td>$83.79</td>
      <td>[0.0774638450516941]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>15-19</td>
      <td>133</td>
      <td>$2.91</td>
      <td>$386.42</td>
      <td>[0.38941172832506976]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20-24</td>
      <td>336</td>
      <td>$2.91</td>
      <td>$978.77</td>
      <td>[1.0]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>25-29</td>
      <td>125</td>
      <td>$2.96</td>
      <td>$370.33</td>
      <td>[0.37282632224547224]</td>
    </tr>
    <tr>
      <th>5</th>
      <td>30-34</td>
      <td>64</td>
      <td>$3.08</td>
      <td>$197.25</td>
      <td>[0.19441724304990055]</td>
    </tr>
    <tr>
      <th>6</th>
      <td>35-39</td>
      <td>56</td>
      <td>$2.94</td>
      <td>$164.51</td>
      <td>[0.16066918866543656]</td>
    </tr>
    <tr>
      <th>7</th>
      <td>&gt;40</td>
      <td>3</td>
      <td>$2.88</td>
      <td>$8.64</td>
      <td>[0.0]</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Top Spender
spenders=df.groupby("SN")
spendercount=spenders["SN"].count()
spenderavg=spenders["Price"].mean()
spendervalue=spenders["Price"].sum()

topdf=pd.DataFrame({"Purchase Count":spendercount,
                   "Average Purchase":spenderavg,
                   "Total Value": spendervalue},
                  columns=["Purchase Count","Average Purchase","Total Value"])
topspender=topdf.sort_values("Total Value",ascending=False)
topspender["Average Purchase"] = topspender["Average Purchase"].map("${:.2f}".format)

topspender.head()
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
      <th>Average Purchase</th>
      <th>Total Value</th>
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
      <td>5</td>
      <td>$3.41</td>
      <td>17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>$3.39</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>$3.18</td>
      <td>12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>$4.24</td>
      <td>12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>$3.86</td>
      <td>11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Popular item
item=df.groupby(["Item ID","Item Name","Price"])
itemcount=item["Price"].count()
itemvalue=item["Price"].sum()
topdf2=pd.DataFrame({"Purchase Count":itemcount,
                    "Total Value":itemvalue},)
topitem=topdf2.sort_values("Purchase Count",ascending=False)
topitem.head()

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
      <th>Total Value</th>
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
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <th>2.35</th>
      <td>11</td>
      <td>25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <th>2.23</th>
      <td>11</td>
      <td>24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <th>2.07</th>
      <td>9</td>
      <td>18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <th>1.24</th>
      <td>9</td>
      <td>11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <th>1.49</th>
      <td>9</td>
      <td>13.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Profitable
topitem=topdf2.sort_values("Total Value",ascending=False)
topitem.head()

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
      <th>Total Value</th>
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


