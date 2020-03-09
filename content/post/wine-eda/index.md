---
authors:
- vincent 
date: "2019-12-07T00:00:00Z"
tags: ["EDA", "Linear-regression", "plot"]
categories: ["regression", "EDA", "Python"]
image: 
  caption: 'Image credit: [**Kim Ellis**](https://unsplash.com/photos/aF1NPSnDQLw)'
  focal_point: ""
  placement: 2
  preview_only: false
lastMod: "2019-12-07T00:00:00Z"
subtitle: Using `pandas`, `matplot`, and `seaborn` library.
summary: Learn how to do basic EDA in Python using Jupyter notebooks
title: Basic Exploratory Data Analysis and Simple Linear Regression in Python
---


# Import Library


```python
import math
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.linear_model import LinearRegression

# Visulaization options:
%matplotlib inline
```

# Read data

### Data obtained from UCI Machine Learning Repository & accessed via Kaggle (https://www.kaggle.com/uciml/red-wine-quality-cortez-et-al-2009)


```python
df = pd.read_csv("redwine.csv")
```


```python
df
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
      <th>fixed acidity</th>
      <th>volatile acidity</th>
      <th>citric acid</th>
      <th>residual sugar</th>
      <th>chlorides</th>
      <th>free sulfur dioxide</th>
      <th>total sulfur dioxide</th>
      <th>density</th>
      <th>pH</th>
      <th>sulphates</th>
      <th>alcohol</th>
      <th>quality</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>7.4</td>
      <td>0.700</td>
      <td>0.00</td>
      <td>1.9</td>
      <td>0.076</td>
      <td>11.0</td>
      <td>34.0</td>
      <td>0.99780</td>
      <td>3.51</td>
      <td>0.56</td>
      <td>9.4</td>
      <td>5</td>
    </tr>
    <tr>
      <td>1</td>
      <td>7.8</td>
      <td>0.880</td>
      <td>0.00</td>
      <td>2.6</td>
      <td>0.098</td>
      <td>25.0</td>
      <td>67.0</td>
      <td>0.99680</td>
      <td>3.20</td>
      <td>0.68</td>
      <td>9.8</td>
      <td>5</td>
    </tr>
    <tr>
      <td>2</td>
      <td>7.8</td>
      <td>0.760</td>
      <td>0.04</td>
      <td>2.3</td>
      <td>0.092</td>
      <td>15.0</td>
      <td>54.0</td>
      <td>0.99700</td>
      <td>3.26</td>
      <td>0.65</td>
      <td>9.8</td>
      <td>5</td>
    </tr>
    <tr>
      <td>3</td>
      <td>11.2</td>
      <td>0.280</td>
      <td>0.56</td>
      <td>1.9</td>
      <td>0.075</td>
      <td>17.0</td>
      <td>60.0</td>
      <td>0.99800</td>
      <td>3.16</td>
      <td>0.58</td>
      <td>9.8</td>
      <td>6</td>
    </tr>
    <tr>
      <td>4</td>
      <td>7.4</td>
      <td>0.700</td>
      <td>0.00</td>
      <td>1.9</td>
      <td>0.076</td>
      <td>11.0</td>
      <td>34.0</td>
      <td>0.99780</td>
      <td>3.51</td>
      <td>0.56</td>
      <td>9.4</td>
      <td>5</td>
    </tr>
    <tr>
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
      <td>...</td>
    </tr>
    <tr>
      <td>1594</td>
      <td>6.2</td>
      <td>0.600</td>
      <td>0.08</td>
      <td>2.0</td>
      <td>0.090</td>
      <td>32.0</td>
      <td>44.0</td>
      <td>0.99490</td>
      <td>3.45</td>
      <td>0.58</td>
      <td>10.5</td>
      <td>5</td>
    </tr>
    <tr>
      <td>1595</td>
      <td>5.9</td>
      <td>0.550</td>
      <td>0.10</td>
      <td>2.2</td>
      <td>0.062</td>
      <td>39.0</td>
      <td>51.0</td>
      <td>0.99512</td>
      <td>3.52</td>
      <td>0.76</td>
      <td>11.2</td>
      <td>6</td>
    </tr>
    <tr>
      <td>1596</td>
      <td>6.3</td>
      <td>0.510</td>
      <td>0.13</td>
      <td>2.3</td>
      <td>0.076</td>
      <td>29.0</td>
      <td>40.0</td>
      <td>0.99574</td>
      <td>3.42</td>
      <td>0.75</td>
      <td>11.0</td>
      <td>6</td>
    </tr>
    <tr>
      <td>1597</td>
      <td>5.9</td>
      <td>0.645</td>
      <td>0.12</td>
      <td>2.0</td>
      <td>0.075</td>
      <td>32.0</td>
      <td>44.0</td>
      <td>0.99547</td>
      <td>3.57</td>
      <td>0.71</td>
      <td>10.2</td>
      <td>5</td>
    </tr>
    <tr>
      <td>1598</td>
      <td>6.0</td>
      <td>0.310</td>
      <td>0.47</td>
      <td>3.6</td>
      <td>0.067</td>
      <td>18.0</td>
      <td>42.0</td>
      <td>0.99549</td>
      <td>3.39</td>
      <td>0.66</td>
      <td>11.0</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
<p>1599 rows × 12 columns</p>
</div>



## Data Description

Accessed via Kaggle
### About

The dataset is related to red variants of the Portuguese **"Vinho Verde"** wine. For more details, consult the reference [Cortez et al., 2009]. Due to privacy and logistic issues, only physicochemical (inputs) and sensory (the output) variables are available (e.g. there is no data about grape types, wine brand, wine selling price, etc.).

### Input variables (based on physicochemical tests):

- `fixed acidity` : most acids involved with wine or fixed or nonvolatile (do not evaporate readily).
- `volatile acidity` : the amount of acetic acid in wine, which at too high of levels can lead to an unpleasant, vinegar taste.
- `citric acid` : found in small quantities, citric acid can add 'freshness' and flavor to wines.
- `residual sugar` : the amount of sugar remaining after fermentation stops, it's rare to find wines with less than 1 gram/liter and wines with greater than 45 grams/liter are considered sweet.
- `chlorides` : the amount of salt in the wine.
- `free sulfur dioxide` : the free form of SO2 exists in equilibrium between molecular SO2 (as a dissolved gas) and bisulfite ion; it prevents microbial growth and the oxidation of wine.
- `total sulfur dioxide` : amount of free and bound forms of S02; in low concentrations, SO2 is mostly undetectable in wine, but at free SO2 concentrations over 50 ppm, SO2 becomes evident in the nose and taste of wine.
- `density` : the density of water is close to that of water depending on the percent alcohol and sugar content.
- `pH` : describes how acidic or basic a wine is on a scale from 0 (very acidic) to 14 (very basic); most wines are between 3-4 on the pH scale.
- `sulphates` : a wine additive which can contribute to sulfur dioxide gas (S02) levels, which acts as an antimicrobial and antioxidant.
- `alcohol` : the percent alcohol content of the wine.

### Output variable (based on sensory data):

- `quality` : score between 0 and 10 given by human wine tasters.

## Exploratory Data Analysis (EDA)


```python
# print out dataframe dimension or shape (rows x columns)
df.shape
```




    (1599, 12)




```python
# print out information on the data
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1599 entries, 0 to 1598
    Data columns (total 12 columns):
    fixed acidity           1599 non-null float64
    volatile acidity        1599 non-null float64
    citric acid             1599 non-null float64
    residual sugar          1599 non-null float64
    chlorides               1599 non-null float64
    free sulfur dioxide     1599 non-null float64
    total sulfur dioxide    1599 non-null float64
    density                 1599 non-null float64
    pH                      1599 non-null float64
    sulphates               1599 non-null float64
    alcohol                 1599 non-null float64
    quality                 1599 non-null int64
    dtypes: float64(11), int64(1)
    memory usage: 150.0 KB



```python
# print out summary information about all numeric data columns in your dataset.
df.describe()
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
      <th>fixed acidity</th>
      <th>volatile acidity</th>
      <th>citric acid</th>
      <th>residual sugar</th>
      <th>chlorides</th>
      <th>free sulfur dioxide</th>
      <th>total sulfur dioxide</th>
      <th>density</th>
      <th>pH</th>
      <th>sulphates</th>
      <th>alcohol</th>
      <th>quality</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>count</td>
      <td>1599.000000</td>
      <td>1599.000000</td>
      <td>1599.000000</td>
      <td>1599.000000</td>
      <td>1599.000000</td>
      <td>1599.000000</td>
      <td>1599.000000</td>
      <td>1599.000000</td>
      <td>1599.000000</td>
      <td>1599.000000</td>
      <td>1599.000000</td>
      <td>1599.000000</td>
    </tr>
    <tr>
      <td>mean</td>
      <td>8.319637</td>
      <td>0.527821</td>
      <td>0.270976</td>
      <td>2.538806</td>
      <td>0.087467</td>
      <td>15.874922</td>
      <td>46.467792</td>
      <td>0.996747</td>
      <td>3.311113</td>
      <td>0.658149</td>
      <td>10.422983</td>
      <td>5.636023</td>
    </tr>
    <tr>
      <td>std</td>
      <td>1.741096</td>
      <td>0.179060</td>
      <td>0.194801</td>
      <td>1.409928</td>
      <td>0.047065</td>
      <td>10.460157</td>
      <td>32.895324</td>
      <td>0.001887</td>
      <td>0.154386</td>
      <td>0.169507</td>
      <td>1.065668</td>
      <td>0.807569</td>
    </tr>
    <tr>
      <td>min</td>
      <td>4.600000</td>
      <td>0.120000</td>
      <td>0.000000</td>
      <td>0.900000</td>
      <td>0.012000</td>
      <td>1.000000</td>
      <td>6.000000</td>
      <td>0.990070</td>
      <td>2.740000</td>
      <td>0.330000</td>
      <td>8.400000</td>
      <td>3.000000</td>
    </tr>
    <tr>
      <td>25%</td>
      <td>7.100000</td>
      <td>0.390000</td>
      <td>0.090000</td>
      <td>1.900000</td>
      <td>0.070000</td>
      <td>7.000000</td>
      <td>22.000000</td>
      <td>0.995600</td>
      <td>3.210000</td>
      <td>0.550000</td>
      <td>9.500000</td>
      <td>5.000000</td>
    </tr>
    <tr>
      <td>50%</td>
      <td>7.900000</td>
      <td>0.520000</td>
      <td>0.260000</td>
      <td>2.200000</td>
      <td>0.079000</td>
      <td>14.000000</td>
      <td>38.000000</td>
      <td>0.996750</td>
      <td>3.310000</td>
      <td>0.620000</td>
      <td>10.200000</td>
      <td>6.000000</td>
    </tr>
    <tr>
      <td>75%</td>
      <td>9.200000</td>
      <td>0.640000</td>
      <td>0.420000</td>
      <td>2.600000</td>
      <td>0.090000</td>
      <td>21.000000</td>
      <td>62.000000</td>
      <td>0.997835</td>
      <td>3.400000</td>
      <td>0.730000</td>
      <td>11.100000</td>
      <td>6.000000</td>
    </tr>
    <tr>
      <td>max</td>
      <td>15.900000</td>
      <td>1.580000</td>
      <td>1.000000</td>
      <td>15.500000</td>
      <td>0.611000</td>
      <td>72.000000</td>
      <td>289.000000</td>
      <td>1.003690</td>
      <td>4.010000</td>
      <td>2.000000</td>
      <td>14.900000</td>
      <td>8.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# print out the distribution on each column (variable) 
df.hist(bins = 50, edgecolor = 'b', grid = False,
                linewidth = 1.0,
                xlabelsize = 8, ylabelsize = 8,  
                figsize = (16, 6), color = 'orange')    
plt.tight_layout(rect = (0, 0, 1.5, 1.5))   
plt.suptitle('Red Wine Plots', x = 0.75, y = 1.65, fontsize = 20);  
```


![This is an image](/img/image1.png)



```python
# print out histogram of the quality variable 
df['quality'].hist(bins = 6, grid = False, color = 'red', edgecolor = 'b')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a1f4fc438>




![This is an image](/img/image2.png)



```python
# print out the correlation matrix (for each column)
df.corr()
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
      <th>fixed acidity</th>
      <th>volatile acidity</th>
      <th>citric acid</th>
      <th>residual sugar</th>
      <th>chlorides</th>
      <th>free sulfur dioxide</th>
      <th>total sulfur dioxide</th>
      <th>density</th>
      <th>pH</th>
      <th>sulphates</th>
      <th>alcohol</th>
      <th>quality</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>fixed acidity</td>
      <td>1.000000</td>
      <td>-0.256131</td>
      <td>0.671703</td>
      <td>0.114777</td>
      <td>0.093705</td>
      <td>-0.153794</td>
      <td>-0.113181</td>
      <td>0.668047</td>
      <td>-0.682978</td>
      <td>0.183006</td>
      <td>-0.061668</td>
      <td>0.124052</td>
    </tr>
    <tr>
      <td>volatile acidity</td>
      <td>-0.256131</td>
      <td>1.000000</td>
      <td>-0.552496</td>
      <td>0.001918</td>
      <td>0.061298</td>
      <td>-0.010504</td>
      <td>0.076470</td>
      <td>0.022026</td>
      <td>0.234937</td>
      <td>-0.260987</td>
      <td>-0.202288</td>
      <td>-0.390558</td>
    </tr>
    <tr>
      <td>citric acid</td>
      <td>0.671703</td>
      <td>-0.552496</td>
      <td>1.000000</td>
      <td>0.143577</td>
      <td>0.203823</td>
      <td>-0.060978</td>
      <td>0.035533</td>
      <td>0.364947</td>
      <td>-0.541904</td>
      <td>0.312770</td>
      <td>0.109903</td>
      <td>0.226373</td>
    </tr>
    <tr>
      <td>residual sugar</td>
      <td>0.114777</td>
      <td>0.001918</td>
      <td>0.143577</td>
      <td>1.000000</td>
      <td>0.055610</td>
      <td>0.187049</td>
      <td>0.203028</td>
      <td>0.355283</td>
      <td>-0.085652</td>
      <td>0.005527</td>
      <td>0.042075</td>
      <td>0.013732</td>
    </tr>
    <tr>
      <td>chlorides</td>
      <td>0.093705</td>
      <td>0.061298</td>
      <td>0.203823</td>
      <td>0.055610</td>
      <td>1.000000</td>
      <td>0.005562</td>
      <td>0.047400</td>
      <td>0.200632</td>
      <td>-0.265026</td>
      <td>0.371260</td>
      <td>-0.221141</td>
      <td>-0.128907</td>
    </tr>
    <tr>
      <td>free sulfur dioxide</td>
      <td>-0.153794</td>
      <td>-0.010504</td>
      <td>-0.060978</td>
      <td>0.187049</td>
      <td>0.005562</td>
      <td>1.000000</td>
      <td>0.667666</td>
      <td>-0.021946</td>
      <td>0.070377</td>
      <td>0.051658</td>
      <td>-0.069408</td>
      <td>-0.050656</td>
    </tr>
    <tr>
      <td>total sulfur dioxide</td>
      <td>-0.113181</td>
      <td>0.076470</td>
      <td>0.035533</td>
      <td>0.203028</td>
      <td>0.047400</td>
      <td>0.667666</td>
      <td>1.000000</td>
      <td>0.071269</td>
      <td>-0.066495</td>
      <td>0.042947</td>
      <td>-0.205654</td>
      <td>-0.185100</td>
    </tr>
    <tr>
      <td>density</td>
      <td>0.668047</td>
      <td>0.022026</td>
      <td>0.364947</td>
      <td>0.355283</td>
      <td>0.200632</td>
      <td>-0.021946</td>
      <td>0.071269</td>
      <td>1.000000</td>
      <td>-0.341699</td>
      <td>0.148506</td>
      <td>-0.496180</td>
      <td>-0.174919</td>
    </tr>
    <tr>
      <td>pH</td>
      <td>-0.682978</td>
      <td>0.234937</td>
      <td>-0.541904</td>
      <td>-0.085652</td>
      <td>-0.265026</td>
      <td>0.070377</td>
      <td>-0.066495</td>
      <td>-0.341699</td>
      <td>1.000000</td>
      <td>-0.196648</td>
      <td>0.205633</td>
      <td>-0.057731</td>
    </tr>
    <tr>
      <td>sulphates</td>
      <td>0.183006</td>
      <td>-0.260987</td>
      <td>0.312770</td>
      <td>0.005527</td>
      <td>0.371260</td>
      <td>0.051658</td>
      <td>0.042947</td>
      <td>0.148506</td>
      <td>-0.196648</td>
      <td>1.000000</td>
      <td>0.093595</td>
      <td>0.251397</td>
    </tr>
    <tr>
      <td>alcohol</td>
      <td>-0.061668</td>
      <td>-0.202288</td>
      <td>0.109903</td>
      <td>0.042075</td>
      <td>-0.221141</td>
      <td>-0.069408</td>
      <td>-0.205654</td>
      <td>-0.496180</td>
      <td>0.205633</td>
      <td>0.093595</td>
      <td>1.000000</td>
      <td>0.476166</td>
    </tr>
    <tr>
      <td>quality</td>
      <td>0.124052</td>
      <td>-0.390558</td>
      <td>0.226373</td>
      <td>0.013732</td>
      <td>-0.128907</td>
      <td>-0.050656</td>
      <td>-0.185100</td>
      <td>-0.174919</td>
      <td>-0.057731</td>
      <td>0.251397</td>
      <td>0.476166</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# print out correlation heatmap using 'seaborn' library
sns.heatmap(df.corr(), cmap = "YlGnBu")
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a1fad1400>




![This is an image](/img/image3.png)


# Modeling

## Using Linear Regression


```python
# Create a linear regression model:
model = LinearRegression()

# Train ("fit") the model:
model = model.fit(df[ ['fixed acidity', 'volatile acidity', 'citric acid', 'residual sugar', 'chlorides', 'free sulfur dioxide', 'total sulfur dioxide', 'density', 'pH', 'sulphates','alcohol'] ], df['quality'] )
```


```python
# print out the intercept:
intercept = model.intercept_
intercept

```




    21.965208449448177




```python
# print out the slope (as table):
slope = model.coef_

coeff_df = pd.DataFrame(slope, ['fixed acidity', 'volatile acidity', 'citric acid', 'residual sugar', 'chlorides', 'free sulfur dioxide', 'total sulfur dioxide', 'density', 'pH', 'sulphates','alcohol']  , columns = ['Coefficient'])  
coeff_df
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
      <th>Coefficient</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>fixed acidity</td>
      <td>0.024991</td>
    </tr>
    <tr>
      <td>volatile acidity</td>
      <td>-1.083590</td>
    </tr>
    <tr>
      <td>citric acid</td>
      <td>-0.182564</td>
    </tr>
    <tr>
      <td>residual sugar</td>
      <td>0.016331</td>
    </tr>
    <tr>
      <td>chlorides</td>
      <td>-1.874225</td>
    </tr>
    <tr>
      <td>free sulfur dioxide</td>
      <td>0.004361</td>
    </tr>
    <tr>
      <td>total sulfur dioxide</td>
      <td>-0.003265</td>
    </tr>
    <tr>
      <td>density</td>
      <td>-17.881164</td>
    </tr>
    <tr>
      <td>pH</td>
      <td>-0.413653</td>
    </tr>
    <tr>
      <td>sulphates</td>
      <td>0.916334</td>
    </tr>
    <tr>
      <td>alcohol</td>
      <td>0.276198</td>
    </tr>
  </tbody>
</table>
</div>




```python
# create prediction using our model
df["predicted"] = model.predict( df[ ['fixed acidity', 'volatile acidity', 'citric acid', 'residual sugar', 'chlorides', 'free sulfur dioxide', 'total sulfur dioxide', 'density', 'pH', 'sulphates','alcohol'] ] )

df["predicted"] = round(df["predicted"], 0)
df["predicted"]
```




    0       5.0
    1       5.0
    2       5.0
    3       6.0
    4       5.0
           ... 
    1594    6.0
    1595    6.0
    1596    6.0
    1597    5.0
    1598    6.0
    Name: predicted, Length: 1599, dtype: float64



# Results

- ### Relationship of `pH` and `fixed acidity` 

    - From the correlation matrix (in EDA section), we can see that `pH` and `fixed acidity` have the highest correlation with the value of **-0.682978**. 


```python
# Create a scatter plot between 'pH' (x-axis) and 'fixed acidity'(y-axis). 
df.plot.scatter(x = 'pH', y = 'fixed acidity')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a2009fb00>




![This is an image](/img/image4.png)


This negative correlation might be obvious and understandable since
- pH is the measure of acidity/basicity with a scale between 0 (very acid) and 14 (very basic) # information taken from chemistry class
- so the more acidic is a solution, pH value will decrease.
- as indicated in the data description, most wines are acidic and have pH values of 3-4 # in this data (lowest = 2.74 and highest = 4.01) obtained from EDA section.

**Therefore, we can conclude that there is a causation between `pH` and `fixed acidity`.**

- ### Relationship of `quality` and `alcohol`
    - From the correlation matrix (in EDA section), we found out that `alcohol` has the highest correlation with our target or response variable `quality` with a value of **0.476166**.


```python
# visualization using 'seaborn' library for scatter plot between 'alcohol' and 'quality'
sns.set()
sns.relplot(data = df, x = 'alcohol', y = 'quality', kind = 'line', height = 6, aspect = 2, color = 'red');    
```


![This is an image](/img/image5.jpg)


- The plot above clearly reflects the positive correlation between `quality` and `alcohol`. Where an increase in the alcohol level (**<** 14) **might** result in a better wine quality.

- One important thing to mention is:
    - **This might not be necessarily true** since there are cases where a higher quality level might result in lower wine quality. (in this dataset, for instance, a wine with 9% alcohol level has a lower quality than wine with 8% alcohol level).
    - **While there is a positive correlation between `quality` and `alcohol` their relationship does not indicate causality.**

# Discussion 

The predictive ability of our model is very **low** with an accuracy of only **59.16%**. This means our model does a really bad job on predicting the wine quality. 
- This bad results (predictive power) might be due to:
    - Limited predictor variable; in this dataset, we are only given variables that are based on physicochemical tests (lab tests such as alcohol percentage level, pH value, etc.).
    - There are many predictor variables that might be more helpful in order to predict the wine quality such as `grape type`, `wine age`, `vineyard location`, and etc.



```python
# check our model accuracy
(df["predicted"] == df["quality"]).mean()
```




    0.5916197623514696



## Solutions and Recommendations:
- Obtain more data (predictor variables and samples) and do another analysis.

