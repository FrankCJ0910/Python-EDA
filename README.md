Since this task is all about analyzing a data sheet, both pandas and matplotlib libraries are imported.

``` python
  import pandas as pd
  import matplotlib.pyplot as plt
```
Next is to import the data sheet, initialy this line was used.

``` python
  SP = pd.read_csv("spotify-2023.csv")
```

But using this line gave an error where it could not read some of the data and it was advised to change the encoding scheme to "ISO-8859-1" instead of the default unicode scheme. Resulting in the new code:

``` python
  SP = pd.read_csv("spotify-2023.csv", encoding="ISO-8859-1")
```

To see if there are any values missing in the data sheet, I use the isnull() function to check for any missing values.

``` python
  SP.isnull().values.any()
```

To find the number of rows and columns in the data sheet, I use .shape()

``` python
  SP.shape()
```

Next to find the mean, meadian, and std of the column streams.mean() and their respective funciton were used

``` python
  SP['streams'].mean()
  SP['streams'].median()
  SP['streams'].std()
```

This gave out an error where it is said that the column of streams is not in numeric to fix this converted the column to numeric using .to_numeric

``` python
  SP['streams']=pd.to_numeric(SP['streams'],errors='coerce')
```

To graph songs released, a histogram was used. With x axis as the songs released and an interval of 30

``` python
  plt.figure(figsize=(12, 6))
  plt.hist(SP['released_year'],bins=30)
  plt.show()
```

![alt text](https://github.com/FrankCJ0910/Python-EDA/blob/main/Images/YearXSongs.png?raw=true)




