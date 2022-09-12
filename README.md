
# Data Cleaning and Scrubbing

 In this project, I performed data preprocessing on two different datasets using Python.
 The first dataset contains data about consumer purchases. The scond dataset is from a database that stores students exam scores. 
 I started by introducing the metadata of each dataset to fully understand it. Then, I cleaned and prepared the data based on the description of the problem statment. 



<h1>Table of Contents<span class="tocSkip"></span></h1>
<div class="toc"><ul class="toc-item"><li><span><a href="#Problem-#A" data-toc-modified-id="Problem-#A-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Problem #A: Cleaning Consumer Goods Dataset</a></span></li><li><span><a href="#Problem-#B" data-toc-modified-id="Problem-#B-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>Problem #B: Cleaning Student Information Dataset 


## Metadata and Details



**Problem # A**

Consider the data given in CSV file **Consumer Data** obtained from a public repository.

Consider the following data description:

Table 1: Data Description

| Field   |      Description      |
|----------|:-------------:|
| Channel |  The mechanism through which the goods were consumed. Contains two values: Hotels or Retail. |
| City Town | The City/Town from which the data is collected   |
| Fresh | Annual spending (in SAR) on the fresh products. |
| Milk   |  Annual consumption (in liter) of milk products.     |
|Grocery | Annual spending (in SAR) on the grocery products.|
| Frozen |  Annual spending (in SAR) on the frozen products. |
| Detergents Paper |  Annual spending (in SAR) on the detergents and paper products|
| Delicassen | Annual spending (in SAR) on the delicatessen products. |

Do the following tasks using data given in **Consumer Data** and Table-1:

*A*-1: **Given Data.** Read and display the data. Identify the fields of the data and count the number of rows and columns.

*A*-2: **Type Consistency.** Identify the type for each field based on value. Report any inconsistency.

*A*-3: **Filter noise.** Any record whose channel value is neither “Retail” nor “Hotels” should be removed.

*A*-4: **Data Wrangling/Munging.** Columns “Fresh”, “Milk”, “Grocery”, “Frozen”, “Detergents Paper” and “Delicassen” should be numeric. Resolve the inconsistency. The value in the “Milk” column is in liters and should be converted to SAR. The conversion can be done by multiplying values in the “Milk” column by 9.8. Round the values in the milk column to the nearest integer.

*A*-5: **Handling NaN values.** All missing values in “Channel” field corresponds to “Hotels”. Similarly,all the missing values in “City Town” fields corresponds to “Khobar”. The missing values in “Detergents Paper” field should be replaced by mean, rounded to the nearest integer.

*A*-6: **Encoding.** Pick field “Channel”, and relabel “Hotels” as 1, and “Retail” as 0.

*A*-7: **Feature Generation.** Create a new filed, called “Region”. The values in region should be as follows:
Region value for “Riyadh”, “Qaseem” or “Hail” should be “Central”.
Region value for “Tabuk”, “Makkah” or “Madinah” should be “Western”.
Region value for “Khobar”, “Dammam” or “Dhahran” should be “Eastern”.

*A*-8: **One-Hot-Encoding.** Do One-Hot-Encoding for “Region” column. Do not delete the original column.

*A*-9: **Standardization.** For all the following columns, do standard scalarization, such that the mean value is 0, and the standard deviation is 1: “Fresh”, “Milk”, “Grocery”, “Frozen”, “Detergents Paper” and “Delicassen”

*A*-10: **Clean & Prepared Data.** Display the modified data and the default summary statisticsfor all the columns.



## Problem #A

## A-1


```python
#A-1: Understand the Given Data
import pandas as pd
import numpy as np
df = pd.read_csv('Consumer Data.csv', delimiter = ',') # Read
display(df.sample(20)) # and display the random 20 rows of data
df.set_index('Index',inplace=True) # The column with “Index” labe should be the index of the data
df.info() #Identify the fields of the data
print()
# Count the number of rows and columns inthe data
print(f'The number of rows are {len(df.index)}, and the number of columns are {len(df.columns)}')
# Count the number of non-null rows for each column
print(f'The number of non-null rows for each column are:\n{df.count()}')
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Index</th>
      <th>Channel</th>
      <th>City_Town</th>
      <th>Fresh</th>
      <th>Milk</th>
      <th>Grocery</th>
      <th>Frozen</th>
      <th>Detergents_Paper</th>
      <th>Delicassen</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>236</th>
      <td>236</td>
      <td>Retail</td>
      <td>Makkah</td>
      <td>$1,479.00</td>
      <td>1498</td>
      <td>$11,924.00</td>
      <td>$662.00</td>
      <td>3891.0</td>
      <td>3508</td>
    </tr>
    <tr>
      <th>331</th>
      <td>331</td>
      <td>Retail</td>
      <td>Khobar</td>
      <td>$2,932.00</td>
      <td>646</td>
      <td>$7,677.00</td>
      <td>$2,561.00</td>
      <td>4573.0</td>
      <td>1386</td>
    </tr>
    <tr>
      <th>273</th>
      <td>273</td>
      <td>Hotels</td>
      <td>Hail</td>
      <td>$583.00</td>
      <td>69</td>
      <td>$2,216.00</td>
      <td>$469.00</td>
      <td>954.0</td>
      <td>18</td>
    </tr>
    <tr>
      <th>91</th>
      <td>91</td>
      <td>Retail</td>
      <td>Khobar</td>
      <td>$219.00</td>
      <td>954</td>
      <td>$14,403.00</td>
      <td>$283.00</td>
      <td>7818.0</td>
      <td>156</td>
    </tr>
    <tr>
      <th>170</th>
      <td>170</td>
      <td>Hotels</td>
      <td>Qaseem</td>
      <td>$18,567.00</td>
      <td>190</td>
      <td>$1,393.00</td>
      <td>$1,801.00</td>
      <td>244.0</td>
      <td>2100</td>
    </tr>
    <tr>
      <th>186</th>
      <td>186</td>
      <td>Hotels</td>
      <td>Khobar</td>
      <td>$1,198.00</td>
      <td>260</td>
      <td>$8,335.00</td>
      <td>$402.00</td>
      <td>3843.0</td>
      <td>303</td>
    </tr>
    <tr>
      <th>392</th>
      <td>392</td>
      <td>Hotels</td>
      <td>Dhahran</td>
      <td>$7,864.00</td>
      <td>54</td>
      <td>$4,042.00</td>
      <td>$9,735.00</td>
      <td>165.0</td>
      <td>46</td>
    </tr>
    <tr>
      <th>136</th>
      <td>136</td>
      <td>Hotels</td>
      <td>Dammam</td>
      <td>$24,025.00</td>
      <td>433</td>
      <td>$4,757.00</td>
      <td>$9,510.00</td>
      <td>1145.0</td>
      <td>5864</td>
    </tr>
    <tr>
      <th>340</th>
      <td>340</td>
      <td>Retail</td>
      <td>Dammam</td>
      <td>$5,224.00</td>
      <td>760</td>
      <td>$8,584.00</td>
      <td>$2,540.00</td>
      <td>3674.0</td>
      <td>238</td>
    </tr>
    <tr>
      <th>21</th>
      <td>21</td>
      <td>Hotels</td>
      <td>Khobar</td>
      <td>$68,951.00</td>
      <td>441</td>
      <td>$12,609.00</td>
      <td>$8,692.00</td>
      <td>751.0</td>
      <td>2406</td>
    </tr>
    <tr>
      <th>292</th>
      <td>292</td>
      <td>Hotels</td>
      <td>Khobar</td>
      <td>$2,126.00</td>
      <td>329</td>
      <td>$3,281.00</td>
      <td>$1,535.00</td>
      <td>235.0</td>
      <td>4365</td>
    </tr>
    <tr>
      <th>58</th>
      <td>58</td>
      <td>Hotels</td>
      <td>Dammam</td>
      <td>$25,066.00</td>
      <td>501</td>
      <td>$5,026.00</td>
      <td>$9,806.00</td>
      <td>1092.0</td>
      <td>960</td>
    </tr>
    <tr>
      <th>406</th>
      <td>406</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>$29,729.00</td>
      <td>479</td>
      <td>$7,326.00</td>
      <td>$6,130.00</td>
      <td>361.0</td>
      <td>1083</td>
    </tr>
    <tr>
      <th>230</th>
      <td>230</td>
      <td>Retail</td>
      <td>Qaseem</td>
      <td>$5,550.00</td>
      <td>1273</td>
      <td>$16,767.00</td>
      <td>$864.00</td>
      <td>12420.0</td>
      <td>797</td>
    </tr>
    <tr>
      <th>359</th>
      <td>359</td>
      <td>Hotels</td>
      <td>Khobar</td>
      <td>$8,258.00</td>
      <td>234</td>
      <td>$2,147.00</td>
      <td>$3,896.00</td>
      <td>266.0</td>
      <td>635</td>
    </tr>
    <tr>
      <th>368</th>
      <td>368</td>
      <td>Hotels</td>
      <td>Khobar</td>
      <td>$5,924.00</td>
      <td>58</td>
      <td>$542.00</td>
      <td>$4,052.00</td>
      <td>283.0</td>
      <td>434</td>
    </tr>
    <tr>
      <th>277</th>
      <td>277</td>
      <td>Hotels</td>
      <td>Riyadh</td>
      <td>$18,044.00</td>
      <td>148</td>
      <td>$2,046.00</td>
      <td>$2,532.00</td>
      <td>130.0</td>
      <td>1158</td>
    </tr>
    <tr>
      <th>35</th>
      <td>35</td>
      <td>Hotels</td>
      <td>Madinah</td>
      <td>$16,933.00</td>
      <td>221</td>
      <td>$3,389.00</td>
      <td>$7,849.00</td>
      <td>210.0</td>
      <td>1534</td>
    </tr>
    <tr>
      <th>222</th>
      <td>222</td>
      <td>Hotels</td>
      <td>Dhahran</td>
      <td>$4,983.00</td>
      <td>486</td>
      <td>$6,633.00</td>
      <td>$17,866.00</td>
      <td>912.0</td>
      <td>2435</td>
    </tr>
    <tr>
      <th>358</th>
      <td>358</td>
      <td>Hotels</td>
      <td>Khobar</td>
      <td>$12,212.00</td>
      <td>20</td>
      <td>$245.00</td>
      <td>$1,991.00</td>
      <td>25.0</td>
      <td>860</td>
    </tr>
  </tbody>
</table>
</div>


    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 451 entries, 0 to 450
    Data columns (total 8 columns):
     #   Column            Non-Null Count  Dtype  
    ---  ------            --------------  -----  
     0   Channel           417 non-null    object 
     1   City_Town         417 non-null    object 
     2   Fresh             451 non-null    object 
     3   Milk              451 non-null    int64  
     4   Grocery           451 non-null    object 
     5   Frozen            451 non-null    object 
     6   Detergents_Paper  446 non-null    float64
     7   Delicassen        451 non-null    int64  
    dtypes: float64(1), int64(2), object(5)
    memory usage: 31.7+ KB
    
    The number of rows are 451, and the number of columns are 8
    The number of non-null rows for each column are:
    Channel             417
    City_Town           417
    Fresh               451
    Milk                451
    Grocery             451
    Frozen              451
    Detergents_Paper    446
    Delicassen          451
    dtype: int64
    

## A-2


```python
#A-2: Check for Type Consistency

df.info() #Identify the type for each field based on value
display(df.sample(5))
print('''As can be seen from the sample and info below:
Channel field is an "object" Dtype, and is consistent (but has some NaN values)
City_Town field is an "object" Dtype, and is consistent (but has some NaN values)
Fresh field should be in numbers, while its values are "object" Dtype. Therefore, it is not consistent
Milk field should be in numbers, while its values are "object" Dtype. Therefore, it is not consistent
Grocery field should be in numbers, while its values are "object" Dtype. Therefore, it is not consistent 
Frozen field should be in numbers, while its values are "object" Dtype. Therefore, it is not consistent 
Detergents_Paper field is in numbers(float Dtype), while its values do not appear to be needing any decimals (should be int Dtype)
Delicassen Detergents_Paper field is in numbers as it should be. Therfore, it is consistent''')
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 451 entries, 0 to 450
    Data columns (total 8 columns):
     #   Column            Non-Null Count  Dtype  
    ---  ------            --------------  -----  
     0   Channel           417 non-null    object 
     1   City_Town         417 non-null    object 
     2   Fresh             451 non-null    object 
     3   Milk              451 non-null    int64  
     4   Grocery           451 non-null    object 
     5   Frozen            451 non-null    object 
     6   Detergents_Paper  446 non-null    float64
     7   Delicassen        451 non-null    int64  
    dtypes: float64(1), int64(2), object(5)
    memory usage: 31.7+ KB
    



<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Channel</th>
      <th>City_Town</th>
      <th>Fresh</th>
      <th>Milk</th>
      <th>Grocery</th>
      <th>Frozen</th>
      <th>Detergents_Paper</th>
      <th>Delicassen</th>
    </tr>
    <tr>
      <th>Index</th>
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
      <th>154</th>
      <td>Hotels</td>
      <td>Riyadh</td>
      <td>$11,092.00</td>
      <td>501</td>
      <td>$5,249.00</td>
      <td>$453.00</td>
      <td>392.0</td>
      <td>373</td>
    </tr>
    <tr>
      <th>269</th>
      <td>Hotels</td>
      <td>Dhahran</td>
      <td>$13,265.00</td>
      <td>120</td>
      <td>$4,221.00</td>
      <td>$6,404.00</td>
      <td>507.0</td>
      <td>1788</td>
    </tr>
    <tr>
      <th>324</th>
      <td>Hotels</td>
      <td>Dammam</td>
      <td>$7,780.00</td>
      <td>250</td>
      <td>$9,464.00</td>
      <td>$669.00</td>
      <td>2518.0</td>
      <td>501</td>
    </tr>
    <tr>
      <th>199</th>
      <td>Hotels</td>
      <td>Dammam</td>
      <td>$13,134.00</td>
      <td>935</td>
      <td>$14,316.00</td>
      <td>$3,141.00</td>
      <td>5079.0</td>
      <td>1894</td>
    </tr>
    <tr>
      <th>43</th>
      <td>Hotels</td>
      <td>Tabuk</td>
      <td>$444.00</td>
      <td>88</td>
      <td>$2,060.00</td>
      <td>$264.00</td>
      <td>290.0</td>
      <td>259</td>
    </tr>
  </tbody>
</table>
</div>


    As can be seen from the sample and info below:
    Channel field is an "object" Dtype, and is consistent (but has some NaN values)
    City_Town field is an "object" Dtype, and is consistent (but has some NaN values)
    Fresh field should be in numbers, while its values are "object" Dtype. Therefore, it is not consistent
    Milk field should be in numbers, while its values are "object" Dtype. Therefore, it is not consistent
    Grocery field should be in numbers, while its values are "object" Dtype. Therefore, it is not consistent 
    Frozen field should be in numbers, while its values are "object" Dtype. Therefore, it is not consistent 
    Detergents_Paper field is in numbers(float Dtype), while its values do not appear to be needing any decimals (should be int Dtype)
    Delicassen Detergents_Paper field is in numbers as it should be. Therfore, it is consistent
    

## A-3


```python
#A-3: Filter Noise
uniques = df['Channel'].unique().tolist()
print(uniques) # to check the unique values before removing values
selected_rows = df[df['Channel'] == "Export"].index
df.drop(selected_rows,inplace=True) #  removing Export records
# df.dropna(axis=0, how='any', inplace=True) # dropping any row containing NaN values
uniques = df['Channel'].unique().tolist()
print(uniques) # to check the unique values after removing values
# nan values will be removed in A-5
```

    ['Retail', 'Hotels', 'Export', nan]
    ['Retail', 'Hotels', nan]
    

## A-4


```python
#A-4: Data Wrangling/Munging
df.loc[:,["Fresh","Grocery","Frozen"]]=df.loc[:,["Fresh","Grocery","Frozen"]].applymap( # removing each character
    lambda x:x.replace("$","").replace(",","").replace(".00","")).applymap(pd.to_numeric)# converting them to numerics
df["Milk"] = np.round(df["Milk"] * 9.8) # converting litters to Riyals 
display(df.sample(5))
```



<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Channel</th>
      <th>City_Town</th>
      <th>Fresh</th>
      <th>Milk</th>
      <th>Grocery</th>
      <th>Frozen</th>
      <th>Detergents_Paper</th>
      <th>Delicassen</th>
    </tr>
    <tr>
      <th>Index</th>
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
      <th>296</th>
      <td>Hotels</td>
      <td>Khobar</td>
      <td>3884</td>
      <td>3724.0</td>
      <td>1641</td>
      <td>876</td>
      <td>397.0</td>
      <td>4829</td>
    </tr>
    <tr>
      <th>406</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>29729</td>
      <td>4694.0</td>
      <td>7326</td>
      <td>6130</td>
      <td>361.0</td>
      <td>1083</td>
    </tr>
    <tr>
      <th>303</th>
      <td>Hotels</td>
      <td>Dammam</td>
      <td>31012</td>
      <td>16356.0</td>
      <td>5429</td>
      <td>15082</td>
      <td>439.0</td>
      <td>1163</td>
    </tr>
    <tr>
      <th>212</th>
      <td>Hotels</td>
      <td>Tabuk</td>
      <td>13970</td>
      <td>1480.0</td>
      <td>1330</td>
      <td>650</td>
      <td>146.0</td>
      <td>778</td>
    </tr>
    <tr>
      <th>405</th>
      <td>Retail</td>
      <td>Khobar</td>
      <td>1689</td>
      <td>6821.0</td>
      <td>26316</td>
      <td>1456</td>
      <td>15469.0</td>
      <td>37</td>
    </tr>
  </tbody>
</table>
</div>


## A-5


```python
#A-5: Handling NaN values
null = df.columns[df.isna().any()] 
print(null) # to check the columns containing NaN values before removing them
df["Channel"].fillna("Hotels",inplace=True)
df["City_Town"].fillna("Khobar",inplace=True)
df["Detergents_Paper"].fillna(int(df["Detergents_Paper"].mean()),inplace=True)
null = df.columns[df.isna().any()]
print(null) # to check that there are not any columns containing NaN values 
```

    Index(['Channel', 'City_Town', 'Detergents_Paper'], dtype='object')
    Index([], dtype='object')
    

## A-6


```python
#A-6: Encoding
Channel_dict = {'Retail':0,'Hotels':1} # creating dictionary for mapping
df["Channel"] = df["Channel"].map(Channel_dict) 
```

## A-7


```python
#A-7: Feature Generation. Create a new filed, called “Region”. (the details can be found in the PDF)
Region_dict = {"Riyadh":"Central", "Qaseem":"Central","Hail":"Central", # creating dictionary for mapping
    "Tabuk":"Western", "Makkah":"Western", "Madinah":"Western",
    "Khobar":"Eastern", "Dammam":"Eastern", "Dhahran":"Eastern"}
df["Region"] = df["City_Town"].map(Region_dict)
```

## A-8


```python
#A-8: One-Hot-Encoding
df = pd.get_dummies(df, columns=['Region'],drop_first=False).join(df["Region"]) #.join to keep original column
                                        # drop_first is False in order to keep Region_Central column 
df
```





<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Channel</th>
      <th>City_Town</th>
      <th>Fresh</th>
      <th>Milk</th>
      <th>Grocery</th>
      <th>Frozen</th>
      <th>Detergents_Paper</th>
      <th>Delicassen</th>
      <th>Region_Central</th>
      <th>Region_Eastern</th>
      <th>Region_Western</th>
      <th>Region</th>
    </tr>
    <tr>
      <th>Index</th>
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
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Dhahran</td>
      <td>11170</td>
      <td>10555.0</td>
      <td>8814</td>
      <td>2194</td>
      <td>1976.0</td>
      <td>143</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>Dammam</td>
      <td>21217</td>
      <td>6086.0</td>
      <td>14982</td>
      <td>3095</td>
      <td>6707.0</td>
      <td>602</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>Khobar</td>
      <td>5963</td>
      <td>3577.0</td>
      <td>6192</td>
      <td>425</td>
      <td>1716.0</td>
      <td>750</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>Khobar</td>
      <td>9413</td>
      <td>8095.0</td>
      <td>5126</td>
      <td>666</td>
      <td>1795.0</td>
      <td>1451</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>Dhahran</td>
      <td>5969</td>
      <td>1950.0</td>
      <td>3417</td>
      <td>5679</td>
      <td>1135.0</td>
      <td>290</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
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
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>446</th>
      <td>1</td>
      <td>Dammam</td>
      <td>717</td>
      <td>3518.0</td>
      <td>6532</td>
      <td>7530</td>
      <td>529.0</td>
      <td>894</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>447</th>
      <td>1</td>
      <td>Dhahran</td>
      <td>22335</td>
      <td>1176.0</td>
      <td>2406</td>
      <td>2046</td>
      <td>101.0</td>
      <td>558</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>448</th>
      <td>1</td>
      <td>Khobar</td>
      <td>45640</td>
      <td>6821.0</td>
      <td>6536</td>
      <td>7368</td>
      <td>1532.0</td>
      <td>230</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>449</th>
      <td>1</td>
      <td>Khobar</td>
      <td>23632</td>
      <td>6595.0</td>
      <td>3842</td>
      <td>8620</td>
      <td>385.0</td>
      <td>819</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>450</th>
      <td>0</td>
      <td>Madinah</td>
      <td>918</td>
      <td>20247.0</td>
      <td>13567</td>
      <td>1465</td>
      <td>6846.0</td>
      <td>806</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>Western</td>
    </tr>
  </tbody>
</table>
<p>440 rows × 12 columns</p>
</div>




```python
#A-9: Standardization: do standard scaling, such that the mean value is 0, and the standard deviation is 1.
from sklearn.preprocessing import StandardScaler
selected_columns = df.loc[:,['Fresh', 'Milk', 'Grocery', 'Frozen', 'Detergents_Paper','Delicassen']].columns
for c in selected_columns:
    scaler = StandardScaler() # creating object
    scaler.fit(df[[c]]) # fitting
    df[c]= scaler.transform(df[[c]]) #transforming
    print(f'The mean and std values for {c} are', abs(np.round(df[c].mean())), ' and ', np.round(df[c].std()))
df
```

    The mean and std values for Fresh are 0.0  and  1.0
    The mean and std values for Milk are 0.0  and  1.0
    The mean and std values for Grocery are 0.0  and  1.0
    The mean and std values for Frozen are 0.0  and  1.0
    The mean and std values for Detergents_Paper are 0.0  and  1.0
    The mean and std values for Delicassen are 0.0  and  1.0
    





<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Channel</th>
      <th>City_Town</th>
      <th>Fresh</th>
      <th>Milk</th>
      <th>Grocery</th>
      <th>Frozen</th>
      <th>Detergents_Paper</th>
      <th>Delicassen</th>
      <th>Region_Central</th>
      <th>Region_Eastern</th>
      <th>Region_Western</th>
      <th>Region</th>
    </tr>
    <tr>
      <th>Index</th>
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
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Dhahran</td>
      <td>-0.065725</td>
      <td>0.674671</td>
      <td>0.090886</td>
      <td>-0.181048</td>
      <td>-0.190230</td>
      <td>-0.490564</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>Dammam</td>
      <td>0.729576</td>
      <td>0.056077</td>
      <td>0.740672</td>
      <td>0.004757</td>
      <td>0.803172</td>
      <td>-0.327619</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>Khobar</td>
      <td>-0.477901</td>
      <td>-0.291216</td>
      <td>-0.185336</td>
      <td>-0.545854</td>
      <td>-0.244824</td>
      <td>-0.275079</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>Khobar</td>
      <td>-0.204806</td>
      <td>0.334161</td>
      <td>-0.297637</td>
      <td>-0.496155</td>
      <td>-0.228236</td>
      <td>-0.026224</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>Dhahran</td>
      <td>-0.477426</td>
      <td>-0.516423</td>
      <td>-0.477677</td>
      <td>0.537634</td>
      <td>-0.366821</td>
      <td>-0.438379</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
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
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>446</th>
      <td>1</td>
      <td>Dammam</td>
      <td>-0.893164</td>
      <td>-0.299382</td>
      <td>-0.149518</td>
      <td>0.919350</td>
      <td>-0.494067</td>
      <td>-0.223959</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>447</th>
      <td>1</td>
      <td>Dhahran</td>
      <td>0.818075</td>
      <td>-0.623559</td>
      <td>-0.584183</td>
      <td>-0.211569</td>
      <td>-0.583937</td>
      <td>-0.343239</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>448</th>
      <td>1</td>
      <td>Khobar</td>
      <td>2.662854</td>
      <td>0.157815</td>
      <td>-0.149096</td>
      <td>0.885942</td>
      <td>-0.283460</td>
      <td>-0.459679</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>449</th>
      <td>1</td>
      <td>Khobar</td>
      <td>0.920743</td>
      <td>0.126532</td>
      <td>-0.432904</td>
      <td>1.144131</td>
      <td>-0.524304</td>
      <td>-0.250584</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>450</th>
      <td>0</td>
      <td>Madinah</td>
      <td>-0.877253</td>
      <td>2.016227</td>
      <td>0.591605</td>
      <td>-0.331384</td>
      <td>0.832359</td>
      <td>-0.255199</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>Western</td>
    </tr>
  </tbody>
</table>
<p>440 rows × 12 columns</p>
</div>



## A-10


```python
#A-10: Display the Clean & Prepared Data:
display(df)
display(df.info())
display(df.describe(include="number"))# only numerical
display(df.describe(include="object")) # only categorical
```



<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Channel</th>
      <th>City_Town</th>
      <th>Fresh</th>
      <th>Milk</th>
      <th>Grocery</th>
      <th>Frozen</th>
      <th>Detergents_Paper</th>
      <th>Delicassen</th>
      <th>Region_Central</th>
      <th>Region_Eastern</th>
      <th>Region_Western</th>
      <th>Region</th>
    </tr>
    <tr>
      <th>Index</th>
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
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Dhahran</td>
      <td>-0.065725</td>
      <td>0.674671</td>
      <td>0.090886</td>
      <td>-0.181048</td>
      <td>-0.190230</td>
      <td>-0.490564</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>Dammam</td>
      <td>0.729576</td>
      <td>0.056077</td>
      <td>0.740672</td>
      <td>0.004757</td>
      <td>0.803172</td>
      <td>-0.327619</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>Khobar</td>
      <td>-0.477901</td>
      <td>-0.291216</td>
      <td>-0.185336</td>
      <td>-0.545854</td>
      <td>-0.244824</td>
      <td>-0.275079</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>Khobar</td>
      <td>-0.204806</td>
      <td>0.334161</td>
      <td>-0.297637</td>
      <td>-0.496155</td>
      <td>-0.228236</td>
      <td>-0.026224</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>Dhahran</td>
      <td>-0.477426</td>
      <td>-0.516423</td>
      <td>-0.477677</td>
      <td>0.537634</td>
      <td>-0.366821</td>
      <td>-0.438379</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
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
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>446</th>
      <td>1</td>
      <td>Dammam</td>
      <td>-0.893164</td>
      <td>-0.299382</td>
      <td>-0.149518</td>
      <td>0.919350</td>
      <td>-0.494067</td>
      <td>-0.223959</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>447</th>
      <td>1</td>
      <td>Dhahran</td>
      <td>0.818075</td>
      <td>-0.623559</td>
      <td>-0.584183</td>
      <td>-0.211569</td>
      <td>-0.583937</td>
      <td>-0.343239</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>448</th>
      <td>1</td>
      <td>Khobar</td>
      <td>2.662854</td>
      <td>0.157815</td>
      <td>-0.149096</td>
      <td>0.885942</td>
      <td>-0.283460</td>
      <td>-0.459679</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>449</th>
      <td>1</td>
      <td>Khobar</td>
      <td>0.920743</td>
      <td>0.126532</td>
      <td>-0.432904</td>
      <td>1.144131</td>
      <td>-0.524304</td>
      <td>-0.250584</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>450</th>
      <td>0</td>
      <td>Madinah</td>
      <td>-0.877253</td>
      <td>2.016227</td>
      <td>0.591605</td>
      <td>-0.331384</td>
      <td>0.832359</td>
      <td>-0.255199</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>Western</td>
    </tr>
  </tbody>
</table>
<p>440 rows × 12 columns</p>
</div>


    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 440 entries, 0 to 450
    Data columns (total 12 columns):
     #   Column            Non-Null Count  Dtype  
    ---  ------            --------------  -----  
     0   Channel           440 non-null    int64  
     1   City_Town         440 non-null    object 
     2   Fresh             440 non-null    float64
     3   Milk              440 non-null    float64
     4   Grocery           440 non-null    float64
     5   Frozen            440 non-null    float64
     6   Detergents_Paper  440 non-null    float64
     7   Delicassen        440 non-null    float64
     8   Region_Central    440 non-null    uint8  
     9   Region_Eastern    440 non-null    uint8  
     10  Region_Western    440 non-null    uint8  
     11  Region            440 non-null    object 
    dtypes: float64(6), int64(1), object(2), uint8(3)
    memory usage: 51.8+ KB
    


    None




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Channel</th>
      <th>Fresh</th>
      <th>Milk</th>
      <th>Grocery</th>
      <th>Frozen</th>
      <th>Detergents_Paper</th>
      <th>Delicassen</th>
      <th>Region_Central</th>
      <th>Region_Eastern</th>
      <th>Region_Western</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>440.000000</td>
      <td>4.400000e+02</td>
      <td>4.400000e+02</td>
      <td>4.400000e+02</td>
      <td>4.400000e+02</td>
      <td>4.400000e+02</td>
      <td>440.000000</td>
      <td>440.0000</td>
      <td>440.000000</td>
      <td>440.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.677273</td>
      <td>-3.835316e-17</td>
      <td>-1.614870e-17</td>
      <td>-4.440892e-17</td>
      <td>2.220446e-17</td>
      <td>3.027881e-17</td>
      <td>0.000000</td>
      <td>0.1750</td>
      <td>0.718182</td>
      <td>0.106818</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.468052</td>
      <td>1.001138e+00</td>
      <td>1.001138e+00</td>
      <td>1.001138e+00</td>
      <td>1.001138e+00</td>
      <td>1.001138e+00</td>
      <td>1.001138</td>
      <td>0.3804</td>
      <td>0.450397</td>
      <td>0.309234</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>-9.496831e-01</td>
      <td>-7.781734e-01</td>
      <td>-8.373344e-01</td>
      <td>-6.283430e-01</td>
      <td>-6.045152e-01</td>
      <td>-0.540264</td>
      <td>0.0000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.000000</td>
      <td>-7.023339e-01</td>
      <td>-5.788502e-01</td>
      <td>-6.108364e-01</td>
      <td>-4.804306e-01</td>
      <td>-5.512335e-01</td>
      <td>-0.396401</td>
      <td>0.0000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>1.000000</td>
      <td>-2.767602e-01</td>
      <td>-2.946070e-01</td>
      <td>-3.366684e-01</td>
      <td>-3.188045e-01</td>
      <td>-4.336988e-01</td>
      <td>-0.198577</td>
      <td>0.0000</td>
      <td>1.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>1.000000</td>
      <td>3.905226e-01</td>
      <td>1.886134e-01</td>
      <td>2.849105e-01</td>
      <td>9.946441e-02</td>
      <td>2.183853e-01</td>
      <td>0.104860</td>
      <td>0.0000</td>
      <td>1.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1.000000</td>
      <td>7.927738e+00</td>
      <td>9.183973e+00</td>
      <td>8.936528e+00</td>
      <td>1.191900e+01</td>
      <td>7.967593e+00</td>
      <td>16.478447</td>
      <td>1.0000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City_Town</th>
      <th>Region</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>440</td>
      <td>440</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>9</td>
      <td>3</td>
    </tr>
    <tr>
      <th>top</th>
      <td>Khobar</td>
      <td>Eastern</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>125</td>
      <td>316</td>
    </tr>
  </tbody>
</table>
</div>

**Problem #B**

Consider data given in CSV file **Student Data** obtained from a public repository2. Consider the following

data description:

Table 2: Data Description

| Field   |      Description      |
|----------|:-------------:|
| gender |  Gender of the student |
| race/ethnicity | The ethnicity the student belongs to (masked)  |
| parental level of education | The highest level of education of either of the parent of the student |
| lunch   | Whether the student pays for lunch at standard or free/reduced rate  |
| test preparation course | Student took and completed the preparation course before the test or not |
| math score |  Score student received in the math assessment (out of 30) |
| reading score | Score student received in the reading assessment (out of 25) |
| writing score | Score student received in the writing assessment (out of 40) |
| gk score | Score student received in the general knowledge (GK) assessment (out of 100) |


Do the following tasks using the data given in **Student Data** and Table-2:

*B*-1: **Given Data.** Read and display the data. Identify the number of rows and columns. Does any column have missing data? Display the description of both numeric and non-numeric columns.

*B*-2: **Type Consistency.** For each column, identify type of each field and verify that each column in Python is identified correctly.

*B*-3: **Inconsistent Data.** Looking at the data, two types of inconsistencies were discovered, some of the scores were entered with negative sign (by mistake). Or in some cases, we found larger values than the possible maximum score. For all such entries, assume the score is out of 100, and scaleit out of corresponding maximum score.

*B*-4: **Handling NaN values.** For any missing data in the score columns, take the average score of the student from other exam scores and replace the NaN value.

*B*-5: **Handling NaN values.** For any missing data in the non-numeric columns, or replace the NaN value as follows: NaN value is to be replaced by the mode based on the race/ethnicity.

*B*-6: **Label Encoding.** Convert “parental level of education” and lunch using label encoder.

*B*-7: **One-Hot-Encoding**. For the ‘test preparation course’, convert it using one-hot-encoding.

*B*-8: **Normalization.** To be able to compare scores, scale all the scores between [0,1].

*B*-9: **Clean & Prepared Data.** Display the modified data, and the summary statistics for all columns



# Problem #B

## B-1


```python
#B-1: Understand the Given Data: 
import pandas as pd
import numpy as np
df = pd.read_csv('Student Data.csv', delimiter = ',')
display(df.head(5))

print(f'The number of rows are {len(df.index)}, and the number of columns are {len(df.columns)}')
print(f'The number of non-null rows for each column are:\n{df.count()}')
null1 =df.columns[df.isna().any()]
print(f'The columns containing missing data are:\n{null1}')
display(df.describe(include="number"))# only numerical
display(df.describe(include="object")) # only categorical
```



<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gender</th>
      <th>race/ethnicity</th>
      <th>parental level of education</th>
      <th>lunch</th>
      <th>test preparation course</th>
      <th>math score</th>
      <th>reading score</th>
      <th>writing score</th>
      <th>gk score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>female</td>
      <td>group B</td>
      <td>bachelor's degree</td>
      <td>standard</td>
      <td>none</td>
      <td>21.6</td>
      <td>72.00</td>
      <td>29.6</td>
      <td>71.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>female</td>
      <td>group C</td>
      <td>some college</td>
      <td>standard</td>
      <td>completed</td>
      <td>20.7</td>
      <td>22.50</td>
      <td>35.2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>female</td>
      <td>group B</td>
      <td>master's degree</td>
      <td>standard</td>
      <td>none</td>
      <td>27.0</td>
      <td>23.75</td>
      <td>37.2</td>
      <td>93.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>male</td>
      <td>group A</td>
      <td>associate's degree</td>
      <td>free/reduced</td>
      <td>none</td>
      <td>14.1</td>
      <td>NaN</td>
      <td>17.6</td>
      <td>56.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>male</td>
      <td>group C</td>
      <td>some college</td>
      <td>standard</td>
      <td>none</td>
      <td>76.0</td>
      <td>19.50</td>
      <td>30.0</td>
      <td>77.0</td>
    </tr>
  </tbody>
</table>
</div>


    The number of rows are 1000, and the number of columns are 9
    The number of non-null rows for each column are:
    gender                         1000
    race/ethnicity                 1000
    parental level of education    1000
    lunch                           991
    test preparation course        1000
    math score                      964
    reading score                   922
    writing score                   961
    gk score                        887
    dtype: int64
    The columns containing missing data are:
    Index(['lunch', 'math score', 'reading score', 'writing score', 'gk score'], dtype='object')
    



<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>math score</th>
      <th>reading score</th>
      <th>writing score</th>
      <th>gk score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>964.000000</td>
      <td>922.000000</td>
      <td>961.000000</td>
      <td>887.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>19.727905</td>
      <td>18.649675</td>
      <td>26.394589</td>
      <td>59.314543</td>
    </tr>
    <tr>
      <th>std</th>
      <td>14.076062</td>
      <td>15.468953</td>
      <td>15.261831</td>
      <td>35.745609</td>
    </tr>
    <tr>
      <th>min</th>
      <td>-29.100000</td>
      <td>-25.000000</td>
      <td>-36.800000</td>
      <td>-100.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>16.200000</td>
      <td>14.500000</td>
      <td>22.400000</td>
      <td>55.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>19.800000</td>
      <td>17.500000</td>
      <td>27.600000</td>
      <td>66.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>23.400000</td>
      <td>20.250000</td>
      <td>32.000000</td>
      <td>77.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>98.000000</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>103.000000</td>
    </tr>
  </tbody>
</table>
</div>




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gender</th>
      <th>race/ethnicity</th>
      <th>parental level of education</th>
      <th>lunch</th>
      <th>test preparation course</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1000</td>
      <td>1000</td>
      <td>1000</td>
      <td>991</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>2</td>
      <td>5</td>
      <td>6</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>top</th>
      <td>female</td>
      <td>group C</td>
      <td>some college</td>
      <td>standard</td>
      <td>none</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>518</td>
      <td>319</td>
      <td>226</td>
      <td>639</td>
      <td>642</td>
    </tr>
  </tbody>
</table>
</div>


## B-2


```python
#B-2: Check for Type Consistency
df.info()
df.sample(5)

print('''As can be seen from the sample and info below:
gender is object Dtype as it should be. Therefore it is consistent
race/ethnicity is object Dtype as it should be. Therefore it is consistent
parental level of education is object Dtype as it should be. Therefore it is consistent
lunch is object Dtype as it should be. (But has some NaN values)
test preparation course level of education is object Dtype as it should be. Therefore it is consistent
math score is numerical as it should be. (But has some NaN values and other issues)
reading score is numerical as it should be. (But has some NaN values and other issues)
writing score is numerical as it should be. (But has some NaN values and other issues)
gk score is numerical as it should be. (But has some NaN values and other issues)''')
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1000 entries, 0 to 999
    Data columns (total 9 columns):
     #   Column                       Non-Null Count  Dtype  
    ---  ------                       --------------  -----  
     0   gender                       1000 non-null   object 
     1   race/ethnicity               1000 non-null   object 
     2   parental level of education  1000 non-null   object 
     3   lunch                        991 non-null    object 
     4   test preparation course      1000 non-null   object 
     5   math score                   964 non-null    float64
     6   reading score                922 non-null    float64
     7   writing score                961 non-null    float64
     8   gk score                     887 non-null    float64
    dtypes: float64(4), object(5)
    memory usage: 70.4+ KB
    As can be seen from the sample and info below:
    gender is object Dtype as it should be. Therefore it is consistent
    race/ethnicity is object Dtype as it should be. Therefore it is consistent
    parental level of education is object Dtype as it should be. Therefore it is consistent
    lunch is object Dtype as it should be. (But has some NaN values)
    test preparation course level of education is object Dtype as it should be. Therefore it is consistent
    math score is numerical as it should be. (But has some NaN values and other issues)
    reading score is numerical as it should be. (But has some NaN values and other issues)
    writing score is numerical as it should be. (But has some NaN values and other issues)
    gk score is numerical as it should be. (But has some NaN values and other issues)
    

## B-3


```python
#B-3: Handling Inconsistent Data:
selected_columns1 = df.select_dtypes(include="number").columns[df.select_dtypes(include="number").isna().any()]
print(selected_columns1) #checking the selected columns
for c in selected_columns1:
    df[c] = df[c].apply(lambda x: np.abs(x) if x < 0 else x) # negative signs
# Scaling
df["math score"] = df["math score"].apply(lambda x: x if x <= 30 else (x/100) *30)
df["reading score"] = df["reading score"].apply(lambda x: x if x <= 25 else (x/100) *25)
df["writing score"] = df["writing score"].apply(lambda x: x if x <= 40 else (x/100) *40)
df
```

    Index(['math score', 'reading score', 'writing score', 'gk score'], dtype='object')
    





<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gender</th>
      <th>race/ethnicity</th>
      <th>parental level of education</th>
      <th>lunch</th>
      <th>test preparation course</th>
      <th>math score</th>
      <th>reading score</th>
      <th>writing score</th>
      <th>gk score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>female</td>
      <td>group B</td>
      <td>bachelor's degree</td>
      <td>standard</td>
      <td>none</td>
      <td>21.6</td>
      <td>18.00</td>
      <td>29.6</td>
      <td>71.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>female</td>
      <td>group C</td>
      <td>some college</td>
      <td>standard</td>
      <td>completed</td>
      <td>20.7</td>
      <td>22.50</td>
      <td>35.2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>female</td>
      <td>group B</td>
      <td>master's degree</td>
      <td>standard</td>
      <td>none</td>
      <td>27.0</td>
      <td>23.75</td>
      <td>37.2</td>
      <td>93.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>male</td>
      <td>group A</td>
      <td>associate's degree</td>
      <td>free/reduced</td>
      <td>none</td>
      <td>14.1</td>
      <td>NaN</td>
      <td>17.6</td>
      <td>56.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>male</td>
      <td>group C</td>
      <td>some college</td>
      <td>standard</td>
      <td>none</td>
      <td>22.8</td>
      <td>19.50</td>
      <td>30.0</td>
      <td>77.0</td>
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
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>995</th>
      <td>female</td>
      <td>group E</td>
      <td>master's degree</td>
      <td>standard</td>
      <td>completed</td>
      <td>26.4</td>
      <td>24.75</td>
      <td>38.0</td>
      <td>93.0</td>
    </tr>
    <tr>
      <th>996</th>
      <td>male</td>
      <td>group C</td>
      <td>high school</td>
      <td>free/reduced</td>
      <td>none</td>
      <td>18.6</td>
      <td>13.75</td>
      <td>22.0</td>
      <td>57.0</td>
    </tr>
    <tr>
      <th>997</th>
      <td>female</td>
      <td>group C</td>
      <td>high school</td>
      <td>free/reduced</td>
      <td>completed</td>
      <td>17.7</td>
      <td>17.75</td>
      <td>26.0</td>
      <td>63.0</td>
    </tr>
    <tr>
      <th>998</th>
      <td>female</td>
      <td>group D</td>
      <td>some college</td>
      <td>standard</td>
      <td>completed</td>
      <td>20.4</td>
      <td>19.50</td>
      <td>30.8</td>
      <td>75.0</td>
    </tr>
    <tr>
      <th>999</th>
      <td>female</td>
      <td>group D</td>
      <td>some college</td>
      <td>free/reduced</td>
      <td>none</td>
      <td>23.1</td>
      <td>21.50</td>
      <td>34.4</td>
      <td>85.0</td>
    </tr>
  </tbody>
</table>
<p>1000 rows × 9 columns</p>
</div>



## B-4


```python
#B-4: Handling NaN values 
#(by taking the weighted average of the scores): 
math_average = 1/3*((df["reading score"] / 25) + (df["writing score"]/40) + (df["gk score"]/100)) * 100 
df["math score"].fillna(np.round(math_average,2),inplace=True)

reading_average = 1/3*((df["math score"] / 30) + (df["writing score"]/40) + (df["gk score"]/100)) * 100 
df["reading score"].fillna(np.round(reading_average,2),inplace=True)

writing_average = 1/3*((df["reading score"] / 25) + (df["math score"] / 30) + (df["gk score"]/100)) * 100
df["writing score"].fillna(np.round(writing_average,2),inplace=True)

gk_average = 1/3*((df["reading score"] / 25) + (df["writing score"]/40) + (df["math score"] / 30)) * 100
df["gk score"].fillna(np.round(gk_average,2),inplace=True)
df
```





<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gender</th>
      <th>race/ethnicity</th>
      <th>parental level of education</th>
      <th>lunch</th>
      <th>test preparation course</th>
      <th>math score</th>
      <th>reading score</th>
      <th>writing score</th>
      <th>gk score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>female</td>
      <td>group B</td>
      <td>bachelor's degree</td>
      <td>standard</td>
      <td>none</td>
      <td>21.6</td>
      <td>18.00</td>
      <td>29.6</td>
      <td>71.00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>female</td>
      <td>group C</td>
      <td>some college</td>
      <td>standard</td>
      <td>completed</td>
      <td>20.7</td>
      <td>22.50</td>
      <td>35.2</td>
      <td>82.33</td>
    </tr>
    <tr>
      <th>2</th>
      <td>female</td>
      <td>group B</td>
      <td>master's degree</td>
      <td>standard</td>
      <td>none</td>
      <td>27.0</td>
      <td>23.75</td>
      <td>37.2</td>
      <td>93.00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>male</td>
      <td>group A</td>
      <td>associate's degree</td>
      <td>free/reduced</td>
      <td>none</td>
      <td>14.1</td>
      <td>49.00</td>
      <td>17.6</td>
      <td>56.00</td>
    </tr>
    <tr>
      <th>4</th>
      <td>male</td>
      <td>group C</td>
      <td>some college</td>
      <td>standard</td>
      <td>none</td>
      <td>22.8</td>
      <td>19.50</td>
      <td>30.0</td>
      <td>77.00</td>
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
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>995</th>
      <td>female</td>
      <td>group E</td>
      <td>master's degree</td>
      <td>standard</td>
      <td>completed</td>
      <td>26.4</td>
      <td>24.75</td>
      <td>38.0</td>
      <td>93.00</td>
    </tr>
    <tr>
      <th>996</th>
      <td>male</td>
      <td>group C</td>
      <td>high school</td>
      <td>free/reduced</td>
      <td>none</td>
      <td>18.6</td>
      <td>13.75</td>
      <td>22.0</td>
      <td>57.00</td>
    </tr>
    <tr>
      <th>997</th>
      <td>female</td>
      <td>group C</td>
      <td>high school</td>
      <td>free/reduced</td>
      <td>completed</td>
      <td>17.7</td>
      <td>17.75</td>
      <td>26.0</td>
      <td>63.00</td>
    </tr>
    <tr>
      <th>998</th>
      <td>female</td>
      <td>group D</td>
      <td>some college</td>
      <td>standard</td>
      <td>completed</td>
      <td>20.4</td>
      <td>19.50</td>
      <td>30.8</td>
      <td>75.00</td>
    </tr>
    <tr>
      <th>999</th>
      <td>female</td>
      <td>group D</td>
      <td>some college</td>
      <td>free/reduced</td>
      <td>none</td>
      <td>23.1</td>
      <td>21.50</td>
      <td>34.4</td>
      <td>85.00</td>
    </tr>
  </tbody>
</table>
<p>1000 rows × 9 columns</p>
</div>



## B-5


```python
#B-5: Replacing NaN values
selected_columns2 = df.select_dtypes(exclude="number").columns[df.select_dtypes(exclude="number").isna().any()]
print(selected_columns2) # checking the selected columns 
df["lunch"].fillna(df["race/ethnicity"].mode()[0],inplace=True)
df
```

    Index(['lunch'], dtype='object')
    





<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gender</th>
      <th>race/ethnicity</th>
      <th>parental level of education</th>
      <th>lunch</th>
      <th>test preparation course</th>
      <th>math score</th>
      <th>reading score</th>
      <th>writing score</th>
      <th>gk score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>female</td>
      <td>group B</td>
      <td>bachelor's degree</td>
      <td>standard</td>
      <td>none</td>
      <td>21.6</td>
      <td>18.00</td>
      <td>29.6</td>
      <td>71.00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>female</td>
      <td>group C</td>
      <td>some college</td>
      <td>standard</td>
      <td>completed</td>
      <td>20.7</td>
      <td>22.50</td>
      <td>35.2</td>
      <td>82.33</td>
    </tr>
    <tr>
      <th>2</th>
      <td>female</td>
      <td>group B</td>
      <td>master's degree</td>
      <td>standard</td>
      <td>none</td>
      <td>27.0</td>
      <td>23.75</td>
      <td>37.2</td>
      <td>93.00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>male</td>
      <td>group A</td>
      <td>associate's degree</td>
      <td>free/reduced</td>
      <td>none</td>
      <td>14.1</td>
      <td>49.00</td>
      <td>17.6</td>
      <td>56.00</td>
    </tr>
    <tr>
      <th>4</th>
      <td>male</td>
      <td>group C</td>
      <td>some college</td>
      <td>standard</td>
      <td>none</td>
      <td>22.8</td>
      <td>19.50</td>
      <td>30.0</td>
      <td>77.00</td>
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
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>995</th>
      <td>female</td>
      <td>group E</td>
      <td>master's degree</td>
      <td>standard</td>
      <td>completed</td>
      <td>26.4</td>
      <td>24.75</td>
      <td>38.0</td>
      <td>93.00</td>
    </tr>
    <tr>
      <th>996</th>
      <td>male</td>
      <td>group C</td>
      <td>high school</td>
      <td>free/reduced</td>
      <td>none</td>
      <td>18.6</td>
      <td>13.75</td>
      <td>22.0</td>
      <td>57.00</td>
    </tr>
    <tr>
      <th>997</th>
      <td>female</td>
      <td>group C</td>
      <td>high school</td>
      <td>free/reduced</td>
      <td>completed</td>
      <td>17.7</td>
      <td>17.75</td>
      <td>26.0</td>
      <td>63.00</td>
    </tr>
    <tr>
      <th>998</th>
      <td>female</td>
      <td>group D</td>
      <td>some college</td>
      <td>standard</td>
      <td>completed</td>
      <td>20.4</td>
      <td>19.50</td>
      <td>30.8</td>
      <td>75.00</td>
    </tr>
    <tr>
      <th>999</th>
      <td>female</td>
      <td>group D</td>
      <td>some college</td>
      <td>free/reduced</td>
      <td>none</td>
      <td>23.1</td>
      <td>21.50</td>
      <td>34.4</td>
      <td>85.00</td>
    </tr>
  </tbody>
</table>
<p>1000 rows × 9 columns</p>
</div>



## B-6


```python
#B-6: Label Encoding
from sklearn.preprocessing import LabelEncoder
encoder = LabelEncoder()
encoder.fit(df['parental level of education']) 
df['parental level of education']=encoder.transform(df['parental level of education'])
print(f'The new values are', df['parental level of education'].unique())
df
```

    The new values are [1 4 3 0 2 5]
    





<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gender</th>
      <th>race/ethnicity</th>
      <th>parental level of education</th>
      <th>lunch</th>
      <th>test preparation course</th>
      <th>math score</th>
      <th>reading score</th>
      <th>writing score</th>
      <th>gk score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>female</td>
      <td>group B</td>
      <td>1</td>
      <td>standard</td>
      <td>none</td>
      <td>21.6</td>
      <td>18.00</td>
      <td>29.6</td>
      <td>71.00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>female</td>
      <td>group C</td>
      <td>4</td>
      <td>standard</td>
      <td>completed</td>
      <td>20.7</td>
      <td>22.50</td>
      <td>35.2</td>
      <td>82.33</td>
    </tr>
    <tr>
      <th>2</th>
      <td>female</td>
      <td>group B</td>
      <td>3</td>
      <td>standard</td>
      <td>none</td>
      <td>27.0</td>
      <td>23.75</td>
      <td>37.2</td>
      <td>93.00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>male</td>
      <td>group A</td>
      <td>0</td>
      <td>free/reduced</td>
      <td>none</td>
      <td>14.1</td>
      <td>49.00</td>
      <td>17.6</td>
      <td>56.00</td>
    </tr>
    <tr>
      <th>4</th>
      <td>male</td>
      <td>group C</td>
      <td>4</td>
      <td>standard</td>
      <td>none</td>
      <td>22.8</td>
      <td>19.50</td>
      <td>30.0</td>
      <td>77.00</td>
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
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>995</th>
      <td>female</td>
      <td>group E</td>
      <td>3</td>
      <td>standard</td>
      <td>completed</td>
      <td>26.4</td>
      <td>24.75</td>
      <td>38.0</td>
      <td>93.00</td>
    </tr>
    <tr>
      <th>996</th>
      <td>male</td>
      <td>group C</td>
      <td>2</td>
      <td>free/reduced</td>
      <td>none</td>
      <td>18.6</td>
      <td>13.75</td>
      <td>22.0</td>
      <td>57.00</td>
    </tr>
    <tr>
      <th>997</th>
      <td>female</td>
      <td>group C</td>
      <td>2</td>
      <td>free/reduced</td>
      <td>completed</td>
      <td>17.7</td>
      <td>17.75</td>
      <td>26.0</td>
      <td>63.00</td>
    </tr>
    <tr>
      <th>998</th>
      <td>female</td>
      <td>group D</td>
      <td>4</td>
      <td>standard</td>
      <td>completed</td>
      <td>20.4</td>
      <td>19.50</td>
      <td>30.8</td>
      <td>75.00</td>
    </tr>
    <tr>
      <th>999</th>
      <td>female</td>
      <td>group D</td>
      <td>4</td>
      <td>free/reduced</td>
      <td>none</td>
      <td>23.1</td>
      <td>21.50</td>
      <td>34.4</td>
      <td>85.00</td>
    </tr>
  </tbody>
</table>
<p>1000 rows × 9 columns</p>
</div>



## B-7


```python
#B-7: One-Hot-Encoding
df = pd.get_dummies(df, columns=['test preparation course'],drop_first=True)
df
```





<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gender</th>
      <th>race/ethnicity</th>
      <th>parental level of education</th>
      <th>lunch</th>
      <th>math score</th>
      <th>reading score</th>
      <th>writing score</th>
      <th>gk score</th>
      <th>test preparation course_none</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>female</td>
      <td>group B</td>
      <td>1</td>
      <td>standard</td>
      <td>21.6</td>
      <td>18.00</td>
      <td>29.6</td>
      <td>71.00</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>female</td>
      <td>group C</td>
      <td>4</td>
      <td>standard</td>
      <td>20.7</td>
      <td>22.50</td>
      <td>35.2</td>
      <td>82.33</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>female</td>
      <td>group B</td>
      <td>3</td>
      <td>standard</td>
      <td>27.0</td>
      <td>23.75</td>
      <td>37.2</td>
      <td>93.00</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>male</td>
      <td>group A</td>
      <td>0</td>
      <td>free/reduced</td>
      <td>14.1</td>
      <td>49.00</td>
      <td>17.6</td>
      <td>56.00</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>male</td>
      <td>group C</td>
      <td>4</td>
      <td>standard</td>
      <td>22.8</td>
      <td>19.50</td>
      <td>30.0</td>
      <td>77.00</td>
      <td>1</td>
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
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>995</th>
      <td>female</td>
      <td>group E</td>
      <td>3</td>
      <td>standard</td>
      <td>26.4</td>
      <td>24.75</td>
      <td>38.0</td>
      <td>93.00</td>
      <td>0</td>
    </tr>
    <tr>
      <th>996</th>
      <td>male</td>
      <td>group C</td>
      <td>2</td>
      <td>free/reduced</td>
      <td>18.6</td>
      <td>13.75</td>
      <td>22.0</td>
      <td>57.00</td>
      <td>1</td>
    </tr>
    <tr>
      <th>997</th>
      <td>female</td>
      <td>group C</td>
      <td>2</td>
      <td>free/reduced</td>
      <td>17.7</td>
      <td>17.75</td>
      <td>26.0</td>
      <td>63.00</td>
      <td>0</td>
    </tr>
    <tr>
      <th>998</th>
      <td>female</td>
      <td>group D</td>
      <td>4</td>
      <td>standard</td>
      <td>20.4</td>
      <td>19.50</td>
      <td>30.8</td>
      <td>75.00</td>
      <td>0</td>
    </tr>
    <tr>
      <th>999</th>
      <td>female</td>
      <td>group D</td>
      <td>4</td>
      <td>free/reduced</td>
      <td>23.1</td>
      <td>21.50</td>
      <td>34.4</td>
      <td>85.00</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>1000 rows × 9 columns</p>
</div>



## B-8


```python
#B-8: Normalization: scale all the scores between [0,1].
selected_columns3 = df.loc[:,["math score","reading score","writing score","gk score"]].columns
print(selected_columns3)
print()
from sklearn.preprocessing import MinMaxScaler
for c in selected_columns3:
    scaler = MinMaxScaler() # creating object
    scaler.fit(df[[c]]) # fitting
    df[c]= scaler.transform(df[[c]]) #transforming
    print(f'The max and min values for {c} are', df[c].max(), ' and ', df[c].min())
df
```

    Index(['math score', 'reading score', 'writing score', 'gk score'], dtype='object')
    
    The max and min values for math score are 1.0  and  0.0
    The max and min values for reading score are 0.9999999999999999  and  0.0
    The max and min values for writing score are 0.9999999999999999  and  0.0
    The max and min values for gk score are 1.0  and  0.0
    





<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gender</th>
      <th>race/ethnicity</th>
      <th>parental level of education</th>
      <th>lunch</th>
      <th>math score</th>
      <th>reading score</th>
      <th>writing score</th>
      <th>gk score</th>
      <th>test preparation course_none</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>female</td>
      <td>group B</td>
      <td>1</td>
      <td>standard</td>
      <td>0.241800</td>
      <td>0.146667</td>
      <td>0.266667</td>
      <td>0.659574</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>female</td>
      <td>group C</td>
      <td>4</td>
      <td>standard</td>
      <td>0.231725</td>
      <td>0.194667</td>
      <td>0.325000</td>
      <td>0.780106</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>female</td>
      <td>group B</td>
      <td>3</td>
      <td>standard</td>
      <td>0.302250</td>
      <td>0.208000</td>
      <td>0.345833</td>
      <td>0.893617</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>male</td>
      <td>group A</td>
      <td>0</td>
      <td>free/reduced</td>
      <td>0.157842</td>
      <td>0.477333</td>
      <td>0.141667</td>
      <td>0.500000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>male</td>
      <td>group C</td>
      <td>4</td>
      <td>standard</td>
      <td>0.255233</td>
      <td>0.162667</td>
      <td>0.270833</td>
      <td>0.723404</td>
      <td>1</td>
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
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>995</th>
      <td>female</td>
      <td>group E</td>
      <td>3</td>
      <td>standard</td>
      <td>0.295533</td>
      <td>0.218667</td>
      <td>0.354167</td>
      <td>0.893617</td>
      <td>0</td>
    </tr>
    <tr>
      <th>996</th>
      <td>male</td>
      <td>group C</td>
      <td>2</td>
      <td>free/reduced</td>
      <td>0.208217</td>
      <td>0.101333</td>
      <td>0.187500</td>
      <td>0.510638</td>
      <td>1</td>
    </tr>
    <tr>
      <th>997</th>
      <td>female</td>
      <td>group C</td>
      <td>2</td>
      <td>free/reduced</td>
      <td>0.198142</td>
      <td>0.144000</td>
      <td>0.229167</td>
      <td>0.574468</td>
      <td>0</td>
    </tr>
    <tr>
      <th>998</th>
      <td>female</td>
      <td>group D</td>
      <td>4</td>
      <td>standard</td>
      <td>0.228367</td>
      <td>0.162667</td>
      <td>0.279167</td>
      <td>0.702128</td>
      <td>0</td>
    </tr>
    <tr>
      <th>999</th>
      <td>female</td>
      <td>group D</td>
      <td>4</td>
      <td>free/reduced</td>
      <td>0.258592</td>
      <td>0.184000</td>
      <td>0.316667</td>
      <td>0.808511</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>1000 rows × 9 columns</p>
</div>



## B-9


```python
#B-9: Display the Clean & Prepared Data.
display(df)
display(df.info())
display(df.describe(include="number"))# only numerical
display(df.describe(include="object")) # only categorical
```



<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gender</th>
      <th>race/ethnicity</th>
      <th>parental level of education</th>
      <th>lunch</th>
      <th>math score</th>
      <th>reading score</th>
      <th>writing score</th>
      <th>gk score</th>
      <th>test preparation course_none</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>female</td>
      <td>group B</td>
      <td>1</td>
      <td>standard</td>
      <td>0.241800</td>
      <td>0.146667</td>
      <td>0.266667</td>
      <td>0.659574</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>female</td>
      <td>group C</td>
      <td>4</td>
      <td>standard</td>
      <td>0.231725</td>
      <td>0.194667</td>
      <td>0.325000</td>
      <td>0.780106</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>female</td>
      <td>group B</td>
      <td>3</td>
      <td>standard</td>
      <td>0.302250</td>
      <td>0.208000</td>
      <td>0.345833</td>
      <td>0.893617</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>male</td>
      <td>group A</td>
      <td>0</td>
      <td>free/reduced</td>
      <td>0.157842</td>
      <td>0.477333</td>
      <td>0.141667</td>
      <td>0.500000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>male</td>
      <td>group C</td>
      <td>4</td>
      <td>standard</td>
      <td>0.255233</td>
      <td>0.162667</td>
      <td>0.270833</td>
      <td>0.723404</td>
      <td>1</td>
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
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>995</th>
      <td>female</td>
      <td>group E</td>
      <td>3</td>
      <td>standard</td>
      <td>0.295533</td>
      <td>0.218667</td>
      <td>0.354167</td>
      <td>0.893617</td>
      <td>0</td>
    </tr>
    <tr>
      <th>996</th>
      <td>male</td>
      <td>group C</td>
      <td>2</td>
      <td>free/reduced</td>
      <td>0.208217</td>
      <td>0.101333</td>
      <td>0.187500</td>
      <td>0.510638</td>
      <td>1</td>
    </tr>
    <tr>
      <th>997</th>
      <td>female</td>
      <td>group C</td>
      <td>2</td>
      <td>free/reduced</td>
      <td>0.198142</td>
      <td>0.144000</td>
      <td>0.229167</td>
      <td>0.574468</td>
      <td>0</td>
    </tr>
    <tr>
      <th>998</th>
      <td>female</td>
      <td>group D</td>
      <td>4</td>
      <td>standard</td>
      <td>0.228367</td>
      <td>0.162667</td>
      <td>0.279167</td>
      <td>0.702128</td>
      <td>0</td>
    </tr>
    <tr>
      <th>999</th>
      <td>female</td>
      <td>group D</td>
      <td>4</td>
      <td>free/reduced</td>
      <td>0.258592</td>
      <td>0.184000</td>
      <td>0.316667</td>
      <td>0.808511</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>1000 rows × 9 columns</p>
</div>


    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1000 entries, 0 to 999
    Data columns (total 9 columns):
     #   Column                        Non-Null Count  Dtype  
    ---  ------                        --------------  -----  
     0   gender                        1000 non-null   object 
     1   race/ethnicity                1000 non-null   object 
     2   parental level of education   1000 non-null   int32  
     3   lunch                         1000 non-null   object 
     4   math score                    1000 non-null   float64
     5   reading score                 1000 non-null   float64
     6   writing score                 1000 non-null   float64
     7   gk score                      1000 non-null   float64
     8   test preparation course_none  1000 non-null   uint8  
    dtypes: float64(4), int32(1), object(3), uint8(1)
    memory usage: 59.7+ KB
    


    None




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>parental level of education</th>
      <th>math score</th>
      <th>reading score</th>
      <th>writing score</th>
      <th>gk score</th>
      <th>test preparation course_none</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1000.000000</td>
      <td>1000.000000</td>
      <td>1000.000000</td>
      <td>1000.000000</td>
      <td>1000.000000</td>
      <td>1000.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2.486000</td>
      <td>0.241875</td>
      <td>0.180360</td>
      <td>0.260529</td>
      <td>0.625131</td>
      <td>0.642000</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1.829522</td>
      <td>0.119786</td>
      <td>0.152368</td>
      <td>0.118475</td>
      <td>0.154279</td>
      <td>0.479652</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>1.000000</td>
      <td>0.191425</td>
      <td>0.114667</td>
      <td>0.200000</td>
      <td>0.521277</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2.000000</td>
      <td>0.225008</td>
      <td>0.146667</td>
      <td>0.250000</td>
      <td>0.627660</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>4.000000</td>
      <td>0.261950</td>
      <td>0.176000</td>
      <td>0.291667</td>
      <td>0.734043</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>5.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gender</th>
      <th>race/ethnicity</th>
      <th>lunch</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1000</td>
      <td>1000</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>2</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>top</th>
      <td>female</td>
      <td>group C</td>
      <td>standard</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>518</td>
      <td>319</td>
      <td>639</td>
    </tr>
  </tbody>
</table>
</div>




## 🛠 Skills Used
Python,  
Data cleaning: handling missing values, filtering noisy data, data wrangling, data manipulation, label encoding, one-hot encoding, and data normalization
## 🚀 About Me
👋 Hi, I’m @Raed-Alshehri

👀 I’m interested in data science, machine learning, and statistics.

🌱 I’m applying my skills in the data analytics field using Python, R, and SQL


## 🔗 Links
[![portfolio](https://img.shields.io/badge/my_portfolio-000?style=for-the-badge&logo=ko-fi&logoColor=white)](https://raed-alshehri.github.io/RaedAlshehri.github.io/)
[![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/raedalshehri/)


## Feedback

If you have any feedback, please reach out to me at alshehri.raeda@gmail.com

