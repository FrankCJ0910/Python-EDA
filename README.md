This program aims to explore and analyze a spotify's data from 2023. <br>
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

Next to find the mean, meadian, and std of the column, the .describe function was used

``` python
  SP['streams'].describe()
```

This gave out an error where it is said that the column of streams is not in numeric to fix this converted the column to numeric using .to_numeric

``` python
  SP['streams']=pd.to_numeric(SP['streams'],errors='coerce')
```
This gives us the following values <br>
![alt text](https://github.com/FrankCJ0910/Python-EDA/blob/main/Images/STATS.png?raw=true)

Next is to graph songs released, a histogram was used. With x axis as the songs released and an interval of 30

``` python
  plt.figure(figsize=(12, 6))
  plt.hist(SP['released_year'],bins=30)
  plt.xlabel("Year")
  plt.ylabel("Released Songs")
  plt.title("Amount Of Songs Released Per Year")
  plt.show()
```
In the graph generated, we can see that around 2020 there was a boom in amount of songs released.<br>
![alt text](https://github.com/FrankCJ0910/Python-EDA/blob/main/Images/YearXSongs.png?raw=true)

Next we graph the amount of songs that are realesed by a certain number of artists

``` python
  plt.figure(figsize=(12, 6))
  plt.hist(SP['artist_count'],bins=16)
  plt.xlabel("Number of Artists")
  plt.ylabel("Number of Songs")
  plt.title("Amount Of Artists per Song")
  plt.show()
```
We see that majority of songs are made by 1 to 3 artist while any more are more rarer.<br>
![alt text](https://github.com/FrankCJ0910/Python-EDA/blob/main/Images/ArtistsXSongs.png?raw=true)

We then find the top streamed songs, using first the , .sort_values() functions then using .head() to display the first 5 rows

``` python
  SP.sort_values(by='streams', ascending=False).head()[['track_name', 'streams']]
```

We get these results <br>
![alt text](https://github.com/FrankCJ0910/Python-EDA/blob/main/Images/TopStreams.png?raw=true)

Next is to get top 5 occuring artists, where we use .value_counts() to count how many times an artist appears in the datasheet, then use .head() to display the first 5 values 

``` python
  SP['artist(s)_name'].value_counts().head()
```

We achieve this result <br>
![alt text](https://github.com/FrankCJ0910/Python-EDA/blob/main/Images/TopArtists.png?raw=true)

To see if the month has an effect of amount of songs that are released, we use the same way we graph the amount of songs of released per year

``` python
  plt.figure(figsize=(6,3))
  plt.hist(SP['released_month'],bins=12,edgecolor='black')
  plt.xlabel("Month")
  plt.ylabel("Number of Songs")
  plt.title("Number of Songs Released on a Specific Month")
  plt.show()
```
We see that the month of january and may have more song released compared to other months<br>

![alt text](https://github.com/FrankCJ0910/Python-EDA/blob/main/Images/MonthXSongs.png?raw=true)

To look for which musical attribute has the highest correlation, we use subplots to make multiple graphs side to side

``` python
  fig, ax = plt.subplots(2, 3, figsize=(15, 10))

  ax[0,0].bar(SP['danceability_%'],SP['streams'],color='y')
  ax[0,0].set_title('Danceability VS Streams')
  ax[0,0].set_xlabel('Danceability (%)')
  ax[0,0].set_ylabel('Streams')
  #... repeat for other graphs

  #turn off the last plot since we only need 5
  ax[1,2].axis('off')
```
Looking at the graphs both acousitcness and energy influences the amount of streams a song receives with lower acousticness having more streams and higher energy having more streams <br>
![alt text](https://github.com/FrankCJ0910/Python-EDA/blob/main/Images/MusicStatsXStreams.png?raw=true)

For the correlation between Energy and danceability to streams, we use both a scatter plot and heat map. The plot showing Enervgy vs Danceavbility and the heat map showing the amount of streams. We also creat a color bar to be able to read the heat map.

``` python
  plt.figure(figsize=(10, 6))
  scatter = plt.scatter(SP['danceability_%'], SP['energy_%'] , c=SP['streams'], cmap='Blues',s=50)

  cbar = plt.colorbar(scatter)
  cbar.set_label('Streams')

  plt.title('Energy vs Danceability with Streams as Heatmap')
  plt.xlabel('Danceability (%)')
  plt.ylabel('Energy (%)')
```
From the graph we can see that higher energy songs with 60% to 80% danceability receives more streams <br>
![alt text](https://github.com/FrankCJ0910/Python-EDA/blob/main/Images/EDS.png?raw=true)

Repeating the same step but with different variables, we can see that really low acousticness % songs with around 50% valence receives more streams <br>
![alt text](https://github.com/FrankCJ0910/Python-EDA/blob/main/Images/VAS.png?raw=true)

Using the same method as the comparison of musical attributes to streams, we can compare the amount popular songs are in playlists of different apps. For spotify and apple, both have more popular songs in a lof of their playlists while deezer has lesser popular songs in more of their playlists.<br>
![alt text](https://github.com/FrankCJ0910/Python-EDA/blob/main/Images/PlaylistsXStreams.png?raw=true)

Next is to compare the average number of streams a major or a minor key song receives. Using the .loc() function to find the required modes and getting their mean, we then graph them.

``` python
  Majorcount = SP.loc[SP['mode']=='Major','streams'].mean()
  Minorcount = SP.loc[SP['mode']=='Minor','streams'].mean()

  plt.figure(figsize=(2, 3))
  plt.bar(['Major','Minor'],[Majorcount,Minorcount])
  plt.ylabel('Streams')
  plt.title('Major VS Minor Streams')
```
From the graph we can see that songs in a Major key have slightly more streams than songs in minor keys.<br>
![alt text](https://github.com/FrankCJ0910/Python-EDA/blob/main/Images/Mvm.png?raw=true)

Lastly to see how many times an artist appear in a playlist, we first group them by their artist names using .groupby(), then use .sum() to sum up all of the playlists the artist appear in spotify for now, next is to reset the index then sort their values by how many spotify playlist they are in. After that repeat it again for apple playlists, then merge the two data frames together. and lastly create a new column totaling the two other columns together.

``` python
  ASCC = SP.groupby('artist(s)_name')['in_spotify_charts'].sum().reset_index().sort_values(by='in_spotify_charts', ascending=False)
  AACC = SP.groupby('artist(s)_name')['in_apple_charts'].sum().reset_index().sort_values(by='in_apple_charts', ascending=False)
  AAC = pd.merge(ASCC,AACC,on='artist(s)_name')
  AAC['total_charts']=AAC['in_spotify_charts']+AAC['in_apple_charts']
```

After all that we get this result <br>
![alt text](https://github.com/FrankCJ0910/Python-EDA/blob/main/Images/APC.png?raw=true)

Repeat again for chart results <br>
![alt text](https://github.com/FrankCJ0910/Python-EDA/blob/main/Images/ACC.png?raw=true)
