
# Heroes of Pymoli Data Analysis

- The vast majority of players (77.84%) are between the ages of 15 and 29. Specifically, the 20-24 range (45.20%) is the largest by far, followed by 15-19 (17.45%) and 25-29 (15.18%).

- Looking at Average Purchase Price by age does not tell us much, as there is not much variance across the age ranges. However, there is a clear dropoff in Normalized Totals among the 15-19 and 20-24 ranges. This indicates that players aged 15-24 are making fewer purchases overall. Considering these are the two largest age ranges, rectifying this discrepancy would boost revenue significantly.

- Females make up just 17.45% of players, and have smaller Normalized Totals than male players. Researching what females prefer and dislike about the game (and relaying that into focused advertisements) should be a top priority.


```python
# Dependencies
import pandas as pd
import numpy as np
```


```python
# Read in the JSON
df = pd.read_json("Resources/purchase_data.json")
```


```python
# Inspect the columns
df.columns
```




    Index(['Age', 'Gender', 'Item ID', 'Item Name', 'Price', 'SN'], dtype='object')



### Player Count


```python
# Player Count
PlayerCount = len(df["SN"].value_counts())
Players = pd.DataFrame(
        {"Total Players": [PlayerCount]})
Players
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



### Purchasing Total (Analysis)


```python
# Purchasing Analysis (Total)
ItemCount = len(df["Item Name"].value_counts())
AvgPurchase = df["Price"].mean()
PurchaseCount = len(df["Item Name"])
TotalRevenue = df["Price"].sum()

TotalAnalysis = pd.DataFrame(
        {"Number of Unique Items": [ItemCount],
         "Average Price": [AvgPurchase],
         "Number of Purchases": [PurchaseCount],
         "Total Revenue": [TotalRevenue]})

TotalAnalysis = TotalAnalysis.style.format({"Average Price": "${:.2f}",
                                            "Total Revenue": "${:.2f}"})
TotalAnalysis
```




<style  type="text/css" >
</style>  
<table id="T_58bd1c18_8da0_11e7_8d19_dca9047bd1fc" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Price</th> 
        <th class="col_heading level0 col1" >Number of Purchases</th> 
        <th class="col_heading level0 col2" >Number of Unique Items</th> 
        <th class="col_heading level0 col3" >Total Revenue</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_58bd1c18_8da0_11e7_8d19_dca9047bd1fc" class="row_heading level0 row0" >0</th> 
        <td id="T_58bd1c18_8da0_11e7_8d19_dca9047bd1fcrow0_col0" class="data row0 col0" >$2.93</td> 
        <td id="T_58bd1c18_8da0_11e7_8d19_dca9047bd1fcrow0_col1" class="data row0 col1" >780</td> 
        <td id="T_58bd1c18_8da0_11e7_8d19_dca9047bd1fcrow0_col2" class="data row0 col2" >179</td> 
        <td id="T_58bd1c18_8da0_11e7_8d19_dca9047bd1fcrow0_col3" class="data row0 col3" >$2286.33</td> 
    </tr></tbody> 
</table> 



### Gender Demographics


```python
# Gender Demographics
GenderUnique = df.groupby("Gender").nunique()["SN"]

MaleCount = GenderUnique["Male"]
FemaleCount = GenderUnique["Female"]
OtherCount = PlayerCount - MaleCount - FemaleCount

MalePercent = (MaleCount / PlayerCount) * 100
FemalePercent = (FemaleCount / PlayerCount) * 100
OtherPercent = (OtherCount / PlayerCount) * 100

GenderDemographics = pd.DataFrame(
        {"Gender": ["Male", "Female", "Other"],
         "Total Count": [MaleCount, FemaleCount, OtherCount],
         "Percentage of Players": [MalePercent, FemalePercent, OtherPercent]}).set_index("Gender")

GenderDemographics = GenderDemographics.style.format({"Percentage of Players": "{:.2f}%"})
GenderDemographics
```




<style  type="text/css" >
</style>  
<table id="T_58c1137a_8da0_11e7_b129_dca9047bd1fc" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Percentage of Players</th> 
        <th class="col_heading level0 col1" >Total Count</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Gender</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_58c1137a_8da0_11e7_b129_dca9047bd1fc" class="row_heading level0 row0" >Male</th> 
        <td id="T_58c1137a_8da0_11e7_b129_dca9047bd1fcrow0_col0" class="data row0 col0" >81.15%</td> 
        <td id="T_58c1137a_8da0_11e7_b129_dca9047bd1fcrow0_col1" class="data row0 col1" >465</td> 
    </tr>    <tr> 
        <th id="T_58c1137a_8da0_11e7_b129_dca9047bd1fc" class="row_heading level0 row1" >Female</th> 
        <td id="T_58c1137a_8da0_11e7_b129_dca9047bd1fcrow1_col0" class="data row1 col0" >17.45%</td> 
        <td id="T_58c1137a_8da0_11e7_b129_dca9047bd1fcrow1_col1" class="data row1 col1" >100</td> 
    </tr>    <tr> 
        <th id="T_58c1137a_8da0_11e7_b129_dca9047bd1fc" class="row_heading level0 row2" >Other</th> 
        <td id="T_58c1137a_8da0_11e7_b129_dca9047bd1fcrow2_col0" class="data row2 col0" >1.40%</td> 
        <td id="T_58c1137a_8da0_11e7_b129_dca9047bd1fcrow2_col1" class="data row2 col1" >8</td> 
    </tr></tbody> 
</table> 



### Purchasing Analysis (Gender)


```python
# Purchasing Analysis (Gender)
GenderGroup = df.groupby("Gender")

GenderCount = GenderGroup["Age"].count()
GenderSum = GenderGroup["Price"].sum()

MalePurchases = GenderCount["Male"]
FemalePurchases = GenderCount["Female"]
OtherPurchases = PurchaseCount - MalePurchases - FemalePurchases

MaleTotal = GenderSum["Male"]
FemaleTotal = GenderSum["Female"]
OtherTotal = TotalRevenue - MaleTotal - FemaleTotal

MaleAvg = MaleTotal / MalePurchases
FemaleAvg = FemaleTotal / FemalePurchases
OtherAvg = OtherTotal / OtherPurchases

MaleNorm = MaleTotal / MaleCount
FemaleNorm = FemaleTotal / FemaleCount
OtherNorm = OtherTotal / OtherCount

GenderAnalysis = pd.DataFrame(
        {"Gender": ["Male", "Female", "Other"],
         "Purchase Count": [MalePurchases, FemalePurchases, OtherPurchases],
         "Average Purchase Price": [MaleAvg, FemaleAvg, OtherAvg],
         "Total Purchase Value": [MaleTotal, FemaleTotal, OtherTotal],
         "Normalized Totals": [MaleNorm, FemaleNorm, OtherNorm]}).set_index("Gender")

GenderAnalysis = GenderAnalysis.style.format({"Average Purchase Price": "${:.2f}",
                                              "Normalized Totals": "${:.2f}",
                                              "Total Purchase Value": "${:.2f}"})
GenderAnalysis
```




<style  type="text/css" >
</style>  
<table id="T_58c46840_8da0_11e7_a328_dca9047bd1fc" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Purchase Price</th> 
        <th class="col_heading level0 col1" >Normalized Totals</th> 
        <th class="col_heading level0 col2" >Purchase Count</th> 
        <th class="col_heading level0 col3" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Gender</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_58c46840_8da0_11e7_a328_dca9047bd1fc" class="row_heading level0 row0" >Male</th> 
        <td id="T_58c46840_8da0_11e7_a328_dca9047bd1fcrow0_col0" class="data row0 col0" >$2.95</td> 
        <td id="T_58c46840_8da0_11e7_a328_dca9047bd1fcrow0_col1" class="data row0 col1" >$4.02</td> 
        <td id="T_58c46840_8da0_11e7_a328_dca9047bd1fcrow0_col2" class="data row0 col2" >633</td> 
        <td id="T_58c46840_8da0_11e7_a328_dca9047bd1fcrow0_col3" class="data row0 col3" >$1867.68</td> 
    </tr>    <tr> 
        <th id="T_58c46840_8da0_11e7_a328_dca9047bd1fc" class="row_heading level0 row1" >Female</th> 
        <td id="T_58c46840_8da0_11e7_a328_dca9047bd1fcrow1_col0" class="data row1 col0" >$2.82</td> 
        <td id="T_58c46840_8da0_11e7_a328_dca9047bd1fcrow1_col1" class="data row1 col1" >$3.83</td> 
        <td id="T_58c46840_8da0_11e7_a328_dca9047bd1fcrow1_col2" class="data row1 col2" >136</td> 
        <td id="T_58c46840_8da0_11e7_a328_dca9047bd1fcrow1_col3" class="data row1 col3" >$382.91</td> 
    </tr>    <tr> 
        <th id="T_58c46840_8da0_11e7_a328_dca9047bd1fc" class="row_heading level0 row2" >Other</th> 
        <td id="T_58c46840_8da0_11e7_a328_dca9047bd1fcrow2_col0" class="data row2 col0" >$3.25</td> 
        <td id="T_58c46840_8da0_11e7_a328_dca9047bd1fcrow2_col1" class="data row2 col1" >$4.47</td> 
        <td id="T_58c46840_8da0_11e7_a328_dca9047bd1fcrow2_col2" class="data row2 col2" >11</td> 
        <td id="T_58c46840_8da0_11e7_a328_dca9047bd1fcrow2_col3" class="data row2 col3" >$35.74</td> 
    </tr></tbody> 
</table> 



### Age Demographics


```python
# Age Demographics
def Bucket(x):
    if x["Age"] < 10:
        return "<10"
    if x["Age"] < 15:
        return "10-14"
    if x["Age"] < 20:
        return "15-19"
    if x["Age"] < 25:
        return "20-24"
    if x["Age"] < 30:
        return "25-29"
    if x["Age"] < 35:
        return "30-34"
    if x["Age"] < 40:
        return "35-39"
    else:
        return "40+"
    
df["Age Bucket"] = df.apply(Bucket, axis=1)

AgeUnique = df.groupby("Age Bucket").nunique()["SN"]

FiveCount = AgeUnique["<10"]
TenCount = AgeUnique["10-14"]
FifteenCount = AgeUnique["15-19"]
TwentyCount = AgeUnique["20-24"]
TwentyfiveCount = AgeUnique["25-29"]
ThirtyCount = AgeUnique["30-34"]
ThirtyfiveCount = AgeUnique["35-39"]
FortyCount = AgeUnique["40+"]

FivePercent = (FiveCount / PlayerCount) * 100
TenPercent = (TenCount / PlayerCount) * 100
FifteenPercent = (FifteenCount / PlayerCount) * 100
TwentyPercent = (TwentyCount / PlayerCount) * 100
TwentyfivePercent = (TwentyfiveCount / PlayerCount) * 100
ThirtyPercent = (ThirtyCount / PlayerCount) * 100
ThirtyfivePercent = (ThirtyfiveCount / PlayerCount) * 100
FortyPercent = (FortyCount / PlayerCount) * 100

AgeDemographics = pd.DataFrame(
        {"Age Range": ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"],
         "Total Count": [FiveCount, TenCount, FifteenCount, TwentyCount,
                            TwentyfiveCount, ThirtyCount, ThirtyfiveCount, FortyCount],
         "Percentage of Players": [FivePercent, TenPercent, FifteenPercent, TwentyPercent, TwentyfivePercent,
                                   ThirtyPercent, ThirtyfivePercent, FortyPercent]}).set_index("Age Range")

AgeDemographics = AgeDemographics.style.format({"Percentage of Players": "{:.2f}%"})
AgeDemographics
```




<style  type="text/css" >
</style>  
<table id="T_57625dac_8da2_11e7_97ae_dca9047bd1fc" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Percentage of Players</th> 
        <th class="col_heading level0 col1" >Total Count</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Age Range</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_57625dac_8da2_11e7_97ae_dca9047bd1fc" class="row_heading level0 row0" ><10</th> 
        <td id="T_57625dac_8da2_11e7_97ae_dca9047bd1fcrow0_col0" class="data row0 col0" >3.32%</td> 
        <td id="T_57625dac_8da2_11e7_97ae_dca9047bd1fcrow0_col1" class="data row0 col1" >19</td> 
    </tr>    <tr> 
        <th id="T_57625dac_8da2_11e7_97ae_dca9047bd1fc" class="row_heading level0 row1" >10-14</th> 
        <td id="T_57625dac_8da2_11e7_97ae_dca9047bd1fcrow1_col0" class="data row1 col0" >4.01%</td> 
        <td id="T_57625dac_8da2_11e7_97ae_dca9047bd1fcrow1_col1" class="data row1 col1" >23</td> 
    </tr>    <tr> 
        <th id="T_57625dac_8da2_11e7_97ae_dca9047bd1fc" class="row_heading level0 row2" >15-19</th> 
        <td id="T_57625dac_8da2_11e7_97ae_dca9047bd1fcrow2_col0" class="data row2 col0" >17.45%</td> 
        <td id="T_57625dac_8da2_11e7_97ae_dca9047bd1fcrow2_col1" class="data row2 col1" >100</td> 
    </tr>    <tr> 
        <th id="T_57625dac_8da2_11e7_97ae_dca9047bd1fc" class="row_heading level0 row3" >20-24</th> 
        <td id="T_57625dac_8da2_11e7_97ae_dca9047bd1fcrow3_col0" class="data row3 col0" >45.20%</td> 
        <td id="T_57625dac_8da2_11e7_97ae_dca9047bd1fcrow3_col1" class="data row3 col1" >259</td> 
    </tr>    <tr> 
        <th id="T_57625dac_8da2_11e7_97ae_dca9047bd1fc" class="row_heading level0 row4" >25-29</th> 
        <td id="T_57625dac_8da2_11e7_97ae_dca9047bd1fcrow4_col0" class="data row4 col0" >15.18%</td> 
        <td id="T_57625dac_8da2_11e7_97ae_dca9047bd1fcrow4_col1" class="data row4 col1" >87</td> 
    </tr>    <tr> 
        <th id="T_57625dac_8da2_11e7_97ae_dca9047bd1fc" class="row_heading level0 row5" >30-34</th> 
        <td id="T_57625dac_8da2_11e7_97ae_dca9047bd1fcrow5_col0" class="data row5 col0" >8.20%</td> 
        <td id="T_57625dac_8da2_11e7_97ae_dca9047bd1fcrow5_col1" class="data row5 col1" >47</td> 
    </tr>    <tr> 
        <th id="T_57625dac_8da2_11e7_97ae_dca9047bd1fc" class="row_heading level0 row6" >35-39</th> 
        <td id="T_57625dac_8da2_11e7_97ae_dca9047bd1fcrow6_col0" class="data row6 col0" >4.71%</td> 
        <td id="T_57625dac_8da2_11e7_97ae_dca9047bd1fcrow6_col1" class="data row6 col1" >27</td> 
    </tr>    <tr> 
        <th id="T_57625dac_8da2_11e7_97ae_dca9047bd1fc" class="row_heading level0 row7" >40+</th> 
        <td id="T_57625dac_8da2_11e7_97ae_dca9047bd1fcrow7_col0" class="data row7 col0" >1.92%</td> 
        <td id="T_57625dac_8da2_11e7_97ae_dca9047bd1fcrow7_col1" class="data row7 col1" >11</td> 
    </tr></tbody> 
</table> 



### Purchasing Analysis (Age)


```python
# Purchasing Analysis (Age)

AgeGroup = df.groupby("Age Bucket")

AgeCount = AgeGroup["Age"].count()
AgeSum = AgeGroup["Price"].sum()

FivePurchases = AgeCount["<10"]
TenPurchases = AgeCount["10-14"]
FifteenPurchases = AgeCount["15-19"]
TwentyPurchases = AgeCount["20-24"]
TwentyfivePurchases = AgeCount["25-29"]
ThirtyPurchases = AgeCount["30-34"]
ThirtyfivePurchases = AgeCount["35-39"]
FortyPurchases = AgeCount["40+"]

FiveTotal = AgeSum["<10"]
TenTotal = AgeSum["10-14"]
FifteenTotal = AgeSum["15-19"]
TwentyTotal = AgeSum["20-24"]
TwentyfiveTotal = AgeSum["25-29"]
ThirtyTotal = AgeSum["30-34"]
ThirtyfiveTotal = AgeSum["35-39"]
FortyTotal = AgeSum["40+"]

FiveAvg = FiveTotal / FivePurchases
TenAvg = TenTotal / TenPurchases
FifteenAvg = FifteenTotal / FifteenPurchases
TwentyAvg = TwentyTotal / TwentyPurchases
TwentyfiveAvg = TwentyfiveTotal / TwentyfivePurchases
ThirtyAvg = ThirtyTotal / ThirtyPurchases
ThirtyfiveAvg = ThirtyfiveTotal / ThirtyfivePurchases
FortyAvg = FortyTotal / FortyPurchases

FiveNorm = FiveTotal / FiveCount
TenNorm = TenTotal / TenCount
FifteenNorm = FifteenTotal / FifteenCount
TwentyNorm = TwentyTotal / TwentyCount
TwentyfiveNorm = TwentyfiveTotal / TwentyfiveCount
ThirtyNorm = ThirtyTotal / ThirtyCount
ThirtyfiveNorm = ThirtyfiveTotal / ThirtyfiveCount
FortyNorm = FortyTotal / FortyCount

AgeAnalysis = pd.DataFrame(
        {"Age Range": ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"],
         "Purchase Count": [FivePurchases, TenPurchases, FifteenPurchases, TwentyPurchases,
                            TwentyfivePurchases, ThirtyPurchases, ThirtyfivePurchases, FortyPurchases],
         "Average Purchase Price": [FiveAvg, TenAvg, FifteenAvg, TwentyAvg,
                            TwentyfiveAvg, ThirtyAvg, ThirtyfiveAvg, FortyAvg],
         "Total Purchase Value": [FiveTotal, TenTotal, FifteenTotal, TwentyTotal,
                            TwentyfiveTotal, ThirtyTotal, ThirtyfiveTotal, FortyTotal],
         "Normalized Totals": [FiveNorm, TenNorm, FifteenNorm, TwentyNorm, TwentyfiveNorm,
                               ThirtyNorm, ThirtyfiveNorm, FortyNorm]}).set_index("Age Range")

AgeAnalysis = AgeAnalysis.style.format({"Average Purchase Price": "${:.2f}",
                                        "Normalized Totals": "${:.2f}",
                                        "Total Purchase Value": "${:.2f}"})
AgeAnalysis
```




<style  type="text/css" >
</style>  
<table id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fc" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Purchase Price</th> 
        <th class="col_heading level0 col1" >Normalized Totals</th> 
        <th class="col_heading level0 col2" >Purchase Count</th> 
        <th class="col_heading level0 col3" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Age Range</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fc" class="row_heading level0 row0" ><10</th> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow0_col0" class="data row0 col0" >$2.98</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow0_col1" class="data row0 col1" >$4.39</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow0_col2" class="data row0 col2" >28</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow0_col3" class="data row0 col3" >$83.46</td> 
    </tr>    <tr> 
        <th id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fc" class="row_heading level0 row1" >10-14</th> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow1_col0" class="data row1 col0" >$2.77</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow1_col1" class="data row1 col1" >$4.22</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow1_col2" class="data row1 col2" >35</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow1_col3" class="data row1 col3" >$96.95</td> 
    </tr>    <tr> 
        <th id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fc" class="row_heading level0 row2" >15-19</th> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow2_col0" class="data row2 col0" >$2.91</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow2_col1" class="data row2 col1" >$3.86</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow2_col2" class="data row2 col2" >133</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow2_col3" class="data row2 col3" >$386.42</td> 
    </tr>    <tr> 
        <th id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fc" class="row_heading level0 row3" >20-24</th> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow3_col0" class="data row3 col0" >$2.91</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow3_col1" class="data row3 col1" >$3.78</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow3_col2" class="data row3 col2" >336</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow3_col3" class="data row3 col3" >$978.77</td> 
    </tr>    <tr> 
        <th id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fc" class="row_heading level0 row4" >25-29</th> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow4_col0" class="data row4 col0" >$2.96</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow4_col1" class="data row4 col1" >$4.26</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow4_col2" class="data row4 col2" >125</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow4_col3" class="data row4 col3" >$370.33</td> 
    </tr>    <tr> 
        <th id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fc" class="row_heading level0 row5" >30-34</th> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow5_col0" class="data row5 col0" >$3.08</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow5_col1" class="data row5 col1" >$4.20</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow5_col2" class="data row5 col2" >64</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow5_col3" class="data row5 col3" >$197.25</td> 
    </tr>    <tr> 
        <th id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fc" class="row_heading level0 row6" >35-39</th> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow6_col0" class="data row6 col0" >$2.84</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow6_col1" class="data row6 col1" >$4.42</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow6_col2" class="data row6 col2" >42</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow6_col3" class="data row6 col3" >$119.40</td> 
    </tr>    <tr> 
        <th id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fc" class="row_heading level0 row7" >40+</th> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow7_col0" class="data row7 col0" >$3.16</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow7_col1" class="data row7 col1" >$4.89</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow7_col2" class="data row7 col2" >17</td> 
        <td id="T_05dc96e8_8da3_11e7_8a21_dca9047bd1fcrow7_col3" class="data row7 col3" >$53.75</td> 
    </tr></tbody> 
</table> 



### Top Spenders


```python
# Top Spenders
SpendersGroup = df.groupby("SN").sum().reset_index()
SpendersGroup.sort_values("Price", ascending=False, inplace=True)

SpendersCount = df.groupby("SN").count().reset_index()
SpendersSum = df.groupby("SN").sum().reset_index()

TopSpenders = SpendersGroup.iloc[0:5, 0:1]

Merge1 = pd.merge(TopSpenders, SpendersCount.iloc[:, 0:2], on="SN")
Merge1.columns.values[1] = "Purchase Count"

Merge2 = pd.merge(Merge1, SpendersSum.iloc[:, [0, 3]], on="SN")
Merge2["Average Purchase Price"] = Merge2["Price"] / Merge2["Purchase Count"]
Merge2.columns.values[2] = "Total Purchase Value"

Merge2 = Merge2.style.format({"Average Purchase Price": "${:.2f}",
                              "Total Purchase Value": "${:.2f}"})
Merge2
```




<style  type="text/css" >
</style>  
<table id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fc" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >SN</th> 
        <th class="col_heading level0 col1" >Purchase Count</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
        <th class="col_heading level0 col3" >Average Purchase Price</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fc" class="row_heading level0 row0" >0</th> 
        <td id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fcrow0_col0" class="data row0 col0" >Undirrala66</td> 
        <td id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fcrow0_col1" class="data row0 col1" >5</td> 
        <td id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fcrow0_col2" class="data row0 col2" >$17.06</td> 
        <td id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fcrow0_col3" class="data row0 col3" >$3.41</td> 
    </tr>    <tr> 
        <th id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fc" class="row_heading level0 row1" >1</th> 
        <td id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fcrow1_col0" class="data row1 col0" >Saedue76</td> 
        <td id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fcrow1_col1" class="data row1 col1" >4</td> 
        <td id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fcrow1_col2" class="data row1 col2" >$13.56</td> 
        <td id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fcrow1_col3" class="data row1 col3" >$3.39</td> 
    </tr>    <tr> 
        <th id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fc" class="row_heading level0 row2" >2</th> 
        <td id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fcrow2_col0" class="data row2 col0" >Mindimnya67</td> 
        <td id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fcrow2_col1" class="data row2 col1" >4</td> 
        <td id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fcrow2_col2" class="data row2 col2" >$12.74</td> 
        <td id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fcrow2_col3" class="data row2 col3" >$3.18</td> 
    </tr>    <tr> 
        <th id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fc" class="row_heading level0 row3" >3</th> 
        <td id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fcrow3_col0" class="data row3 col0" >Haellysu29</td> 
        <td id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fcrow3_col1" class="data row3 col1" >3</td> 
        <td id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fcrow3_col2" class="data row3 col2" >$12.73</td> 
        <td id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fcrow3_col3" class="data row3 col3" >$4.24</td> 
    </tr>    <tr> 
        <th id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fc" class="row_heading level0 row4" >4</th> 
        <td id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fcrow4_col0" class="data row4 col0" >Eoda93</td> 
        <td id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fcrow4_col1" class="data row4 col1" >3</td> 
        <td id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fcrow4_col2" class="data row4 col2" >$11.58</td> 
        <td id="T_58ded2b6_8da0_11e7_b930_dca9047bd1fcrow4_col3" class="data row4 col3" >$3.86</td> 
    </tr></tbody> 
</table> 



### Most Popular Items


```python
# Most Popular Items
PopularGroup = df.groupby("Item Name").count().reset_index()
PopularGroup.sort_values("Price", ascending=False, inplace=True)

PopularCount = df.groupby("Item Name").count().reset_index()
PopularSum = df.groupby("Item Name").sum().reset_index()

MostPopular = PopularGroup.iloc[0:5, 0:1]

Merge3 = pd.merge(MostPopular, PopularCount.iloc[:, 0:2], on="Item Name")
Merge3.columns.values[1] = "Purchase Count"

Merge4 = pd.merge(Merge3, PopularSum.iloc[:, [0, 3]], on="Item Name")
Merge4["Average Purchase Price"] = Merge4["Price"] / Merge4["Purchase Count"]
Merge4.columns.values[2] = "Total Purchase Value"

Merge4 = Merge4.style.format({"Average Purchase Price": "${:.2f}",
                              "Total Purchase Value": "${:.2f}"})
Merge4
```




<style  type="text/css" >
</style>  
<table id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fc" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Item Name</th> 
        <th class="col_heading level0 col1" >Purchase Count</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
        <th class="col_heading level0 col3" >Average Purchase Price</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fc" class="row_heading level0 row0" >0</th> 
        <td id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fcrow0_col0" class="data row0 col0" >Final Critic</td> 
        <td id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fcrow0_col1" class="data row0 col1" >14</td> 
        <td id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fcrow0_col2" class="data row0 col2" >$38.60</td> 
        <td id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fcrow0_col3" class="data row0 col3" >$2.76</td> 
    </tr>    <tr> 
        <th id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fc" class="row_heading level0 row1" >1</th> 
        <td id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fcrow1_col0" class="data row1 col0" >Arcane Gem</td> 
        <td id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fcrow1_col1" class="data row1 col1" >11</td> 
        <td id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fcrow1_col2" class="data row1 col2" >$24.53</td> 
        <td id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fcrow1_col3" class="data row1 col3" >$2.23</td> 
    </tr>    <tr> 
        <th id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fc" class="row_heading level0 row2" >2</th> 
        <td id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fcrow2_col0" class="data row2 col0" >Betrayal, Whisper of Grieving Widows</td> 
        <td id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fcrow2_col1" class="data row2 col1" >11</td> 
        <td id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fcrow2_col2" class="data row2 col2" >$25.85</td> 
        <td id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fcrow2_col3" class="data row2 col3" >$2.35</td> 
    </tr>    <tr> 
        <th id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fc" class="row_heading level0 row3" >3</th> 
        <td id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fcrow3_col0" class="data row3 col0" >Stormcaller</td> 
        <td id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fcrow3_col1" class="data row3 col1" >10</td> 
        <td id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fcrow3_col2" class="data row3 col2" >$34.65</td> 
        <td id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fcrow3_col3" class="data row3 col3" >$3.46</td> 
    </tr>    <tr> 
        <th id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fc" class="row_heading level0 row4" >4</th> 
        <td id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fcrow4_col0" class="data row4 col0" >Woeful Adamantite Claymore</td> 
        <td id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fcrow4_col1" class="data row4 col1" >9</td> 
        <td id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fcrow4_col2" class="data row4 col2" >$11.16</td> 
        <td id="T_58e53a18_8da0_11e7_9a92_dca9047bd1fcrow4_col3" class="data row4 col3" >$1.24</td> 
    </tr></tbody> 
</table> 



### Most Profitable Items


```python
# Most Profitable Items
ProfitableGroup = df.groupby("Item Name").sum().reset_index()
ProfitableGroup.sort_values("Price", ascending=False, inplace=True)

ProfitableCount = df.groupby("Item Name").count().reset_index()
ProfitableSum = df.groupby("Item Name").sum().reset_index()

MostProfitable = ProfitableGroup.iloc[0:5, 0:1]

Merge5 = pd.merge(MostProfitable, ProfitableCount.iloc[:, 0:2], on="Item Name")
Merge5.columns.values[1] = "Purchase Count"

Merge6 = pd.merge(Merge5, ProfitableSum.iloc[:, [0, 3]], on="Item Name")
Merge6["Average Purchase Price"] = Merge6["Price"] / Merge6["Purchase Count"]
Merge6.columns.values[2] = "Total Purchase Value"

Merge6 = Merge6.style.format({"Average Purchase Price": "${:.2f}",
                              "Total Purchase Value": "${:.2f}"})
Merge6
```




<style  type="text/css" >
</style>  
<table id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fc" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Item Name</th> 
        <th class="col_heading level0 col1" >Purchase Count</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
        <th class="col_heading level0 col3" >Average Purchase Price</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fc" class="row_heading level0 row0" >0</th> 
        <td id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fcrow0_col0" class="data row0 col0" >Final Critic</td> 
        <td id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fcrow0_col1" class="data row0 col1" >14</td> 
        <td id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fcrow0_col2" class="data row0 col2" >$38.60</td> 
        <td id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fcrow0_col3" class="data row0 col3" >$2.76</td> 
    </tr>    <tr> 
        <th id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fc" class="row_heading level0 row1" >1</th> 
        <td id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fcrow1_col0" class="data row1 col0" >Retribution Axe</td> 
        <td id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fcrow1_col1" class="data row1 col1" >9</td> 
        <td id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fcrow1_col2" class="data row1 col2" >$37.26</td> 
        <td id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fcrow1_col3" class="data row1 col3" >$4.14</td> 
    </tr>    <tr> 
        <th id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fc" class="row_heading level0 row2" >2</th> 
        <td id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fcrow2_col0" class="data row2 col0" >Stormcaller</td> 
        <td id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fcrow2_col1" class="data row2 col1" >10</td> 
        <td id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fcrow2_col2" class="data row2 col2" >$34.65</td> 
        <td id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fcrow2_col3" class="data row2 col3" >$3.46</td> 
    </tr>    <tr> 
        <th id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fc" class="row_heading level0 row3" >3</th> 
        <td id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fcrow3_col0" class="data row3 col0" >Spectral Diamond Doomblade</td> 
        <td id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fcrow3_col1" class="data row3 col1" >7</td> 
        <td id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fcrow3_col2" class="data row3 col2" >$29.75</td> 
        <td id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fcrow3_col3" class="data row3 col3" >$4.25</td> 
    </tr>    <tr> 
        <th id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fc" class="row_heading level0 row4" >4</th> 
        <td id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fcrow4_col0" class="data row4 col0" >Orenmir</td> 
        <td id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fcrow4_col1" class="data row4 col1" >6</td> 
        <td id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fcrow4_col2" class="data row4 col2" >$29.70</td> 
        <td id="T_58ec7aa6_8da0_11e7_ae24_dca9047bd1fcrow4_col3" class="data row4 col3" >$4.95</td> 
    </tr></tbody> 
</table> 


