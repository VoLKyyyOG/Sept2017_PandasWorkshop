
# Introduction to Pandas

This workshop will be focused on the exploration, cleaning and basic visualisation of a prepared set of sample data - the Canberra Climate Sensor Data. If you have not already downloaded this data from the [Github](https://github.com/resbaz/Sept2017_PandasWorkshop), please do so now, and place this data file in the same folder as this jupyter notebook. 

## Learning objectives

Thorughout this session we're going to be teaching you a range of tools and skills related to cleaning, manipulation and visualising large datasets. Using the pandas package, we can read in and manipulate large spreadsheets of data, and matplotlib lets you visualise these datasets in a useable, customisable format.

The three sections I'll be taking you through today are:

- Data examination
- Dataframe manipulation
- Plotting your data with pyplot


## Setting Up

First, we need to import Python's *pandas*, *matplotlib* and *numpy* packages, and then use inline plotting "magic" command so that all plots generated will appear within this notebook instead of in a new browser tab.

While numpy isn't directly related to this course, it's handy for generating random values, which will be useful when learning how to create your own dataframe


```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
matplotlib.style.use('ggplot')
```


```python
%pylab inline
```

    Populating the interactive namespace from numpy and matplotlib
    

## A Basic Pandas Introduction

- creating your own dataframe
    - the "series" object
- subsetting columns
- subsetting rows
    - `head()` and `tail()`
    - slicing
    - `loc` vs `iloc`


### Creating a Dataframe

A Pandas dataframe can be thought of as a collection of lists (of equal length), where each list makes up a column inside your dateframe. 

The key difference between a list and a Pandas column though, is that every single item inside your column _must be of the same data type_. If you have a column of integers, and a single string value, like this, `[1,2,3,4,'seven']`, then every single value inside your column is going to be a string-type.

Instead of using a list - which can take any type of values -, Pandas performs this type-coercion by using a data type called a Series.


```python
pd.Series([1,2,3,4,5])
```




    0    1
    1    2
    2    3
    3    4
    4    5
    dtype: int64



Each "Series" object can be thought of as it's own miniature dataframe. So our previous example would be a dataframe with one column, and 5 rows.

Therefore, when creating a dataframe, you actually have to create it as a collection of these "Series" objects


```python
df = pd.DataFrame(
         {'A':['a', 'b', 'c','d','e','f'], 
             'B':[54, 67, 89, 100, None, 64],
             'C': np.random.randn(6),
             'D': [2.6,None, 8.0, 9.4, 3.3, None]
                },
          index=[49, 48, 47, 1, 2, 3] # Lets you set your row names
            )
```


```python
df
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>49</th>
      <td>a</td>
      <td>54.0</td>
      <td>1.118139</td>
      <td>2.6</td>
    </tr>
    <tr>
      <th>48</th>
      <td>b</td>
      <td>67.0</td>
      <td>-1.293534</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>47</th>
      <td>c</td>
      <td>89.0</td>
      <td>-0.490262</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>d</td>
      <td>100.0</td>
      <td>0.905747</td>
      <td>9.4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>e</td>
      <td>NaN</td>
      <td>-0.869318</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>f</td>
      <td>64.0</td>
      <td>0.146700</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



## Working with Data

### Reading in Data

The first step to any data exploration and manipulation is to open your data within your program. We are going to do this using the **pandas** package, which reads in spread-sheet style data and converts them into *dataframes*. 

These dataframes work with rows and columns, like a spreadsheet, except that all data within a single column has to be the same data type. 

For example, imagine you had a spreadsheet containing two columns - "labels" and "numbers", and that the rows in the "labels" column contains either a text or number sequence. Because you cannot turn text into a number, every single row in that "labels" column would need to be a string (text) type. Similarly, if some (but not all) of the rows in the "numbers" column contained decimals, **all** of the rows within this column would need to be of a decimal (float) data type.

To read in a comma-separated file, or \*.csv, you can use the pandas function read_csv()


```python
weather_observations = pd.read_csv('Canberra_observations.csv')
```

You can also open a variety of other file types using the "reader" functions found in this [IO tools documentation](http://pandas.pydata.org/pandas-docs/version/0.20/io.html "Pandas IO tools"). 

This includes file types such as excel (\*.xlsx) files, and text (\*.txt) files. You can use the parameters within these functions to specify file or data attributes such as column separators, whether there's column/row names, and even specifying your own column names.


```python
weather_observations.head()
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
      <th>Date	Time	Wind dir	Wind spd	Wind gust	Tmp	Dew pt	Feels like	rh	Fire	Rain	Rain 10'	Pres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>01/01/2013\tWed 00:00 EDT\tWNW\t9\t9\t19.6\t0....</td>
    </tr>
    <tr>
      <th>1</th>
      <td>01/01/2013\tTue 23:30 EDT\tWNW\t-\t13\t20.7\t0...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>01/01/2013\tTue 23:00 EDT\tW\t9\t11\t22.8\t0.9...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>01/01/2013\tTue 22:30 EDT\tWNW\t11\t13\t22.6\t...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>01/01/2013\tTue 22:00 EDT\tWNW\t15\t19\t23.9\t...</td>
    </tr>
  </tbody>
</table>
</div>



Oops! Seems as though this file is tab separated, not comma separated. 

Fortunately, pandas.read_csv() has a range of keyword arguments to read in our data.

- `sep`: the type of separator between our columns
-  `header`: the row number to take as the start of the data. Useful if you have metadata attached at the beginning of your file
- `names`: You can also specify your column names. Takes a list of values. If your file contains no header, also use `header = none`. Otherwise, use `header = 0`.
* `parse_dates`: Treat one or more columns like dates.
* `dayfirst`: Use DD.MM.YYYY format, not month first.
* `infer_datetime_format`: Tell pandas to guess the date format.
- `na_values`: Specify values to be treated as empty.




```python
weather_observations = pd.read_csv('Canberra_observations.csv',
                                   sep='\t',
                                   na_values=['-'],
                                   parse_dates={'Datetime': ['Date', 'Time']},
                                   dayfirst=True, #redundant in this instance
                                   infer_datetime_format=True, #redundant in this instance
                                  )
# Display some entries
weather_observations.head()
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
      <th>Datetime</th>
      <th>Wind dir</th>
      <th>Wind spd</th>
      <th>Wind gust</th>
      <th>Tmp</th>
      <th>Dew pt</th>
      <th>Feels like</th>
      <th>rh</th>
      <th>Fire</th>
      <th>Rain</th>
      <th>Rain 10'</th>
      <th>Pres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2013-01-01 00:00:00</td>
      <td>WNW</td>
      <td>9.0</td>
      <td>9.0</td>
      <td>19.6</td>
      <td>0.1</td>
      <td>19.6</td>
      <td>27.0</td>
      <td>12.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2013-01-01 23:30:00</td>
      <td>WNW</td>
      <td>NaN</td>
      <td>13.0</td>
      <td>20.7</td>
      <td>0.7</td>
      <td>20.7</td>
      <td>26.0</td>
      <td>13.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2013-01-01 23:00:00</td>
      <td>W</td>
      <td>9.0</td>
      <td>11.0</td>
      <td>22.8</td>
      <td>0.9</td>
      <td>22.8</td>
      <td>23.0</td>
      <td>15.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2013-01-01 22:30:00</td>
      <td>WNW</td>
      <td>11.0</td>
      <td>13.0</td>
      <td>22.6</td>
      <td>-1.0</td>
      <td>22.6</td>
      <td>21.0</td>
      <td>17.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2013-01-01 22:00:00</td>
      <td>WNW</td>
      <td>15.0</td>
      <td>19.0</td>
      <td>23.9</td>
      <td>-2.2</td>
      <td>23.9</td>
      <td>18.0</td>
      <td>21.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.1</td>
    </tr>
  </tbody>
</table>
</div>



### Examining your Data

- `head()`, `tail()`
- `columns`
- `df.Column` vs `df['Column']`
- `dtypes`
- `describe()`
- `shape`; `shape[0]` vs. `shape[1]`
- `iloc` vs `loc`

One of the first steps in exploring your data is to see what it looks like, what data types are present, and how many rows/columns there are.


```python
weather_observations.tail()
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
      <th>Datetime</th>
      <th>Wind dir</th>
      <th>Wind spd</th>
      <th>Wind gust</th>
      <th>Tmp</th>
      <th>Dew pt</th>
      <th>Feels like</th>
      <th>rh</th>
      <th>Fire</th>
      <th>Rain</th>
      <th>Rain 10'</th>
      <th>Pres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>19913</th>
      <td>2013-12-31 01:37:00</td>
      <td>SE</td>
      <td>7.0</td>
      <td>9.0</td>
      <td>11.6</td>
      <td>10.0</td>
      <td>11.0</td>
      <td>90.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.4</td>
    </tr>
    <tr>
      <th>19914</th>
      <td>2013-12-31 01:30:00</td>
      <td>SE</td>
      <td>7.0</td>
      <td>7.0</td>
      <td>12.0</td>
      <td>10.2</td>
      <td>11.5</td>
      <td>89.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.3</td>
    </tr>
    <tr>
      <th>19915</th>
      <td>2013-12-31 01:00:00</td>
      <td>SE</td>
      <td>7.0</td>
      <td>7.0</td>
      <td>13.1</td>
      <td>11.2</td>
      <td>12.7</td>
      <td>88.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.3</td>
    </tr>
    <tr>
      <th>19916</th>
      <td>2013-12-31 00:30:00</td>
      <td>SE</td>
      <td>7.0</td>
      <td>9.0</td>
      <td>12.7</td>
      <td>10.4</td>
      <td>12.3</td>
      <td>86.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.6</td>
    </tr>
    <tr>
      <th>19917</th>
      <td>2013-12-31 00:00:00</td>
      <td>SE</td>
      <td>7.0</td>
      <td>9.0</td>
      <td>13.4</td>
      <td>10.6</td>
      <td>13.1</td>
      <td>83.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.7</td>
    </tr>
  </tbody>
</table>
</div>



You can also specify how many rows you want `head` and `tail` to return


```python
weather_observations.head(10)
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
      <th>Datetime</th>
      <th>Wind dir</th>
      <th>Wind spd</th>
      <th>Wind gust</th>
      <th>Tmp</th>
      <th>Dew pt</th>
      <th>Feels like</th>
      <th>rh</th>
      <th>Fire</th>
      <th>Rain</th>
      <th>Rain 10'</th>
      <th>Pres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2013-01-01 00:00:00</td>
      <td>WNW</td>
      <td>9.0</td>
      <td>9.0</td>
      <td>19.6</td>
      <td>0.1</td>
      <td>19.6</td>
      <td>27.0</td>
      <td>12.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2013-01-01 23:30:00</td>
      <td>WNW</td>
      <td>NaN</td>
      <td>13.0</td>
      <td>20.7</td>
      <td>0.7</td>
      <td>20.7</td>
      <td>26.0</td>
      <td>13.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2013-01-01 23:00:00</td>
      <td>W</td>
      <td>9.0</td>
      <td>11.0</td>
      <td>22.8</td>
      <td>0.9</td>
      <td>22.8</td>
      <td>23.0</td>
      <td>15.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2013-01-01 22:30:00</td>
      <td>WNW</td>
      <td>11.0</td>
      <td>13.0</td>
      <td>22.6</td>
      <td>-1.0</td>
      <td>22.6</td>
      <td>21.0</td>
      <td>17.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2013-01-01 22:00:00</td>
      <td>WNW</td>
      <td>15.0</td>
      <td>19.0</td>
      <td>23.9</td>
      <td>-2.2</td>
      <td>23.9</td>
      <td>18.0</td>
      <td>21.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2013-01-01 21:30:00</td>
      <td>W</td>
      <td>13.0</td>
      <td>17.0</td>
      <td>24.0</td>
      <td>-2.0</td>
      <td>24.0</td>
      <td>18.0</td>
      <td>20.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2013-01-01 21:00:00</td>
      <td>WNW</td>
      <td>15.0</td>
      <td>22.0</td>
      <td>25.8</td>
      <td>-4.4</td>
      <td>25.0</td>
      <td>13.0</td>
      <td>27.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1010.8</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2013-01-01 20:30:00</td>
      <td>W</td>
      <td>22.0</td>
      <td>33.0</td>
      <td>27.3</td>
      <td>-6.4</td>
      <td>26.0</td>
      <td>10.0</td>
      <td>37.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1010.6</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2013-01-01 20:00:00</td>
      <td>W</td>
      <td>20.0</td>
      <td>28.0</td>
      <td>27.7</td>
      <td>-3.5</td>
      <td>26.0</td>
      <td>13.0</td>
      <td>33.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1010.4</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2013-01-01 19:30:00</td>
      <td>W</td>
      <td>24.0</td>
      <td>35.0</td>
      <td>28.9</td>
      <td>-5.2</td>
      <td>27.0</td>
      <td>10.0</td>
      <td>41.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1010.2</td>
    </tr>
  </tbody>
</table>
</div>



The `shape` function gives you the dimensions of your data, in the form `(#rows, #columns)`.


```python
# This returns a tuple (or linked pairs) of the number of rows and columns in your dataframe.
weather_observations.shape
```




    (19918, 12)



So we can see here that we have 12 columns, and 19,918 rows of data.




```python
# Calling the first or second element of the tuple can give you either the rows or the columns
#Gives you the rows (remember, 0 indexing!)
print(weather_observations.shape[0])

#Gives you the columns
print(weather_observations.shape[1])
```

    19918
    12
    

You can also use `df.columns` to examine the column names.


```python
weather_observations.columns
```




    Index(['Datetime', 'Wind dir', 'Wind spd', 'Wind gust', 'Tmp', 'Dew pt',
           'Feels like', 'rh', 'Fire', 'Rain', 'Rain 10'', 'Pres'],
          dtype='object')



You can also examine the data types inside your columns using `dtypes`


```python
weather_observations.dtypes
```




    Datetime      datetime64[ns]
    Wind dir              object
    Wind spd             float64
    Wind gust            float64
    Tmp                  float64
    Dew pt               float64
    Feels like           float64
    rh                   float64
    Fire                 float64
    Rain                 float64
    Rain 10'             float64
    Pres                 float64
    dtype: object




```python
# What an "object" data type??
weather_observations.head()
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
      <th>Datetime</th>
      <th>Wind dir</th>
      <th>Wind spd</th>
      <th>Wind gust</th>
      <th>Tmp</th>
      <th>Dew pt</th>
      <th>Feels like</th>
      <th>rh</th>
      <th>Fire</th>
      <th>Rain</th>
      <th>Rain 10'</th>
      <th>Pres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2013-01-01 00:00:00</td>
      <td>WNW</td>
      <td>9.0</td>
      <td>9.0</td>
      <td>19.6</td>
      <td>0.1</td>
      <td>19.6</td>
      <td>27.0</td>
      <td>12.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2013-01-01 23:30:00</td>
      <td>WNW</td>
      <td>NaN</td>
      <td>13.0</td>
      <td>20.7</td>
      <td>0.7</td>
      <td>20.7</td>
      <td>26.0</td>
      <td>13.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2013-01-01 23:00:00</td>
      <td>W</td>
      <td>9.0</td>
      <td>11.0</td>
      <td>22.8</td>
      <td>0.9</td>
      <td>22.8</td>
      <td>23.0</td>
      <td>15.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2013-01-01 22:30:00</td>
      <td>WNW</td>
      <td>11.0</td>
      <td>13.0</td>
      <td>22.6</td>
      <td>-1.0</td>
      <td>22.6</td>
      <td>21.0</td>
      <td>17.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2013-01-01 22:00:00</td>
      <td>WNW</td>
      <td>15.0</td>
      <td>19.0</td>
      <td>23.9</td>
      <td>-2.2</td>
      <td>23.9</td>
      <td>18.0</td>
      <td>21.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.1</td>
    </tr>
  </tbody>
</table>
</div>



#### Subsetting Columns

To select a particular column in your dataframe, you can use one of two options:

1) Calling the column as an "attribute"
    - `df.columnName`

2) Subsetting the dataframe
    - `df['columnName']`

The first option is useful for some functions, but the second form is essential if you want to call more than column at once. You do this by inserting a list, [], of column names, instead a single column.

For example: `df[["Column1", "Column2", ... , etc]]`


```python
weather_observations.Tmp.head() #you can also "chain" commands
```




    0    19.6
    1    20.7
    2    22.8
    3    22.6
    4    23.9
    Name: Tmp, dtype: float64




```python
weather_observations["Wind dir"].head()
```




    0    WNW
    1    WNW
    2      W
    3    WNW
    4    WNW
    Name: Wind dir, dtype: object




```python
weather_observations.columns
```




    Index(['Datetime', 'Wind dir', 'Wind spd', 'Wind gust', 'Tmp', 'Dew pt',
           'Feels like', 'rh', 'Fire', 'Rain', 'Rain 10'', 'Pres'],
          dtype='object')




```python
# Try subsetting the first two columns
weather_observations[["Datetime","Wind dir"]].head()
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
      <th>Datetime</th>
      <th>Wind dir</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2013-01-01 00:00:00</td>
      <td>WNW</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2013-01-01 23:30:00</td>
      <td>WNW</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2013-01-01 23:00:00</td>
      <td>W</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2013-01-01 22:30:00</td>
      <td>WNW</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2013-01-01 22:00:00</td>
      <td>WNW</td>
    </tr>
  </tbody>
</table>
</div>




```python
# What happens if you change the order of the columns around?
weather_observations[["Wind dir","Datetime"]].head()
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
      <th>Wind dir</th>
      <th>Datetime</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>WNW</td>
      <td>2013-01-01 00:00:00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>WNW</td>
      <td>2013-01-01 23:30:00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>W</td>
      <td>2013-01-01 23:00:00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>WNW</td>
      <td>2013-01-01 22:30:00</td>
    </tr>
    <tr>
      <th>4</th>
      <td>WNW</td>
      <td>2013-01-01 22:00:00</td>
    </tr>
  </tbody>
</table>
</div>




```python
# What about a column that doesnt exist?
weather_observations.Wind
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-108-f7cd6c4236c8> in <module>()
          1 # What about a column that doesnt exist?
    ----> 2 weather_observations.Wind
    

    C:\Users\Kahli\Anaconda2\envs\python36\lib\site-packages\pandas\core\generic.py in __getattr__(self, name)
       2968             if name in self._info_axis:
       2969                 return self[name]
    -> 2970             return object.__getattribute__(self, name)
       2971 
       2972     def __setattr__(self, name, value):
    

    AttributeError: 'DataFrame' object has no attribute 'Wind'


#### Slicing

Sometimes you need to examine specific rows and columns in the middle of your data though, which aren't covered by `head()` or `tail()`. Instead, you can use index slicing.

Slicing works similarly to how you might slice a string, or a list. You simply call the indexes of the rows you want from the dataframe: `df[rowNumbers]`


```python
weather_observations[:3]
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
      <th>Datetime</th>
      <th>Wind dir</th>
      <th>Wind spd</th>
      <th>Wind gust</th>
      <th>Tmp</th>
      <th>Dew pt</th>
      <th>Feels like</th>
      <th>rh</th>
      <th>Fire</th>
      <th>Rain</th>
      <th>Rain 10'</th>
      <th>Pres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2013-01-01 00:00:00</td>
      <td>WNW</td>
      <td>9.0</td>
      <td>9.0</td>
      <td>19.6</td>
      <td>0.1</td>
      <td>19.6</td>
      <td>27.0</td>
      <td>12.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2013-01-01 23:30:00</td>
      <td>WNW</td>
      <td>NaN</td>
      <td>13.0</td>
      <td>20.7</td>
      <td>0.7</td>
      <td>20.7</td>
      <td>26.0</td>
      <td>13.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2013-01-01 23:00:00</td>
      <td>W</td>
      <td>9.0</td>
      <td>11.0</td>
      <td>22.8</td>
      <td>0.9</td>
      <td>22.8</td>
      <td>23.0</td>
      <td>15.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
weather_observations[["Pres","rh","Fire"]][:10]
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
      <th>Pres</th>
      <th>rh</th>
      <th>Fire</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1011.4</td>
      <td>27.0</td>
      <td>12.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1011.1</td>
      <td>26.0</td>
      <td>13.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1011.0</td>
      <td>23.0</td>
      <td>15.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1011.0</td>
      <td>21.0</td>
      <td>17.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1011.1</td>
      <td>18.0</td>
      <td>21.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1011.0</td>
      <td>18.0</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1010.8</td>
      <td>13.0</td>
      <td>27.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1010.6</td>
      <td>10.0</td>
      <td>37.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1010.4</td>
      <td>13.0</td>
      <td>33.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1010.2</td>
      <td>10.0</td>
      <td>41.0</td>
    </tr>
  </tbody>
</table>
</div>



#### `loc` vs `iloc`

You can also use `loc` and `iloc` to slice rows and columns.

`df.iloc[]` is positional based, so takes integer values that correlate with the row and column **numbers** in your dataframe

`df.loc[]` is label based, and takes the row and column **names** as inputs

For this example we're going to use our toy dataframe, df, as an example


```python
df
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>49</th>
      <td>a</td>
      <td>54.0</td>
      <td>1.118139</td>
      <td>2.6</td>
    </tr>
    <tr>
      <th>48</th>
      <td>b</td>
      <td>67.0</td>
      <td>-1.293534</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>47</th>
      <td>c</td>
      <td>89.0</td>
      <td>-0.490262</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>d</td>
      <td>100.0</td>
      <td>0.905747</td>
      <td>9.4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>e</td>
      <td>NaN</td>
      <td>-0.869318</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>f</td>
      <td>64.0</td>
      <td>0.146700</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Using iloc to get rows 0, 1 and 2
df.iloc[:3]
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>49</th>
      <td>a</td>
      <td>54.0</td>
      <td>1.118139</td>
      <td>2.6</td>
    </tr>
    <tr>
      <th>48</th>
      <td>b</td>
      <td>67.0</td>
      <td>-1.293534</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>47</th>
      <td>c</td>
      <td>89.0</td>
      <td>-0.490262</td>
      <td>8.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Using loc to get row labels 1, 2 and 3
df.loc[1:3]
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>d</td>
      <td>100.0</td>
      <td>0.905747</td>
      <td>9.4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>e</td>
      <td>NaN</td>
      <td>-0.869318</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>f</td>
      <td>64.0</td>
      <td>0.146700</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



If you only enter one set of values into `loc` and `iloc`, they will return the values for every column in your dataset.

By using a second integer though, you can choose which rows and columns you specifically want to subset. The first value corresponds to the row, and the second to the columns, or rows x columns. You can also think of this with the moniker *"Roman Catholic"*


```python
# Using loc to get row labels 1, 2 and 3 for Columns A and B
df.loc[1:3,["A","B"]]
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
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>d</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>e</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>f</td>
      <td>64.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Using iloc to get rows 0, 1 and 2 for columns 0 and 1.
df.iloc[:3,:2]
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
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>49</th>
      <td>a</td>
      <td>54.0</td>
    </tr>
    <tr>
      <th>48</th>
      <td>b</td>
      <td>67.0</td>
    </tr>
    <tr>
      <th>47</th>
      <td>c</td>
      <td>89.0</td>
    </tr>
  </tbody>
</table>
</div>



The differences between `loc` and `iloc` can seem minor, but they're very important, and which one you should use depends on what your needs are at the time.

`iloc` is based on dataframe position, so calling `iloc[:3]` would give you rows 0 through 3. 

`loc` however is based on the index label, so if you were to call `df.loc[:3]`, it would give you all rows UP TO the row labelled as index 3.

While the indexes are in order and all present, this isn't an issue. Consider what happens when the indexes are out of order though, with our dataframe 'df'


```python
# Can see that this only takes the first 2 rows
df.iloc[:2]
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>49</th>
      <td>a</td>
      <td>54.0</td>
      <td>1.118139</td>
      <td>2.6</td>
    </tr>
    <tr>
      <th>48</th>
      <td>b</td>
      <td>67.0</td>
      <td>-1.293534</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Whereas this takes all rows UP TO index label 2
df.loc[:2]
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>49</th>
      <td>a</td>
      <td>54.0</td>
      <td>1.118139</td>
      <td>2.6</td>
    </tr>
    <tr>
      <th>48</th>
      <td>b</td>
      <td>67.0</td>
      <td>-1.293534</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>47</th>
      <td>c</td>
      <td>89.0</td>
      <td>-0.490262</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>d</td>
      <td>100.0</td>
      <td>0.905747</td>
      <td>9.4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>e</td>
      <td>NaN</td>
      <td>-0.869318</td>
      <td>3.3</td>
    </tr>
  </tbody>
</table>
</div>



Since `loc` is based on labels, if you try to subset a row or column label that doesn't exist, even if it corresponds to a positional row, python will throw you an error


```python
df.loc[0]
```


    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    C:\Users\Kahli\Anaconda2\envs\python36\lib\site-packages\pandas\core\indexing.py in _has_valid_type(self, key, axis)
       1433                 if not ax.contains(key):
    -> 1434                     error()
       1435             except TypeError as e:
    

    C:\Users\Kahli\Anaconda2\envs\python36\lib\site-packages\pandas\core\indexing.py in error()
       1428                 raise KeyError("the label [%s] is not in the [%s]" %
    -> 1429                                (key, self.obj._get_axis_name(axis)))
       1430 
    

    KeyError: 'the label [0] is not in the [index]'

    
    During handling of the above exception, another exception occurred:
    

    KeyError                                  Traceback (most recent call last)

    <ipython-input-74-549f4325178e> in <module>()
    ----> 1 df.loc[0]
    

    C:\Users\Kahli\Anaconda2\envs\python36\lib\site-packages\pandas\core\indexing.py in __getitem__(self, key)
       1326         else:
       1327             key = com._apply_if_callable(key, self.obj)
    -> 1328             return self._getitem_axis(key, axis=0)
       1329 
       1330     def _is_scalar_access(self, key):
    

    C:\Users\Kahli\Anaconda2\envs\python36\lib\site-packages\pandas\core\indexing.py in _getitem_axis(self, key, axis)
       1549 
       1550         # fall thru to straight lookup
    -> 1551         self._has_valid_type(key, axis)
       1552         return self._get_label(key, axis=axis)
       1553 
    

    C:\Users\Kahli\Anaconda2\envs\python36\lib\site-packages\pandas\core\indexing.py in _has_valid_type(self, key, axis)
       1440                 raise
       1441             except:
    -> 1442                 error()
       1443 
       1444         return True
    

    C:\Users\Kahli\Anaconda2\envs\python36\lib\site-packages\pandas\core\indexing.py in error()
       1427                                     "key")
       1428                 raise KeyError("the label [%s] is not in the [%s]" %
    -> 1429                                (key, self.obj._get_axis_name(axis)))
       1430 
       1431             try:
    

    KeyError: 'the label [0] is not in the [index]'


Due to this, it's important that you carefully consider which tool is appropriate for your needs

#### Challenge

Find the 11th through 20th (inclusive) rows for the columns related to wind variables using each of loc, iloc and slicing


```python
#slicing

```


```python
#loc

```


```python
#iloc

```

#### Subsetting with Conditionals

You can also subset using conditional statements, such as ==, !=, >, <, etc.

For example, if I want to find all of the rows where Wind Gust > 80, I would type:


```python
# All columns/rows where the wind gusts > 80
weather_observations[weather_observations["Wind gust"] > 80]
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
      <th>Datetime</th>
      <th>Wind dir</th>
      <th>Wind spd</th>
      <th>Wind gust</th>
      <th>Tmp</th>
      <th>Dew pt</th>
      <th>Feels like</th>
      <th>rh</th>
      <th>Fire</th>
      <th>Rain</th>
      <th>Rain 10'</th>
      <th>Pres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>396</th>
      <td>2013-01-08 13:30:00</td>
      <td>NW</td>
      <td>48.0</td>
      <td>83.0</td>
      <td>35.1</td>
      <td>3.2</td>
      <td>33.0</td>
      <td>14.0</td>
      <td>77.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1003.4</td>
    </tr>
    <tr>
      <th>14768</th>
      <td>2013-09-26 13:20:00</td>
      <td>WNW</td>
      <td>48.0</td>
      <td>81.0</td>
      <td>14.2</td>
      <td>0.8</td>
      <td>11.3</td>
      <td>40.0</td>
      <td>16.0</td>
      <td>0.8</td>
      <td>0.0</td>
      <td>1007.4</td>
    </tr>
    <tr>
      <th>15309</th>
      <td>2013-10-06 16:45:00</td>
      <td>W</td>
      <td>43.0</td>
      <td>81.0</td>
      <td>21.9</td>
      <td>1.0</td>
      <td>21.9</td>
      <td>25.0</td>
      <td>30.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1008.6</td>
    </tr>
    <tr>
      <th>17766</th>
      <td>2013-11-21 18:46:00</td>
      <td>N</td>
      <td>57.0</td>
      <td>93.0</td>
      <td>15.0</td>
      <td>14.8</td>
      <td>15.0</td>
      <td>99.0</td>
      <td>3.0</td>
      <td>23.2</td>
      <td>21.2</td>
      <td>1010.5</td>
    </tr>
    <tr>
      <th>17767</th>
      <td>2013-11-21 18:41:00</td>
      <td>N</td>
      <td>69.0</td>
      <td>93.0</td>
      <td>13.3</td>
      <td>10.5</td>
      <td>9.4</td>
      <td>83.0</td>
      <td>6.0</td>
      <td>9.8</td>
      <td>9.4</td>
      <td>1009.1</td>
    </tr>
    <tr>
      <th>18716</th>
      <td>2013-12-09 15:48:00</td>
      <td>WNW</td>
      <td>41.0</td>
      <td>81.0</td>
      <td>28.4</td>
      <td>1.0</td>
      <td>27.0</td>
      <td>17.0</td>
      <td>47.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1004.3</td>
    </tr>
  </tbody>
</table>
</div>



Just as with lists and for loops, etc, you can also combine these conditionals using & {and} , or | {or}


```python
#Find the rows where Wind spd < 10 AND Pres > 1020
weather_observations[(weather_observations["Wind spd"] > 60) & (weather_observations.Pres > 1000)]
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
      <th>Datetime</th>
      <th>Wind dir</th>
      <th>Wind spd</th>
      <th>Wind gust</th>
      <th>Tmp</th>
      <th>Dew pt</th>
      <th>Feels like</th>
      <th>rh</th>
      <th>Fire</th>
      <th>Rain</th>
      <th>Rain 10'</th>
      <th>Pres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>390</th>
      <td>2013-01-08 15:17:00</td>
      <td>NW</td>
      <td>65.0</td>
      <td>78.0</td>
      <td>34.8</td>
      <td>1.8</td>
      <td>32.0</td>
      <td>13.0</td>
      <td>117.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1000.1</td>
    </tr>
    <tr>
      <th>14333</th>
      <td>2013-09-18 21:08:00</td>
      <td>W</td>
      <td>65.0</td>
      <td>78.0</td>
      <td>10.8</td>
      <td>6.4</td>
      <td>6.0</td>
      <td>74.0</td>
      <td>6.0</td>
      <td>3.2</td>
      <td>0.0</td>
      <td>1003.5</td>
    </tr>
    <tr>
      <th>17767</th>
      <td>2013-11-21 18:41:00</td>
      <td>N</td>
      <td>69.0</td>
      <td>93.0</td>
      <td>13.3</td>
      <td>10.5</td>
      <td>9.4</td>
      <td>83.0</td>
      <td>6.0</td>
      <td>9.8</td>
      <td>9.4</td>
      <td>1009.1</td>
    </tr>
  </tbody>
</table>
</div>




```python
#You can also get a list of the row indexes for your subset
list(weather_observations[(weather_observations["Wind spd"] > 60) & (weather_observations.Pres > 1000)].index)
```




    [390, 14333, 17767]



Similarly, you can subset using boolean lists


```python
# a boolean list. The 1st, 3rd and 5th values are True.
na = weather_observations["Wind spd"].isnull()

weather_observations[na] #subsets all rows where Wind spd = None
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
      <th>Datetime</th>
      <th>Wind dir</th>
      <th>Wind spd</th>
      <th>Wind gust</th>
      <th>Tmp</th>
      <th>Dew pt</th>
      <th>Feels like</th>
      <th>rh</th>
      <th>Fire</th>
      <th>Rain</th>
      <th>Rain 10'</th>
      <th>Pres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>2013-01-01 23:30:00</td>
      <td>WNW</td>
      <td>NaN</td>
      <td>13.0</td>
      <td>20.7</td>
      <td>0.7</td>
      <td>20.7</td>
      <td>26.0</td>
      <td>13.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.1</td>
    </tr>
    <tr>
      <th>220</th>
      <td>2013-01-05 18:30:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>28.9</td>
      <td>12.8</td>
      <td>28.0</td>
      <td>37.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1015.4</td>
    </tr>
    <tr>
      <th>221</th>
      <td>2013-01-05 18:06:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>33.5</td>
      <td>9.3</td>
      <td>32.0</td>
      <td>23.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.9</td>
    </tr>
    <tr>
      <th>222</th>
      <td>2013-01-05 18:00:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>35.4</td>
      <td>11.5</td>
      <td>34.0</td>
      <td>24.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.7</td>
    </tr>
    <tr>
      <th>223</th>
      <td>2013-01-05 17:30:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>37.3</td>
      <td>10.6</td>
      <td>36.0</td>
      <td>20.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.0</td>
    </tr>
    <tr>
      <th>224</th>
      <td>2013-01-05 17:00:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>39.2</td>
      <td>9.7</td>
      <td>38.0</td>
      <td>17.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.1</td>
    </tr>
    <tr>
      <th>225</th>
      <td>2013-01-05 16:30:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>39.5</td>
      <td>10.2</td>
      <td>38.0</td>
      <td>17.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.1</td>
    </tr>
    <tr>
      <th>226</th>
      <td>2013-01-05 16:00:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>38.0</td>
      <td>9.4</td>
      <td>36.0</td>
      <td>18.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.3</td>
    </tr>
    <tr>
      <th>272</th>
      <td>2013-01-06 18:00:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>26.3</td>
      <td>15.9</td>
      <td>27.0</td>
      <td>53.0</td>
      <td>NaN</td>
      <td>0.4</td>
      <td>0.4</td>
      <td>1020.4</td>
    </tr>
    <tr>
      <th>273</th>
      <td>2013-01-06 17:52:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>31.4</td>
      <td>17.2</td>
      <td>32.0</td>
      <td>43.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1020.4</td>
    </tr>
    <tr>
      <th>274</th>
      <td>2013-01-06 17:38:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>31.7</td>
      <td>17.0</td>
      <td>32.0</td>
      <td>41.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1020.1</td>
    </tr>
    <tr>
      <th>275</th>
      <td>2013-01-06 17:30:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>32.3</td>
      <td>17.2</td>
      <td>33.0</td>
      <td>41.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1019.8</td>
    </tr>
    <tr>
      <th>276</th>
      <td>2013-01-06 17:00:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>35.7</td>
      <td>14.5</td>
      <td>35.0</td>
      <td>28.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1018.6</td>
    </tr>
    <tr>
      <th>277</th>
      <td>2013-01-06 16:30:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>36.6</td>
      <td>4.1</td>
      <td>34.0</td>
      <td>13.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1018.6</td>
    </tr>
    <tr>
      <th>278</th>
      <td>2013-01-06 16:00:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>36.6</td>
      <td>9.2</td>
      <td>35.0</td>
      <td>19.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1018.9</td>
    </tr>
    <tr>
      <th>1044</th>
      <td>2013-01-20 14:02:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>26.0</td>
      <td>12.7</td>
      <td>26.0</td>
      <td>44.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1016.0</td>
    </tr>
    <tr>
      <th>1353</th>
      <td>2013-01-26 18:39:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.2</td>
      <td>17.7</td>
      <td>18.2</td>
      <td>97.0</td>
      <td>NaN</td>
      <td>3.8</td>
      <td>NaN</td>
      <td>1012.6</td>
    </tr>
    <tr>
      <th>2626</th>
      <td>2013-02-19 17:00:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>29.1</td>
      <td>9.3</td>
      <td>28.0</td>
      <td>29.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1013.3</td>
    </tr>
    <tr>
      <th>2627</th>
      <td>2013-02-19 16:30:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>30.3</td>
      <td>7.8</td>
      <td>29.0</td>
      <td>25.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1013.0</td>
    </tr>
    <tr>
      <th>2786</th>
      <td>2013-02-22 13:39:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>25.3</td>
      <td>11.1</td>
      <td>26.0</td>
      <td>41.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>1020.9</td>
    </tr>
    <tr>
      <th>2787</th>
      <td>2013-02-22 13:30:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>25.3</td>
      <td>11.1</td>
      <td>26.0</td>
      <td>41.0</td>
      <td>NaN</td>
      <td>0.6</td>
      <td>0.0</td>
      <td>1020.9</td>
    </tr>
    <tr>
      <th>3989</th>
      <td>2013-03-16 12:30:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>24.3</td>
      <td>11.5</td>
      <td>24.3</td>
      <td>45.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1013.0</td>
    </tr>
    <tr>
      <th>3990</th>
      <td>2013-03-16 12:00:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>22.9</td>
      <td>12.3</td>
      <td>22.9</td>
      <td>51.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1013.9</td>
    </tr>
    <tr>
      <th>3991</th>
      <td>2013-03-16 11:30:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>22.6</td>
      <td>14.1</td>
      <td>22.6</td>
      <td>59.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.1</td>
    </tr>
  </tbody>
</table>
</div>



#### Challenge

How many rows exist where null values are concurrently present in the columns "Wind dir", "Fire" and "Wind spd"?

_Hint: Remember that you can combine conditions with '&'_


```python

```


```python

```


```python

```

#### Describing your data

Often you'll also want a quick summary of your data and what's in each column

You can do this with the function `describe()`


```python
weather_observations.describe()
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
      <th>Wind spd</th>
      <th>Wind gust</th>
      <th>Tmp</th>
      <th>Dew pt</th>
      <th>Feels like</th>
      <th>rh</th>
      <th>Fire</th>
      <th>Rain</th>
      <th>Rain 10'</th>
      <th>Pres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>19894.000000</td>
      <td>19531.000000</td>
      <td>19906.000000</td>
      <td>19878.000000</td>
      <td>19906.000000</td>
      <td>19878.000000</td>
      <td>19855.000000</td>
      <td>19224.000000</td>
      <td>19836.000000</td>
      <td>19906.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>13.582035</td>
      <td>19.602581</td>
      <td>13.170029</td>
      <td>6.258527</td>
      <td>12.192917</td>
      <td>68.722055</td>
      <td>5.309091</td>
      <td>0.869320</td>
      <td>0.018542</td>
      <td>1017.574159</td>
    </tr>
    <tr>
      <th>std</th>
      <td>10.109245</td>
      <td>13.965958</td>
      <td>7.787843</td>
      <td>4.884600</td>
      <td>8.296635</td>
      <td>23.663755</td>
      <td>8.258205</td>
      <td>3.807593</td>
      <td>0.280324</td>
      <td>6.825606</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>-5.800000</td>
      <td>-13.300000</td>
      <td>-8.100000</td>
      <td>8.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>994.500000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>7.000000</td>
      <td>9.000000</td>
      <td>7.600000</td>
      <td>2.700000</td>
      <td>5.700000</td>
      <td>50.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1012.800000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>11.000000</td>
      <td>15.000000</td>
      <td>12.500000</td>
      <td>6.000000</td>
      <td>11.300000</td>
      <td>73.000000</td>
      <td>2.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1017.500000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>20.000000</td>
      <td>28.000000</td>
      <td>18.100000</td>
      <td>9.700000</td>
      <td>18.100000</td>
      <td>90.000000</td>
      <td>6.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1022.600000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>69.000000</td>
      <td>93.000000</td>
      <td>40.200000</td>
      <td>19.700000</td>
      <td>38.000000</td>
      <td>100.000000</td>
      <td>117.000000</td>
      <td>57.800000</td>
      <td>21.200000</td>
      <td>1036.100000</td>
    </tr>
  </tbody>
</table>
</div>



You'll notice that it's excluded the non-numeric columns though.

To get a description of all of your data at once, you need to give `describe` the `include = all` argument


```python
weather_observations.describe(include = 'all')
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
      <th>Datetime</th>
      <th>Wind dir</th>
      <th>Wind spd</th>
      <th>Wind gust</th>
      <th>Tmp</th>
      <th>Dew pt</th>
      <th>Feels like</th>
      <th>rh</th>
      <th>Fire</th>
      <th>Rain</th>
      <th>Rain 10'</th>
      <th>Pres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>19918</td>
      <td>17955</td>
      <td>19894.000000</td>
      <td>19531.000000</td>
      <td>19906.000000</td>
      <td>19878.000000</td>
      <td>19906.000000</td>
      <td>19878.000000</td>
      <td>19855.000000</td>
      <td>19224.000000</td>
      <td>19836.000000</td>
      <td>19906.000000</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>19556</td>
      <td>16</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>top</th>
      <td>2013-04-14 00:00:00</td>
      <td>N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>2</td>
      <td>1940</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>first</th>
      <td>2013-01-01 00:00:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>last</th>
      <td>2013-12-31 23:30:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>13.582035</td>
      <td>19.602581</td>
      <td>13.170029</td>
      <td>6.258527</td>
      <td>12.192917</td>
      <td>68.722055</td>
      <td>5.309091</td>
      <td>0.869320</td>
      <td>0.018542</td>
      <td>1017.574159</td>
    </tr>
    <tr>
      <th>std</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>10.109245</td>
      <td>13.965958</td>
      <td>7.787843</td>
      <td>4.884600</td>
      <td>8.296635</td>
      <td>23.663755</td>
      <td>8.258205</td>
      <td>3.807593</td>
      <td>0.280324</td>
      <td>6.825606</td>
    </tr>
    <tr>
      <th>min</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>-5.800000</td>
      <td>-13.300000</td>
      <td>-8.100000</td>
      <td>8.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>994.500000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>7.000000</td>
      <td>9.000000</td>
      <td>7.600000</td>
      <td>2.700000</td>
      <td>5.700000</td>
      <td>50.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1012.800000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>11.000000</td>
      <td>15.000000</td>
      <td>12.500000</td>
      <td>6.000000</td>
      <td>11.300000</td>
      <td>73.000000</td>
      <td>2.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1017.500000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>20.000000</td>
      <td>28.000000</td>
      <td>18.100000</td>
      <td>9.700000</td>
      <td>18.100000</td>
      <td>90.000000</td>
      <td>6.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1022.600000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>69.000000</td>
      <td>93.000000</td>
      <td>40.200000</td>
      <td>19.700000</td>
      <td>38.000000</td>
      <td>100.000000</td>
      <td>117.000000</td>
      <td>57.800000</td>
      <td>21.200000</td>
      <td>1036.100000</td>
    </tr>
  </tbody>
</table>
</div>



We can also find specific statistics for each of these columns by using `max()`, `min()`, `count()`, `std()` {standard deviation}, `mean()` and `sum()`. 

Just remember though that many of these functions rely on numeric data types, and will cause errors if used on a str type. Calling `sum()` on a string however will concatentate those strings.


```python
#The maximum value in Fire
weather_observations.Fire.max()
```




    117.0




```python
# The number of null values in Fire
weather_observations.Fire.isnull().sum()
```




    63




```python
#Try finding the maximum of the Location (a string) column
weather_observations['Wind dir'].max()
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    C:\Users\Kahli\Anaconda2\envs\python36\lib\site-packages\pandas\core\nanops.py in f(values, axis, skipna, **kwds)
        118                 else:
    --> 119                     result = alt(values, axis=axis, skipna=skipna, **kwds)
        120             except Exception:
    

    C:\Users\Kahli\Anaconda2\envs\python36\lib\site-packages\pandas\core\nanops.py in reduction(values, axis, skipna)
        459         else:
    --> 460             result = getattr(values, meth)(axis)
        461 
    

    C:\Users\Kahli\Anaconda2\envs\python36\lib\site-packages\numpy\core\_methods.py in _amax(a, axis, out, keepdims)
         25 def _amax(a, axis=None, out=None, keepdims=False):
    ---> 26     return umr_maximum(a, axis, None, out, keepdims)
         27 
    

    TypeError: '>=' not supported between instances of 'str' and 'float'

    
    During handling of the above exception, another exception occurred:
    

    TypeError                                 Traceback (most recent call last)

    <ipython-input-173-e28b52fa0a98> in <module>()
          1 #Try finding the maximum of the Location (a string) column
    ----> 2 weather_observations['Wind dir'].max()
    

    C:\Users\Kahli\Anaconda2\envs\python36\lib\site-packages\pandas\core\generic.py in stat_func(self, axis, skipna, level, numeric_only, **kwargs)
       6195                                       skipna=skipna)
       6196         return self._reduce(f, name, axis=axis, skipna=skipna,
    -> 6197                             numeric_only=numeric_only)
       6198 
       6199     return set_function_name(stat_func, name, cls)
    

    C:\Users\Kahli\Anaconda2\envs\python36\lib\site-packages\pandas\core\series.py in _reduce(self, op, name, axis, skipna, numeric_only, filter_type, **kwds)
       2379                                           'numeric_only.'.format(name))
       2380             with np.errstate(all='ignore'):
    -> 2381                 return op(delegate, skipna=skipna, **kwds)
       2382 
       2383         return delegate._reduce(op=op, name=name, axis=axis, skipna=skipna,
    

    C:\Users\Kahli\Anaconda2\envs\python36\lib\site-packages\pandas\core\nanops.py in f(values, axis, skipna, **kwds)
        120             except Exception:
        121                 try:
    --> 122                     result = alt(values, axis=axis, skipna=skipna, **kwds)
        123                 except ValueError as e:
        124                     # we want to transform an object array
    

    C:\Users\Kahli\Anaconda2\envs\python36\lib\site-packages\pandas\core\nanops.py in reduction(values, axis, skipna)
        458                 result = np.nan
        459         else:
    --> 460             result = getattr(values, meth)(axis)
        461 
        462         result = _wrap_results(result, dtype)
    

    C:\Users\Kahli\Anaconda2\envs\python36\lib\site-packages\numpy\core\_methods.py in _amax(a, axis, out, keepdims)
         24 # small reductions
         25 def _amax(a, axis=None, out=None, keepdims=False):
    ---> 26     return umr_maximum(a, axis, None, out, keepdims)
         27 
         28 def _amin(a, axis=None, out=None, keepdims=False):
    

    TypeError: '>=' not supported between instances of 'str' and 'float'


It's often useful to know exactly what and how many different values you have in categorical columns.

`unique()` works on both numeric and string data, and gives you a list of all of the unique values within a particular column.


```python
weather_observations["Wind dir"].unique()
```




    array(['WNW', 'W', 'NW', 'N', nan, 'SSW', 'ESE', 'SE', 'SW', 'ENE', 'E',
           'NE', 'NNW', 'SSE', 'NNE', 'S', 'WSW'], dtype=object)



You can get the size/how many unique values there in two ways:

1) Taking the `len()` of the array  
2) Using the numpy `size` function


```python
print("Len:", len(weather_observations["Wind dir"].unique()))

print("Numpy:", weather_observations["Wind dir"].unique().size)
```

    Len: 17
    Numpy: 17
    

## Cleaning and Manipulating Data

- Dealing with duplicates
- Sorting data
- using the `apply()` function
    - `reset_index` for row names
- Adding and deleting columns/rows
    - renaming columns
- setting data frequency


If you look at our data you can notice some funny things.


```python
# First 5 and last 5 values around the 01/01/2013
weather_observations["Datetime"].iloc[list(range(5)) + list(range(48,53))]
```




    0    2013-01-01 00:00:00
    1    2013-01-01 23:30:00
    2    2013-01-01 23:00:00
    3    2013-01-01 22:30:00
    4    2013-01-01 22:00:00
    48   2013-01-01 01:00:00
    49   2013-01-01 00:30:00
    50   2013-01-01 00:00:00
    51   2013-01-02 00:00:00
    52   2013-01-02 23:30:00
    Name: Datetime, dtype: datetime64[ns]



Not only does it order the time series in the reverse order, but it actually includes two midnight values for the same day - one at the start and one at the end of each day


```python
# Remove duplicated items with the same date and time
no_duplicates = weather_observations.drop_duplicates('Datetime', keep='last')
```


```python
print(weather_observations.shape)
print(no_duplicates.shape)

no_duplicates.head()
```

    (19918, 12)
    (19556, 12)
    




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
      <th>Datetime</th>
      <th>Wind dir</th>
      <th>Wind spd</th>
      <th>Wind gust</th>
      <th>Tmp</th>
      <th>Dew pt</th>
      <th>Feels like</th>
      <th>rh</th>
      <th>Fire</th>
      <th>Rain</th>
      <th>Rain 10'</th>
      <th>Pres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>2013-01-01 23:30:00</td>
      <td>WNW</td>
      <td>NaN</td>
      <td>13.0</td>
      <td>20.7</td>
      <td>0.7</td>
      <td>20.7</td>
      <td>26.0</td>
      <td>13.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2013-01-01 23:00:00</td>
      <td>W</td>
      <td>9.0</td>
      <td>11.0</td>
      <td>22.8</td>
      <td>0.9</td>
      <td>22.8</td>
      <td>23.0</td>
      <td>15.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2013-01-01 22:30:00</td>
      <td>WNW</td>
      <td>11.0</td>
      <td>13.0</td>
      <td>22.6</td>
      <td>-1.0</td>
      <td>22.6</td>
      <td>21.0</td>
      <td>17.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2013-01-01 22:00:00</td>
      <td>WNW</td>
      <td>15.0</td>
      <td>19.0</td>
      <td>23.9</td>
      <td>-2.2</td>
      <td>23.9</td>
      <td>18.0</td>
      <td>21.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2013-01-01 21:30:00</td>
      <td>W</td>
      <td>13.0</td>
      <td>17.0</td>
      <td>24.0</td>
      <td>-2.0</td>
      <td>24.0</td>
      <td>18.0</td>
      <td>20.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1011.0</td>
    </tr>
  </tbody>
</table>
</div>



We can also use `sort_values()` to sort our data in chronological order


```python
# Sorting is ascending by default, or chronological order
sorted_dataframe = no_duplicates.sort_values('Datetime')
```


```python
sorted_dataframe.head()
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
      <th>Datetime</th>
      <th>Wind dir</th>
      <th>Wind spd</th>
      <th>Wind gust</th>
      <th>Tmp</th>
      <th>Dew pt</th>
      <th>Feels like</th>
      <th>rh</th>
      <th>Fire</th>
      <th>Rain</th>
      <th>Rain 10'</th>
      <th>Pres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>50</th>
      <td>2013-01-01 00:00:00</td>
      <td>ENE</td>
      <td>2.0</td>
      <td>6.0</td>
      <td>15.9</td>
      <td>3.7</td>
      <td>15.9</td>
      <td>44.0</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.1</td>
    </tr>
    <tr>
      <th>49</th>
      <td>2013-01-01 00:30:00</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>14.5</td>
      <td>4.7</td>
      <td>14.5</td>
      <td>52.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.0</td>
    </tr>
    <tr>
      <th>48</th>
      <td>2013-01-01 01:00:00</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>12.8</td>
      <td>4.6</td>
      <td>12.8</td>
      <td>57.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1013.5</td>
    </tr>
    <tr>
      <th>47</th>
      <td>2013-01-01 01:30:00</td>
      <td>SSW</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>13.3</td>
      <td>5.9</td>
      <td>13.3</td>
      <td>61.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1013.4</td>
    </tr>
    <tr>
      <th>46</th>
      <td>2013-01-01 02:00:00</td>
      <td>SW</td>
      <td>2.0</td>
      <td>7.0</td>
      <td>12.5</td>
      <td>4.7</td>
      <td>12.5</td>
      <td>59.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1013.2</td>
    </tr>
  </tbody>
</table>
</div>



Similarly, while querying our data, it might be better to have our datetime as our indexes, rather than some arbitrary numbers


```python
# Use `Datetime` as our DataFrame index
indexed_weather_observations = sorted_dataframe.set_index('Datetime')
```


```python
indexed_weather_observations.head()
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
      <th>Wind dir</th>
      <th>Wind spd</th>
      <th>Wind gust</th>
      <th>Tmp</th>
      <th>Dew pt</th>
      <th>Feels like</th>
      <th>rh</th>
      <th>Fire</th>
      <th>Rain</th>
      <th>Rain 10'</th>
      <th>Pres</th>
    </tr>
    <tr>
      <th>Datetime</th>
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
      <th>2013-01-01 00:00:00</th>
      <td>ENE</td>
      <td>2.0</td>
      <td>6.0</td>
      <td>15.9</td>
      <td>3.7</td>
      <td>15.9</td>
      <td>44.0</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.1</td>
    </tr>
    <tr>
      <th>2013-01-01 00:30:00</th>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>14.5</td>
      <td>4.7</td>
      <td>14.5</td>
      <td>52.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.0</td>
    </tr>
    <tr>
      <th>2013-01-01 01:00:00</th>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>12.8</td>
      <td>4.6</td>
      <td>12.8</td>
      <td>57.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1013.5</td>
    </tr>
    <tr>
      <th>2013-01-01 01:30:00</th>
      <td>SSW</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>13.3</td>
      <td>5.9</td>
      <td>13.3</td>
      <td>61.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1013.4</td>
    </tr>
    <tr>
      <th>2013-01-01 02:00:00</th>
      <td>SW</td>
      <td>2.0</td>
      <td>7.0</td>
      <td>12.5</td>
      <td>4.7</td>
      <td>12.5</td>
      <td>59.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1013.2</td>
    </tr>
  </tbody>
</table>
</div>



Otherwise, if you didn't want to index your data with another column, you can just do `df.reset_index()`

### Adding and deleting columns

#### The `apply()` function

If we wanted to model this data, we would need the `Wind  dir` column to be numeric values, rather than strings.

Thankfully, we can do this using the "apply" function.

Apply will essentially "apply" a function of your choosing to the specified column/s. This could be to create a new column based off of the values of other columns, or, as in this case, processing the data within a single column to become something new.

In this particular instance, each of our wind directions corresponds to an angle:
 * North wind (↓) is 0 degrees, going clockwise ⟳.
 * East wind (←) is 90 degrees.
 * South wind (↑) is 180 degrees
 * West wind (→) is 270 degrees
 * etc


```python
# Translate wind direction to degrees
wind_directions = {
     'N':   0. , 'NNE':  22.5, 'NE':  45. , 'ENE':  67.5 ,
     'E':  90. , 'ESE': 112.5, 'SE': 135. , 'SSE': 157.5 ,
     'S': 180. , 'SSW': 202.5, 'SW': 225. , 'WSW': 247.5 ,
     'W': 270. , 'WNW': 292.5, 'NW': 315. , 'NNW': 337.5 }
```

If we wanted to create a new column, called "Wind deg", we simply need to assign a new "series" object to a new column in our dataframe


```python
# Create a new wind directions column with a new number column
# `get()` accesses values safely from dictionary

# Create a new column 
indexed_weather_observations['Wind deg'] = \
    indexed_weather_observations['Wind dir'].apply(wind_directions.get) # using these values
```


```python
indexed_weather_observations.head()
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
      <th>Wind dir</th>
      <th>Wind spd</th>
      <th>Wind gust</th>
      <th>Tmp</th>
      <th>Dew pt</th>
      <th>Feels like</th>
      <th>rh</th>
      <th>Fire</th>
      <th>Rain</th>
      <th>Rain 10'</th>
      <th>Pres</th>
      <th>Wind deg</th>
    </tr>
    <tr>
      <th>Datetime</th>
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
      <th>2013-01-01 00:00:00</th>
      <td>ENE</td>
      <td>2.0</td>
      <td>6.0</td>
      <td>15.9</td>
      <td>3.7</td>
      <td>15.9</td>
      <td>44.0</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.1</td>
      <td>67.5</td>
    </tr>
    <tr>
      <th>2013-01-01 00:30:00</th>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>14.5</td>
      <td>4.7</td>
      <td>14.5</td>
      <td>52.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2013-01-01 01:00:00</th>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>12.8</td>
      <td>4.6</td>
      <td>12.8</td>
      <td>57.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1013.5</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2013-01-01 01:30:00</th>
      <td>SSW</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>13.3</td>
      <td>5.9</td>
      <td>13.3</td>
      <td>61.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1013.4</td>
      <td>202.5</td>
    </tr>
    <tr>
      <th>2013-01-01 02:00:00</th>
      <td>SW</td>
      <td>2.0</td>
      <td>7.0</td>
      <td>12.5</td>
      <td>4.7</td>
      <td>12.5</td>
      <td>59.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1013.2</td>
      <td>225.0</td>
    </tr>
  </tbody>
</table>
</div>



Similarly, we could have just over-written the current values inside `Wind dir` instead of creating a new column.

However, now that we have a new column, we want to delete the old one. 

Just as with a list, we can do this with the command `del`


```python
del indexed_weather_observations["Wind dir"]
```


```python
indexed_weather_observations.head()
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
      <th>Wind spd</th>
      <th>Wind gust</th>
      <th>Tmp</th>
      <th>Dew pt</th>
      <th>Feels like</th>
      <th>rh</th>
      <th>Fire</th>
      <th>Rain</th>
      <th>Rain 10'</th>
      <th>Pres</th>
      <th>Wind deg</th>
    </tr>
    <tr>
      <th>Datetime</th>
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
      <th>2013-01-01 00:00:00</th>
      <td>2.0</td>
      <td>6.0</td>
      <td>15.9</td>
      <td>3.7</td>
      <td>15.9</td>
      <td>44.0</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.1</td>
      <td>67.5</td>
    </tr>
    <tr>
      <th>2013-01-01 00:30:00</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>14.5</td>
      <td>4.7</td>
      <td>14.5</td>
      <td>52.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2013-01-01 01:00:00</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>12.8</td>
      <td>4.6</td>
      <td>12.8</td>
      <td>57.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1013.5</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2013-01-01 01:30:00</th>
      <td>6.0</td>
      <td>9.0</td>
      <td>13.3</td>
      <td>5.9</td>
      <td>13.3</td>
      <td>61.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1013.4</td>
      <td>202.5</td>
    </tr>
    <tr>
      <th>2013-01-01 02:00:00</th>
      <td>2.0</td>
      <td>7.0</td>
      <td>12.5</td>
      <td>4.7</td>
      <td>12.5</td>
      <td>59.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1013.2</td>
      <td>225.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# If you then wanted to reorder your columns
indexed_weather_observations = indexed_weather_observations.iloc[:,[-1]+list(range(10))]
```


```python
indexed_weather_observations.head()
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
      <th>Wind deg</th>
      <th>Wind spd</th>
      <th>Wind gust</th>
      <th>Tmp</th>
      <th>Dew pt</th>
      <th>Feels like</th>
      <th>rh</th>
      <th>Fire</th>
      <th>Rain</th>
      <th>Rain 10'</th>
      <th>Pres</th>
    </tr>
    <tr>
      <th>Datetime</th>
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
      <th>2013-01-01 00:00:00</th>
      <td>67.5</td>
      <td>2.0</td>
      <td>6.0</td>
      <td>15.9</td>
      <td>3.7</td>
      <td>15.9</td>
      <td>44.0</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.1</td>
    </tr>
    <tr>
      <th>2013-01-01 00:30:00</th>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>14.5</td>
      <td>4.7</td>
      <td>14.5</td>
      <td>52.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.0</td>
    </tr>
    <tr>
      <th>2013-01-01 01:00:00</th>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>12.8</td>
      <td>4.6</td>
      <td>12.8</td>
      <td>57.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1013.5</td>
    </tr>
    <tr>
      <th>2013-01-01 01:30:00</th>
      <td>202.5</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>13.3</td>
      <td>5.9</td>
      <td>13.3</td>
      <td>61.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1013.4</td>
    </tr>
    <tr>
      <th>2013-01-01 02:00:00</th>
      <td>225.0</td>
      <td>2.0</td>
      <td>7.0</td>
      <td>12.5</td>
      <td>4.7</td>
      <td>12.5</td>
      <td>59.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1013.2</td>
    </tr>
  </tbody>
</table>
</div>



#### Renaming columns
If I wanted, we could also rename our `Wind deg` column to be `Wind dir` again


```python
indexed_weather_observations =\
            indexed_weather_observations.rename(columns = {'Wind deg':'Wind dir'})

indexed_weather_observations.head()
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
      <th>Wind dir</th>
      <th>Wind spd</th>
      <th>Wind gust</th>
      <th>Tmp</th>
      <th>Dew pt</th>
      <th>Feels like</th>
      <th>rh</th>
      <th>Fire</th>
      <th>Rain</th>
      <th>Rain 10'</th>
      <th>Pres</th>
    </tr>
    <tr>
      <th>Datetime</th>
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
      <th>2013-01-01 00:00:00</th>
      <td>67.5</td>
      <td>2.0</td>
      <td>6.0</td>
      <td>15.9</td>
      <td>3.7</td>
      <td>15.9</td>
      <td>44.0</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.1</td>
    </tr>
    <tr>
      <th>2013-01-01 00:30:00</th>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>14.5</td>
      <td>4.7</td>
      <td>14.5</td>
      <td>52.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1014.0</td>
    </tr>
    <tr>
      <th>2013-01-01 01:00:00</th>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>12.8</td>
      <td>4.6</td>
      <td>12.8</td>
      <td>57.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1013.5</td>
    </tr>
    <tr>
      <th>2013-01-01 01:30:00</th>
      <td>202.5</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>13.3</td>
      <td>5.9</td>
      <td>13.3</td>
      <td>61.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1013.4</td>
    </tr>
    <tr>
      <th>2013-01-01 02:00:00</th>
      <td>225.0</td>
      <td>2.0</td>
      <td>7.0</td>
      <td>12.5</td>
      <td>4.7</td>
      <td>12.5</td>
      <td>59.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1013.2</td>
    </tr>
  </tbody>
</table>
</div>



#### Challenge

Create a new column "E" for our toy dataframe, df. "E" will contain the values from column B + 100. 

_Hint 1: You can test your output by using the "apply" function without assigning it to a new column first_

_Hint 2: you can create your own functions using "lambda". e.g. _`apply(lambda x: ` _`insert stuff with x`_`)`



```python

```


```python

```

### Changing Data Frequency

If you delve into your data, you might occasionally see some timestamps that don't follow the typical half-hour format


```python
# One section where the data has weird timestamps ...
indexed_weather_observations[1800:1806]
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
      <th>Wind dir</th>
      <th>Wind spd</th>
      <th>Wind gust</th>
      <th>Tmp</th>
      <th>Dew pt</th>
      <th>Feels like</th>
      <th>rh</th>
      <th>Fire</th>
      <th>Rain</th>
      <th>Rain 10'</th>
      <th>Pres</th>
    </tr>
    <tr>
      <th>Datetime</th>
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
      <th>2013-02-04 00:30:00</th>
      <td>67.5</td>
      <td>9.0</td>
      <td>15.0</td>
      <td>15.1</td>
      <td>12.6</td>
      <td>15.1</td>
      <td>85.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1020.2</td>
    </tr>
    <tr>
      <th>2013-02-04 00:33:00</th>
      <td>67.5</td>
      <td>7.0</td>
      <td>11.0</td>
      <td>15.0</td>
      <td>12.7</td>
      <td>15.0</td>
      <td>86.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1020.2</td>
    </tr>
    <tr>
      <th>2013-02-04 01:00:00</th>
      <td>67.5</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>14.9</td>
      <td>12.8</td>
      <td>14.9</td>
      <td>87.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1020.2</td>
    </tr>
    <tr>
      <th>2013-02-04 01:11:00</th>
      <td>45.0</td>
      <td>4.0</td>
      <td>7.0</td>
      <td>15.0</td>
      <td>12.9</td>
      <td>15.0</td>
      <td>87.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1020.1</td>
    </tr>
    <tr>
      <th>2013-02-04 01:30:00</th>
      <td>67.5</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>15.4</td>
      <td>12.9</td>
      <td>15.4</td>
      <td>85.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1020.0</td>
    </tr>
    <tr>
      <th>2013-02-04 02:00:00</th>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>14.7</td>
      <td>13.1</td>
      <td>14.7</td>
      <td>90.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1019.9</td>
    </tr>
  </tbody>
</table>
</div>



Another function, `df.asfreq()`, allows you to force a frequency on your index, and will discard/fill in the rest


```python
# Force the index to be every 30 minutes
regular_observations = indexed_weather_observations.asfreq('30min')

print(regular_observations.shape) # we've now deleted ~ 2000 rows

# Same section we observed earlier
regular_observations[1633:1638]
```

    (17520, 11)
    




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
      <th>Wind dir</th>
      <th>Wind spd</th>
      <th>Wind gust</th>
      <th>Tmp</th>
      <th>Dew pt</th>
      <th>Feels like</th>
      <th>rh</th>
      <th>Fire</th>
      <th>Rain</th>
      <th>Rain 10'</th>
      <th>Pres</th>
    </tr>
    <tr>
      <th>Datetime</th>
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
      <th>2013-02-04 00:30:00</th>
      <td>67.5</td>
      <td>9.0</td>
      <td>15.0</td>
      <td>15.1</td>
      <td>12.6</td>
      <td>15.1</td>
      <td>85.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1020.2</td>
    </tr>
    <tr>
      <th>2013-02-04 01:00:00</th>
      <td>67.5</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>14.9</td>
      <td>12.8</td>
      <td>14.9</td>
      <td>87.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1020.2</td>
    </tr>
    <tr>
      <th>2013-02-04 01:30:00</th>
      <td>67.5</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>15.4</td>
      <td>12.9</td>
      <td>15.4</td>
      <td>85.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1020.0</td>
    </tr>
    <tr>
      <th>2013-02-04 02:00:00</th>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>14.7</td>
      <td>13.1</td>
      <td>14.7</td>
      <td>90.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1019.9</td>
    </tr>
    <tr>
      <th>2013-02-04 02:30:00</th>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>14.6</td>
      <td>13.0</td>
      <td>14.6</td>
      <td>90.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1019.9</td>
    </tr>
  </tbody>
</table>
</div>




```python

```


```python

```

### Dealing with Missing Data

- finding missing data
- interpolating missing data
- data interrogation with the `groupby` function

Often we want to know where and if we have any missing data in our dataset

A rudimentary way of doing this is with `counts`. `counts` will give you the number of **non-null** values within each column. Therefore, if your columns are shorter than the number of rows, you have nulls


```python
regular_observations.count()
```




    Wind dir      15861
    Wind spd      17372
    Wind gust     17192
    Tmp           17379
    Dew pt        17365
    Feels like    17379
    rh            17365
    Fire          17348
    Rain          16780
    Rain 10'      17327
    Pres          17379
    dtype: int64



So we can see here, none of our columns contain all 17520 values, with the most significant number of nulls being found in "Wind dir"

As seen when applying conditional statements before, there is also the `isnull()` function, which returns a list of True/False values. This can be used to subset your data to find the null rows, or to count how many there are in specific columns


```python
regular_observations["Wind dir"].isnull().sum()
```




    1659



You can also show *every* row with null values at once using the isnull().any() chained operation. any() contains the optional parameter "axis", which allows you to choose whether it operates on the rows or the columns. By default axis = 0, which gives you columns, but setting axis = 1 checks over the rows instead


```python
# will give the total number of rows that contain nulls
regular_observations.isnull().any(axis = 1)
```




    Datetime
    2013-01-01 00:00:00    False
    2013-01-01 00:30:00     True
    2013-01-01 01:00:00     True
    2013-01-01 01:30:00    False
    2013-01-01 02:00:00    False
    2013-01-01 02:30:00    False
    2013-01-01 03:00:00    False
    2013-01-01 03:30:00    False
    2013-01-01 04:00:00    False
    2013-01-01 04:30:00    False
    2013-01-01 05:00:00     True
    2013-01-01 05:30:00     True
    2013-01-01 06:00:00     True
    2013-01-01 06:30:00     True
    2013-01-01 07:00:00    False
    2013-01-01 07:30:00     True
    2013-01-01 08:00:00    False
    2013-01-01 08:30:00    False
    2013-01-01 09:00:00    False
    2013-01-01 09:30:00    False
    2013-01-01 10:00:00    False
    2013-01-01 10:30:00    False
    2013-01-01 11:00:00    False
    2013-01-01 11:30:00    False
    2013-01-01 12:00:00    False
    2013-01-01 12:30:00    False
    2013-01-01 13:00:00    False
    2013-01-01 13:30:00    False
    2013-01-01 14:00:00    False
    2013-01-01 14:30:00    False
                           ...  
    2013-12-31 09:00:00    False
    2013-12-31 09:30:00    False
    2013-12-31 10:00:00    False
    2013-12-31 10:30:00    False
    2013-12-31 11:00:00    False
    2013-12-31 11:30:00    False
    2013-12-31 12:00:00    False
    2013-12-31 12:30:00    False
    2013-12-31 13:00:00    False
    2013-12-31 13:30:00    False
    2013-12-31 14:00:00    False
    2013-12-31 14:30:00    False
    2013-12-31 15:00:00    False
    2013-12-31 15:30:00    False
    2013-12-31 16:00:00    False
    2013-12-31 16:30:00    False
    2013-12-31 17:00:00    False
    2013-12-31 17:30:00    False
    2013-12-31 18:00:00    False
    2013-12-31 18:30:00    False
    2013-12-31 19:00:00    False
    2013-12-31 19:30:00    False
    2013-12-31 20:00:00    False
    2013-12-31 20:30:00    False
    2013-12-31 21:00:00    False
    2013-12-31 21:30:00    False
    2013-12-31 22:00:00    False
    2013-12-31 22:30:00    False
    2013-12-31 23:00:00    False
    2013-12-31 23:30:00    False
    Freq: 30T, Length: 17520, dtype: bool




```python
# Will give True/False values for whether a column contains nulls
regular_observations.isnull().any(axis = 0)
```




    Wind dir      True
    Wind spd      True
    Wind gust     True
    Tmp           True
    Dew pt        True
    Feels like    True
    rh            True
    Fire          True
    Rain          True
    Rain 10'      True
    Pres          True
    dtype: bool



Similarly, if we plot our data we can see that we have a significant portion of data missing


```python
# Make the graphs a bit prettier
# pd.set_option('display.mpl_style', 'default') 
# plt.rcParams['figure.figsize'] = (18, 5)

regular_observations[['Wind spd', 'Wind gust', 'Tmp', 'Feels like']][:500].plot()
```

    C:\Users\Kahli\Anaconda2\envs\python36\lib\site-packages\IPython\core\interactiveshell.py:2881: FutureWarning: 
    mpl_style had been deprecated and will be removed in a future version.
    Use `matplotlib.pyplot.style.use` instead.
    
      exec(code_obj, self.user_global_ns, self.user_ns)
    




    <matplotlib.axes._subplots.AxesSubplot at 0x1a1a7cbf4e0>




![png](output_128_2.png)


As we were seeing while tracking the null values, we can see that there are a number of values missing even just in January

This can be due to errors in the sensors, or because of maintainence, etc. However if we wanted to model all of this data, this data needs to be complete

#### Interpolating Missing Data

One option is to use the function `Series.interpolate` to predict and fill in the missing values based on the index

If we examine the function more closely, we can see that it can take multiple arguments, including the prediction method (default = linear), and the direction in which it can replace your data


```python
help(df.interpolate)
```

    Help on method interpolate in module pandas.core.generic:
    
    interpolate(method='linear', axis=0, limit=None, inplace=False, limit_direction='forward', downcast=None, **kwargs) method of pandas.core.frame.DataFrame instance
        Interpolate values according to different methods.
        
        Please note that only ``method='linear'`` is supported for
        DataFrames/Series with a MultiIndex.
        
        Parameters
        ----------
        method : {'linear', 'time', 'index', 'values', 'nearest', 'zero',
                  'slinear', 'quadratic', 'cubic', 'barycentric', 'krogh',
                  'polynomial', 'spline', 'piecewise_polynomial',
                  'from_derivatives', 'pchip', 'akima'}
        
            * 'linear': ignore the index and treat the values as equally
              spaced. This is the only method supported on MultiIndexes.
              default
            * 'time': interpolation works on daily and higher resolution
              data to interpolate given length of interval
            * 'index', 'values': use the actual numerical values of the index
            * 'nearest', 'zero', 'slinear', 'quadratic', 'cubic',
              'barycentric', 'polynomial' is passed to
              ``scipy.interpolate.interp1d``. Both 'polynomial' and 'spline'
              require that you also specify an `order` (int),
              e.g. df.interpolate(method='polynomial', order=4).
              These use the actual numerical values of the index.
            * 'krogh', 'piecewise_polynomial', 'spline', 'pchip' and 'akima'
              are all wrappers around the scipy interpolation methods of
              similar names. These use the actual numerical values of the
              index. For more information on their behavior, see the
              `scipy documentation
              <http://docs.scipy.org/doc/scipy/reference/interpolate.html#univariate-interpolation>`__
              and `tutorial documentation
              <http://docs.scipy.org/doc/scipy/reference/tutorial/interpolate.html>`__
            * 'from_derivatives' refers to BPoly.from_derivatives which
              replaces 'piecewise_polynomial' interpolation method in
              scipy 0.18
        
            .. versionadded:: 0.18.1
        
               Added support for the 'akima' method
               Added interpolate method 'from_derivatives' which replaces
               'piecewise_polynomial' in scipy 0.18; backwards-compatible with
               scipy < 0.18
        
        axis : {0, 1}, default 0
            * 0: fill column-by-column
            * 1: fill row-by-row
        limit : int, default None.
            Maximum number of consecutive NaNs to fill. Must be greater than 0.
        limit_direction : {'forward', 'backward', 'both'}, default 'forward'
            If limit is specified, consecutive NaNs will be filled in this
            direction.
        
            .. versionadded:: 0.17.0
        
        inplace : bool, default False
            Update the NDFrame in place if possible.
        downcast : optional, 'infer' or None, defaults to None
            Downcast dtypes if possible.
        kwargs : keyword arguments to pass on to the interpolating function.
        
        Returns
        -------
        Series or DataFrame of same shape interpolated at the NaNs
        
        See Also
        --------
        reindex, replace, fillna
        
        Examples
        --------
        
        Filling in NaNs
        
        >>> s = pd.Series([0, 1, np.nan, 3])
        >>> s.interpolate()
        0    0
        1    1
        2    2
        3    3
        dtype: float64
    
    


```python
# Some of the null values    
regular_observations[1633:1638]
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
      <th>Wind dir</th>
      <th>Wind spd</th>
      <th>Wind gust</th>
      <th>Tmp</th>
      <th>Dew pt</th>
      <th>Feels like</th>
      <th>rh</th>
      <th>Fire</th>
      <th>Rain</th>
      <th>Rain 10'</th>
      <th>Pres</th>
    </tr>
    <tr>
      <th>Datetime</th>
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
      <th>2013-02-04 00:30:00</th>
      <td>67.5</td>
      <td>9.0</td>
      <td>15.0</td>
      <td>15.1</td>
      <td>12.6</td>
      <td>15.1</td>
      <td>85.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1020.2</td>
    </tr>
    <tr>
      <th>2013-02-04 01:00:00</th>
      <td>67.5</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>14.9</td>
      <td>12.8</td>
      <td>14.9</td>
      <td>87.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1020.2</td>
    </tr>
    <tr>
      <th>2013-02-04 01:30:00</th>
      <td>67.5</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>15.4</td>
      <td>12.9</td>
      <td>15.4</td>
      <td>85.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1020.0</td>
    </tr>
    <tr>
      <th>2013-02-04 02:00:00</th>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>14.7</td>
      <td>13.1</td>
      <td>14.7</td>
      <td>90.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1019.9</td>
    </tr>
    <tr>
      <th>2013-02-04 02:30:00</th>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>14.6</td>
      <td>13.0</td>
      <td>14.6</td>
      <td>90.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1019.9</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Interpolate data to fill empty values
for column in regular_observations.columns:
    regular_observations[column].interpolate('time', inplace=True)
```


```python
# The null values have been replaced    
regular_observations[1633:1638]
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
      <th>Wind dir</th>
      <th>Wind spd</th>
      <th>Wind gust</th>
      <th>Tmp</th>
      <th>Dew pt</th>
      <th>Feels like</th>
      <th>rh</th>
      <th>Fire</th>
      <th>Rain</th>
      <th>Rain 10'</th>
      <th>Pres</th>
    </tr>
    <tr>
      <th>Datetime</th>
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
      <th>2013-02-04 00:30:00</th>
      <td>67.5</td>
      <td>9.0</td>
      <td>15.0</td>
      <td>15.1</td>
      <td>12.6</td>
      <td>15.1</td>
      <td>85.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1020.2</td>
    </tr>
    <tr>
      <th>2013-02-04 01:00:00</th>
      <td>67.5</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>14.9</td>
      <td>12.8</td>
      <td>14.9</td>
      <td>87.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1020.2</td>
    </tr>
    <tr>
      <th>2013-02-04 01:30:00</th>
      <td>67.5</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>15.4</td>
      <td>12.9</td>
      <td>15.4</td>
      <td>85.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1020.0</td>
    </tr>
    <tr>
      <th>2013-02-04 02:00:00</th>
      <td>67.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>14.7</td>
      <td>13.1</td>
      <td>14.7</td>
      <td>90.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1019.9</td>
    </tr>
    <tr>
      <th>2013-02-04 02:30:00</th>
      <td>67.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>14.6</td>
      <td>13.0</td>
      <td>14.6</td>
      <td>90.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1019.9</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Plot it again - to be sure
regular_observations[['Wind spd', 'Wind gust', 'Tmp', 'Feels like']][:500].plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a1a8ccc7f0>




![png](output_135_1.png)


No gaps!

If you didn't want to predict values, there are a few other options you have to resolve nulls.

One option, provided you don't require a consecutive sequence of data, is to delete your null observations.

This can be easily done using the `dropna()` function

`dropna()` can work on either the rows or the columns, and allows you to specify whether you want to remove rows/column where either 'any' or 'all' of the data are nulls. Alternatively, you can set a threshold value, where rows/columns are discarded if they don't have at least a threshold # of non-null values


```python
temp_df = indexed_weather_observations.dropna(axis = 0, thresh= 5)
```


```python
print(indexed_weather_observations.shape)

temp_df.shape #As we can see, only 11 rows had < 5 values in their row
```

    (19556, 11)
    




    (19545, 11)



Otherwise, we could replace all the null values in each column with an appropriate value. While this wouldn't be appropriate for this dataset, there are many where a default value would be appropriate. You can do this with a method called `fillna()`


```python
df
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>49</th>
      <td>a</td>
      <td>54.0</td>
      <td>1.118139</td>
      <td>2.6</td>
      <td>154.0</td>
    </tr>
    <tr>
      <th>48</th>
      <td>b</td>
      <td>67.0</td>
      <td>-1.293534</td>
      <td>NaN</td>
      <td>167.0</td>
    </tr>
    <tr>
      <th>47</th>
      <td>c</td>
      <td>89.0</td>
      <td>-0.490262</td>
      <td>8.0</td>
      <td>189.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>d</td>
      <td>100.0</td>
      <td>0.905747</td>
      <td>9.4</td>
      <td>200.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>e</td>
      <td>NaN</td>
      <td>-0.869318</td>
      <td>3.3</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>f</td>
      <td>64.0</td>
      <td>0.146700</td>
      <td>NaN</td>
      <td>164.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df["E"] = df["E"].fillna(value = 0)

df
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>49</th>
      <td>a</td>
      <td>54.0</td>
      <td>1.118139</td>
      <td>2.6</td>
      <td>154.0</td>
    </tr>
    <tr>
      <th>48</th>
      <td>b</td>
      <td>67.0</td>
      <td>-1.293534</td>
      <td>NaN</td>
      <td>167.0</td>
    </tr>
    <tr>
      <th>47</th>
      <td>c</td>
      <td>89.0</td>
      <td>-0.490262</td>
      <td>8.0</td>
      <td>189.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>d</td>
      <td>100.0</td>
      <td>0.905747</td>
      <td>9.4</td>
      <td>200.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>e</td>
      <td>NaN</td>
      <td>-0.869318</td>
      <td>3.3</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>f</td>
      <td>64.0</td>
      <td>0.146700</td>
      <td>NaN</td>
      <td>164.0</td>
    </tr>
  </tbody>
</table>
</div>



`fillna` also allows you to forward or backfill your dataframe with the "methods" argument.


```python
df["B"] = df["B"].fillna(method = 'ffill')
df
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>49</th>
      <td>a</td>
      <td>54.0</td>
      <td>1.118139</td>
      <td>2.6</td>
      <td>154.0</td>
    </tr>
    <tr>
      <th>48</th>
      <td>b</td>
      <td>67.0</td>
      <td>-1.293534</td>
      <td>NaN</td>
      <td>167.0</td>
    </tr>
    <tr>
      <th>47</th>
      <td>c</td>
      <td>89.0</td>
      <td>-0.490262</td>
      <td>8.0</td>
      <td>189.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>d</td>
      <td>100.0</td>
      <td>0.905747</td>
      <td>9.4</td>
      <td>200.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>e</td>
      <td>100.0</td>
      <td>-0.869318</td>
      <td>3.3</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>f</td>
      <td>64.0</td>
      <td>0.146700</td>
      <td>NaN</td>
      <td>164.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df["D"] = df["D"].fillna(method = 'bfill')
df
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>49</th>
      <td>a</td>
      <td>54.0</td>
      <td>1.118139</td>
      <td>2.6</td>
      <td>154.0</td>
    </tr>
    <tr>
      <th>48</th>
      <td>b</td>
      <td>67.0</td>
      <td>-1.293534</td>
      <td>8.0</td>
      <td>167.0</td>
    </tr>
    <tr>
      <th>47</th>
      <td>c</td>
      <td>89.0</td>
      <td>-0.490262</td>
      <td>8.0</td>
      <td>189.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>d</td>
      <td>100.0</td>
      <td>0.905747</td>
      <td>9.4</td>
      <td>200.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>e</td>
      <td>100.0</td>
      <td>-0.869318</td>
      <td>3.3</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>f</td>
      <td>64.0</td>
      <td>0.146700</td>
      <td>NaN</td>
      <td>164.0</td>
    </tr>
  </tbody>
</table>
</div>



### Grouping Data

It's often useful to be able to group the data within a column according to the data in another column. 

For example, you might wish to take the mean temperature for each month, or each year. We could do this in a complicated and time consuming way where you subset the data for each month, and then take the mean of the Tmp column...or we could just use the `groupby` function. `groupby` outputs a reformatted version of your data where all values associated with your "grouping factor" are taken together. You can then perform basic statistical tests (or plot) on this grouped output, and the tests will be performed within each of the "groups" you defined.

Going back to the previous example, say that we wanted to find the mean temperature seen in each month:


```python
# we use TimeGrouper because we're grouping based on datetime data
regular_observations[["Tmp","Feels like"]].groupby(pd.TimeGrouper(freq='M')).mean()
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
      <th>Tmp</th>
      <th>Feels like</th>
    </tr>
    <tr>
      <th>Datetime</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-31</th>
      <td>22.300941</td>
      <td>21.811290</td>
    </tr>
    <tr>
      <th>2013-02-28</th>
      <td>19.397061</td>
      <td>19.168824</td>
    </tr>
    <tr>
      <th>2013-03-31</th>
      <td>17.427789</td>
      <td>17.138105</td>
    </tr>
    <tr>
      <th>2013-04-30</th>
      <td>13.334618</td>
      <td>12.714687</td>
    </tr>
    <tr>
      <th>2013-05-31</th>
      <td>8.816767</td>
      <td>7.646741</td>
    </tr>
    <tr>
      <th>2013-06-30</th>
      <td>7.171111</td>
      <td>5.647014</td>
    </tr>
    <tr>
      <th>2013-07-31</th>
      <td>6.763844</td>
      <td>4.993952</td>
    </tr>
    <tr>
      <th>2013-08-31</th>
      <td>8.025067</td>
      <td>5.683569</td>
    </tr>
    <tr>
      <th>2013-09-30</th>
      <td>11.827535</td>
      <td>10.807118</td>
    </tr>
    <tr>
      <th>2013-10-31</th>
      <td>12.955712</td>
      <td>12.121035</td>
    </tr>
    <tr>
      <th>2013-11-30</th>
      <td>15.023403</td>
      <td>14.299063</td>
    </tr>
    <tr>
      <th>2013-12-31</th>
      <td>19.380007</td>
      <td>18.925672</td>
    </tr>
  </tbody>
</table>
</div>



You can also choose to group over multiple factors, such as by Month and Year together, by specifying a list for the `by` parameter within `groupby()`. The order of the values in this list matters, as `groupby` will first group your data based on the first factor, and THEN the second factor.


```python
# Grouping by month, then by day
#The average number of pedestrians 
regular_observations[["Wind spd", "Wind gust"]].groupby(by=\
                                            [pd.TimeGrouper(freq='M'),pd.TimeGrouper(freq='D')]).mean()
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
      <th>Wind spd</th>
      <th>Wind gust</th>
    </tr>
    <tr>
      <th>Datetime</th>
      <th>Datetime</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="30" valign="top">2013-01-31</th>
      <th>2013-01-01</th>
      <td>15.312500</td>
      <td>20.812500</td>
    </tr>
    <tr>
      <th>2013-01-02</th>
      <td>14.729167</td>
      <td>21.520833</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>13.062500</td>
      <td>19.208333</td>
    </tr>
    <tr>
      <th>2013-01-04</th>
      <td>12.395833</td>
      <td>17.833333</td>
    </tr>
    <tr>
      <th>2013-01-05</th>
      <td>12.979167</td>
      <td>20.041667</td>
    </tr>
    <tr>
      <th>2013-01-06</th>
      <td>10.145833</td>
      <td>15.291667</td>
    </tr>
    <tr>
      <th>2013-01-07</th>
      <td>10.916667</td>
      <td>16.354167</td>
    </tr>
    <tr>
      <th>2013-01-08</th>
      <td>25.875000</td>
      <td>37.395833</td>
    </tr>
    <tr>
      <th>2013-01-09</th>
      <td>20.187500</td>
      <td>28.500000</td>
    </tr>
    <tr>
      <th>2013-01-10</th>
      <td>10.937500</td>
      <td>16.875000</td>
    </tr>
    <tr>
      <th>2013-01-11</th>
      <td>13.791667</td>
      <td>19.145833</td>
    </tr>
    <tr>
      <th>2013-01-12</th>
      <td>12.270833</td>
      <td>17.708333</td>
    </tr>
    <tr>
      <th>2013-01-13</th>
      <td>12.395833</td>
      <td>17.416667</td>
    </tr>
    <tr>
      <th>2013-01-14</th>
      <td>17.041667</td>
      <td>24.729167</td>
    </tr>
    <tr>
      <th>2013-01-15</th>
      <td>9.520833</td>
      <td>14.604167</td>
    </tr>
    <tr>
      <th>2013-01-16</th>
      <td>11.145833</td>
      <td>16.479167</td>
    </tr>
    <tr>
      <th>2013-01-17</th>
      <td>12.083333</td>
      <td>17.312500</td>
    </tr>
    <tr>
      <th>2013-01-18</th>
      <td>17.281250</td>
      <td>25.250000</td>
    </tr>
    <tr>
      <th>2013-01-19</th>
      <td>16.145833</td>
      <td>22.583333</td>
    </tr>
    <tr>
      <th>2013-01-20</th>
      <td>14.062500</td>
      <td>19.822917</td>
    </tr>
    <tr>
      <th>2013-01-21</th>
      <td>10.812500</td>
      <td>16.208333</td>
    </tr>
    <tr>
      <th>2013-01-22</th>
      <td>14.770833</td>
      <td>20.906250</td>
    </tr>
    <tr>
      <th>2013-01-23</th>
      <td>16.312500</td>
      <td>23.270833</td>
    </tr>
    <tr>
      <th>2013-01-24</th>
      <td>13.416667</td>
      <td>19.510417</td>
    </tr>
    <tr>
      <th>2013-01-25</th>
      <td>10.031250</td>
      <td>14.937500</td>
    </tr>
    <tr>
      <th>2013-01-26</th>
      <td>16.437500</td>
      <td>23.895833</td>
    </tr>
    <tr>
      <th>2013-01-27</th>
      <td>10.875000</td>
      <td>15.812500</td>
    </tr>
    <tr>
      <th>2013-01-28</th>
      <td>12.916667</td>
      <td>17.958333</td>
    </tr>
    <tr>
      <th>2013-01-29</th>
      <td>15.458333</td>
      <td>20.895833</td>
    </tr>
    <tr>
      <th>2013-01-30</th>
      <td>15.604167</td>
      <td>22.125000</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th rowspan="30" valign="top">2013-12-31</th>
      <th>2013-12-02</th>
      <td>10.208333</td>
      <td>15.125000</td>
    </tr>
    <tr>
      <th>2013-12-03</th>
      <td>9.625000</td>
      <td>14.145833</td>
    </tr>
    <tr>
      <th>2013-12-04</th>
      <td>20.270833</td>
      <td>28.937500</td>
    </tr>
    <tr>
      <th>2013-12-05</th>
      <td>22.604167</td>
      <td>31.395833</td>
    </tr>
    <tr>
      <th>2013-12-06</th>
      <td>20.250000</td>
      <td>26.895833</td>
    </tr>
    <tr>
      <th>2013-12-07</th>
      <td>11.479167</td>
      <td>17.104167</td>
    </tr>
    <tr>
      <th>2013-12-08</th>
      <td>11.458333</td>
      <td>16.552083</td>
    </tr>
    <tr>
      <th>2013-12-09</th>
      <td>14.375000</td>
      <td>20.947917</td>
    </tr>
    <tr>
      <th>2013-12-10</th>
      <td>27.270833</td>
      <td>37.125000</td>
    </tr>
    <tr>
      <th>2013-12-11</th>
      <td>18.708333</td>
      <td>25.958333</td>
    </tr>
    <tr>
      <th>2013-12-12</th>
      <td>13.541667</td>
      <td>19.291667</td>
    </tr>
    <tr>
      <th>2013-12-13</th>
      <td>12.354167</td>
      <td>18.625000</td>
    </tr>
    <tr>
      <th>2013-12-14</th>
      <td>15.229167</td>
      <td>21.437500</td>
    </tr>
    <tr>
      <th>2013-12-15</th>
      <td>10.541667</td>
      <td>15.812500</td>
    </tr>
    <tr>
      <th>2013-12-16</th>
      <td>12.312500</td>
      <td>17.687500</td>
    </tr>
    <tr>
      <th>2013-12-17</th>
      <td>9.937500</td>
      <td>14.604167</td>
    </tr>
    <tr>
      <th>2013-12-18</th>
      <td>11.250000</td>
      <td>16.062500</td>
    </tr>
    <tr>
      <th>2013-12-19</th>
      <td>12.333333</td>
      <td>17.520833</td>
    </tr>
    <tr>
      <th>2013-12-20</th>
      <td>12.541667</td>
      <td>18.104167</td>
    </tr>
    <tr>
      <th>2013-12-21</th>
      <td>15.166667</td>
      <td>21.875000</td>
    </tr>
    <tr>
      <th>2013-12-22</th>
      <td>15.666667</td>
      <td>22.041667</td>
    </tr>
    <tr>
      <th>2013-12-23</th>
      <td>13.854167</td>
      <td>19.229167</td>
    </tr>
    <tr>
      <th>2013-12-24</th>
      <td>13.333333</td>
      <td>18.937500</td>
    </tr>
    <tr>
      <th>2013-12-25</th>
      <td>9.083333</td>
      <td>12.395833</td>
    </tr>
    <tr>
      <th>2013-12-26</th>
      <td>10.250000</td>
      <td>16.270833</td>
    </tr>
    <tr>
      <th>2013-12-27</th>
      <td>11.812500</td>
      <td>17.666667</td>
    </tr>
    <tr>
      <th>2013-12-28</th>
      <td>12.979167</td>
      <td>18.333333</td>
    </tr>
    <tr>
      <th>2013-12-29</th>
      <td>17.229167</td>
      <td>25.604167</td>
    </tr>
    <tr>
      <th>2013-12-30</th>
      <td>16.166667</td>
      <td>23.250000</td>
    </tr>
    <tr>
      <th>2013-12-31</th>
      <td>14.052083</td>
      <td>19.979167</td>
    </tr>
  </tbody>
</table>
<p>365 rows × 2 columns</p>
</div>



You can also chain together more commands, like max(), etc, to find more specific data


```python
# The maximum average monthly temperature 
regular_observations['Tmp'].groupby(by=pd.TimeGrouper("M")).mean().max()
```




    22.300940860215078




```python
# If i wanted to find which month that belonged to though....
max_temp = regular_observations['Tmp'].groupby(by=pd.TimeGrouper("M")).mean().max()

regular_observations["Tmp"].groupby(by=pd.TimeGrouper("M")).mean()[\
                                    regular_observations["Tmp"].groupby(by=pd.TimeGrouper("M")).mean() == max_temp]
```




    Datetime
    2013-01-31    22.300941
    Freq: M, Name: Tmp, dtype: float64



#### Challenge

Which month has the greatest standard deviation in temperature?


```python

```


```python

```


```python

```

## Plotting Data

Pandas has some in-built plotting functionality, that builds off of the traditional matplotlib library

We've already seen a bit of this with "plot"


```python
regular_observations[['Wind spd', 'Wind gust', 'Tmp', 'Feels like']][:500].plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a1ab0b6860>




![png](output_158_1.png)



```python
regular_observations[:500].plot(x = "Feels like", y = "Tmp")
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a1ac4fde80>




![png](output_159_1.png)



```python
 from pandas.plotting import scatter_matrix
```


```python
 scatter_matrix(regular_observations[:500], alpha=0.2, diagonal='kde')
```




    array([[<matplotlib.axes._subplots.AxesSubplot object at 0x000001A1AC4FA2E8>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B08CF4A8>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B092F128>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B0D84E48>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B0DE6198>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B0DE61D0>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B0EAADA0>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B0F20630>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B10A8358>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B1117B70>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B11846D8>],
           [<matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B11BEFD0>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B124AC88>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B125B400>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B131B668>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B137B470>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B13E8128>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B1445AC8>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B14C0DA0>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B1524D30>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B15646A0>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B15F2320>],
           [<matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B16032B0>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B16B8DA0>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B1711AC8>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B178A860>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B17EF160>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B1869438>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B18AF438>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B1928278>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B197B9E8>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B19B6F28>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B1A4B860>],
           [<matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B1A5DE80>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B1B1D518>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B1B7A320>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B1BE1F98>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B1C45978>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B1CBFD68>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B1D23BE0>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B1D64550>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B1DF41D0>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B1E04080>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B1EBBB70>],
           [<matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B1F1B978>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B1F8A630>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B1FE6FD0>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B2069400>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B20CB278>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B2107BA8>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B2196710>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B21A6F60>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B22650F0>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B1CE6208>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B230BB38>],
           [<matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B236A908>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B23DB5F8>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B2439F60>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B24B9198>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B251E128>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B2556940>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B25E5630>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1B24718D0>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BE4460F0>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BE49BE80>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BE50DB70>],
           [<matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BE573518>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BE5EA710>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BE64C6A0>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BE687EB8>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BE716BA8>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BE6D0828>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BE7E6668>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BE844438>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BE8B4128>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BE910A90>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BE989B70>],
           [<matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BE9ECB38>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BEA31390>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BEABD080>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BEA74358>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BEB859E8>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BEBE68D0>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BEC534A8>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BECB2F28>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BED2AE48>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BED6C978>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BEDE32E8>],
           [<matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BEE39D30>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BEE76208>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BEF15518>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BED7C208>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BEFD5898>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF03B240>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF0B5240>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF1182E8>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF1592E8>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF1DF7F0>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF1F3B00>],
           [<matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF2AF198>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF30D080>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF377C18>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF3DB6D8>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF4545F8>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF4B9780>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF4F95F8>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF580C88>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF59B160>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF64F630>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF6AE518>],
           [<matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF7210F0>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF77BB70>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF7F3A90>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF857C18>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF89CD30>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF928160>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF93CE48>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BF9F0AC8>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BFA4D9B0>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BFAC0588>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000001A1BFB25048>]], dtype=object)




![png](output_161_1.png)



```python
from pandas.plotting import andrews_curves

plt.figure()
andrews_curves(regular_observations[:500], 'Wind dir')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a1ca93b390>




![png](output_162_1.png)



```python
from pandas.plotting import parallel_coordinates

plt.figure()

parallel_coordinates(regular_observations[:500], 'Name')
```

You can see exampes of these and many more inside the pandas documentation, https://pandas.pydata.org/pandas-docs/stable/visualization.html


```python

```


```python

```

## Combining Datasets

We also have another dataset, called "Canberra Sky". 

#### Challenge

Read in the Canberra Sky file, as `sky_observations`, in an appropriate format


```python

```


```python

```


```python

```


```python

```

Looking at the "head" of the data, tt seems to have the same issues that our previous dataset originally had.


```python
# As before, remove duplicates and set index to datetime.
sky_observations.drop_duplicates('Datetime', keep='last', inplace=True)
sky_observations.sort_values('Datetime', inplace=True)
sky_observations.set_index('Datetime', inplace=True)

sky_observations.head()
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
      <th>Vis</th>
      <th>Cloud</th>
      <th>Current weather</th>
      <th>Past weather</th>
      <th>?</th>
    </tr>
    <tr>
      <th>Datetime</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-01 00:00:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2013-01-01 02:00:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2013-01-01 03:00:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2013-01-01 05:00:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2013-01-01 06:00:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



#### Challenge

Remove all of the rows in `sky_observations` that have no data in them. i.e. all observations for that timestamp are NaN

_Hint: remember the `dropna` function?_


```python

```


```python

```

Ok, now let's explore our data!


```python
# Display the inferred data types
sky_observations.dtypes
```




    Vis                float64
    Cloud               object
    Current weather    float64
    Past weather       float64
    ?                  float64
    dtype: object



A closer look at Cloud shows that, rather than restricting the column values to using a scale between 1 - 8, people have been typing in a fractional string 


```python
# Display the inferred data types
sky_observations.Cloud.unique()
```




    array(['8/8', '1/8', 'clear', '4/8', '6/8', '3/8', '7/8', nan, '2/8', '5/8'], dtype=object)



In the Cloud column, 'clear' means no clouds, and 'nan' represents "obscured", meaning that the visibility was too low to see cloud. In this case, we would take that as a null value

To convert this column into a numerical value, we can define our own function and then use `apply` just as we did before


```python
# Define a function to Change the 'Cloud' column to numerical values
def cloud_to_numeric(s):
    if s == 'clear' or pd.isnull(s):
        return 0
    else:
        # s[0] is the first str character, or the numeric value
        return int(s[0]) / 8.0 # because we want a scale between 0 and 1
```


```python
# Apply the function to every item and assign it back to the original dataframe
sky_observations['Cloud'] = \
    sky_observations['Cloud'].apply(cloud_to_numeric, convert_dtype=False).astype('float64')
sky_observations.head()
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
      <th>Vis</th>
      <th>Cloud</th>
      <th>Current weather</th>
      <th>Past weather</th>
      <th>?</th>
    </tr>
    <tr>
      <th>Datetime</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-03 03:00:00</th>
      <td>NaN</td>
      <td>1.000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2013-01-03 06:00:00</th>
      <td>NaN</td>
      <td>1.000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2013-01-03 09:00:00</th>
      <td>NaN</td>
      <td>0.125</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2013-01-05 09:00:00</th>
      <td>25000.0</td>
      <td>0.000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2013-01-05 18:00:00</th>
      <td>NaN</td>
      <td>0.500</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



We now have a cleaned dataset for `sky_observations` that only contain numeric values. 

If we wanted to combine this with our previous `regular_observations` dataset though, we can see that despite `sky_observations` being indexed over the same time period as `regular_observations`, we had to remove many more of the null values, and it now has far fewer rows.




```python
print(sky_observations.shape)

print(regular_observations.shape)
```

    (1316, 5)
    (17520, 11)
    

#### The `combine` function
Therefore if we wanted to add these two datasets together, we couldn't use the simple Column addition that we learned earlier.

To combine these wo datasets, we can therefore use the function `df1.combine(df2)`

In this particular instance, we only want to join the Cloud column to our `regular_observations` dataset


```python
# Join the two observations together
combined_observations = regular_observations.combine_first(sky_observations[['Cloud']])
combined_observations.head()
combined_observations[combined_observations.Cloud.notnull()][:10]
```

We have a lot of null values, but it joined it all together

#### Challenge

Try joining another column from `sky_observations` onto the `combined_observations` dataframe. Which column/s would be the most appropriate? Which would be the least? Why?


```python

```


```python

```


```python

```

## Summary

This session has taken you through the beginner's guide of how to visualise, clean and investigate your data, and given you the basic tools to query your own research data. 

You've learned how to:
* read in your .csv file in a proper structure
* sort your dataset
* reindex your data according to another column
* transforming data within columns by applying a function
* regulate data frequencies
* combine datasets with different frequencies
* interpolate, fill or remove null values
* create new dataframes and columns
* plotting your dataset


Python is a very versatile tool, and can go much further than what we've shown you today. Many packages are open source and being developed for a range of purpoess all the time. Scipy allows you to perform basic math and statistical tests for example, there are a host of packages related to investigating biological data.

Using a programming language allows you to investigate much larger data than you can do by hand or by eye, and the customisability of the plotting tools makes it ideal for generating useful and professional images  ideal for publication.

Follow us on Twitter to keep up-to-date on new and up-coming trainings
- [@kflekac](https://twitter.com/kflekac)
- [@ResBaz](https://twitter.com/resbaz)
- [@ResPlat](https://twitter.com/resplat)

There's also a reasonably new facebook group, called ["Data Wrangling with Python"](https://www.facebook.com/groups/797677037064561/) where you can post your python problems, cool things you've done, and stay apprised of new trainings coming up as well.

Also remember that if you're having problems, Research Platforms runs a weekly Hacky Hour, where anybody and everybody is able to enquire about coding and programming problems in a variety of tools. Every Thursday from 3-4pm, at the large table in Tsubu Bar.


```python

```
