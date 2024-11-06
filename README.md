# Spotify Exploratory Data Analysis on Most Streamed Song of 2023

  ![baby-imissyouuu](https://github.com/user-attachments/assets/8d4073ea-533b-47b9-983c-ad73906d21a1)


## Overview
#### This repository shows an interpretation and visualization of the data provided in the `spotify-2023.csv` file for better understanding between the relationship of different data which can provide an insight on what type of song can garner more streams.

## Dependencies and Libraries Used
```python
import pandas as pd
import numpy as np
import matplotlib.pylab as plt
import seaborn as sns
```

## Guide Questions
### Overview of Dataset
```
How many rows and columns does the dataset contain?
What are the data types of each column?
Are there any missing values?
```
### Basic Descriptive Statistics
```
What are the mean, median, and standard deviation of the streams column?
What is the distribution of released_year and artist_count?
Are there any noticeable trends or outliers?
```
### Top Performers
```
Which track has the highest number of streams? Display the top 5 most streamed tracks.
Who are the top 5 most frequent artists based on the number of tracks in the dataset?
```
### Temporal Trends
```
Analyze the trends in the number of tracks released over time. Plot the number of tracks released per year.
Does the number of tracks released per month follow any noticeable patterns? Which month sees the most releases?
```
### Genre and Music Characteristics
```
Examine the correlation between streams and musical attributes like bpm, danceability_%, and energy_%.
Which attributes seem to influence streams the most?
Is there a correlation between danceability_% and energy_%? How about valence_% and acousticness_%?
```
### Platform Popularity
```
How do the numbers of tracks in spotify_playlists, spotify_charts, and apple_playlists compare?
Which platform seems to favor the most popular tracks?
```
### Advanced Analysis
```
Based on the streams data, can you identify any patterns among tracks with the same key or mode (Major vs. Minor)?
Do certain genres or artists consistently appear in more playlists or charts?
Perform an analysis to compare the most frequently appearing artists in playlists or charts.
```

# Code Execution and Results
## Cleaning the CSV File
```python
#Loading the csv file
df = pd.read_csv('spotify-2023.csv', encoding='latin1')
df.head()
```
<p align="center">
  <img width="1373" alt="1" src="https://github.com/user-attachments/assets/b2407d40-346d-4634-bff2-0eeb7cbe3777">
</p>

```python
#Checking the data for any null values
df.isnull().sum()
```
<p align="center">
  <img width="225" alt="2" src="https://github.com/user-attachments/assets/bb1e7bc2-ba6b-4ace-b9e0-7d060ef16080">
</p>

```python
#Checking each column's data type
df.dtypes
```
<p align="center">
  <img width="225" alt="3" src="https://github.com/user-attachments/assets/c9fdc10c-35ed-4950-b136-4970101c51e7">
</p>

```python
#Checking inital rows and columns before cleaning the data
df.shape
```
<p align="center">
  <img width="300" alt="4" src="https://github.com/user-attachments/assets/03df9ed5-f133-4334-ba32-79c7db402e42">
</p>

```python
#Upon checking the data types of each column, it is noted that columns 'streams', 'in_deezer_playlists' and'in_shazam_charts' are of data type object
#Since in_deezer_playlists and in_shazam_charts are object data type due to commas, remove the commas using replace function
df['in_deezer_playlists']=df['in_deezer_playlists'].str.replace(',','')
df['in_shazam_charts']=df['in_shazam_charts'].str.replace(',','')
df.head()
```
<p align="center">
  <img width="1373" alt="5" src="https://github.com/user-attachments/assets/1087a958-8e20-44b6-b3b0-b363ce88654c">
</p>

```python
#However removing the commas does not change its data type
#To change the data type use 'pd.to_numeric' for the string data type to float in the 'streams' column, and .astype(float) to convert the column to float data type
#Convert data type object to float
df['streams'] = pd.to_numeric(df['streams'], errors='coerce') #Converting streams column to float data type
df['in_deezer_playlists'] = df['in_deezer_playlists'].astype(float) #Converting 'in_deezer_playlists' to float data type
df['in_shazam_charts'] = df['in_shazam_charts'].astype(float) #Converting 'in_shazam_charts' to float data type
df.head()
```
<p align="center">
  <img width="1373" alt="6" src="https://github.com/user-attachments/assets/d06887bd-e674-46e9-b89e-dbea5edd6fdd">
</p>

```python
#Upon checking for the data type of each column again, 'streams', 'in_deezer_playlists' and 'in_shazam_charts' are now of data type float
df.dtypes
```
<p align="center">
  <img width="225" alt="7" src="https://github.com/user-attachments/assets/d955fc75-f42f-49a6-9b63-f16c60cf7fd6">
</p>

```python
#Checking for any new null values after converting data type to float
df.isnull().sum()
```
<p align="center">
  <img width="225" alt="8" src="https://github.com/user-attachments/assets/2d5e04f5-6c86-4ff1-ab40-007dbc2103c1">
</p>

```python
#Removing rows with duplicates in columns 'track_name' and 'artist(s)_name'
df = df.drop_duplicates(['track_name','artist(s)_name'])
df
#4 rows that have the same track_name and artist(s)_name have been removed, the dataframe now has a shape of 949 rows × 24 columns
```
<p align="center">
  <img width="1373" alt="9" src="https://github.com/user-attachments/assets/97883956-5266-4bbb-88a0-7bd4971e4a9e">
</p>

```python
#Removing all rows with null values 
clean= df.dropna()
clean
#The dataframe now has a shape of 813 rows × 24 columns after removing all rows will null values
```
<p align="center">
  <img width="1384" alt="10" src="https://github.com/user-attachments/assets/f10c295c-3c0a-40fc-b793-69dc118ca8b4">
</p>

```python
#Renaming columns for better readability
clean = clean.rename(columns={'track_name':'Track_name','artist(s)_name':'Artist','artist_count':'Artist_count','released_year':'Released_year',
                   'released_month':'Released_month','released_day':'Released_day','in_spotify_playlists':'Spotify_playlists',
                   'in_spotify_charts':'Spotify_charts','streams':'Streams','in_apple_playlists':'Apple_playlists','in_apple_charts':'Apple_charts',
                   'in_deezer_playlists':'Deezer_playlists','in_deezer_charts':'Deezer_charts','in_shazam_charts':'Shazam_charts',
                   'bpm':'BPM','key':'Key','mode':'Mode','danceability_%':'Danceability%','valence_%':'Valence%','energy_%':'Energy%',
                   'acousticness_%':'Acousticness%','instrumentalness_%':'Instrumentalness%','liveness_%':'Liveness%',
                   'speechiness_%':'Speechiness%'})

#Resetting index to match the shape of the dataframe
final = clean.reset_index(drop=True)
final
```
<p align="center">
  <img width="1206" alt="11" src="https://github.com/user-attachments/assets/0f4ca7a6-5a0a-4b61-abe1-81f4fd4fb38d">
</p>

## Overview of Dataset
```
- How many rows and columns does the dataset contain?

    The dataset had 953 rows × 24 columns before cleaning and 813 rows x 24 columns after cleaning.

- What are the data types of each column? Are there any missing values?

    Before cleaning the dataset, all columns are int64 data type except for 'track_name', 'artist(s)_name', 'streams',
  'in_deezer_playlists', 'in_shazam_charts', 'key', and 'mode' are of object data type. There were missing values in the
  'streams', 'in_shazam_charts', and 'key' column.

```

## Basic Descriptive Statistics
```python
#Using the .describe() function can give us basic descriptive statistics of each column
final.describe()
```
<p align="center">
  <img width="1126" alt="12" src="https://github.com/user-attachments/assets/9b43239d-6ab3-40e5-a07a-ab691ddfb752">
</p>

```python
#We can also calculate the mean, median, and standard deviation of the streams column using the .mean(), .median(), and .std() function respectively
print("The mean of streams is:", final['Streams'].mean()) #Printing the mean of 'Streams' column
print("The median of streams is:", final['Streams'].median()) #Printing the median of 'Streams' column
print("The standard deviation of streams is:", final['Streams'].std()) #Printing the standard deviation of 'Streams' column
```
<p align="center">
  <img width="550" alt="13" src="https://github.com/user-attachments/assets/557bc134-25ba-41ae-b577-a59f5a038d7c">
</p>
  
```python
#Setting the figure size of the plots
plt.figure(figsize=(12,5))

#Creating a subplot for distribution of tracks based on released year
plt.subplot(1,2,1) #First subplot
plt.hist(final['Released_year'], bins = 101, rwidth = 0.9, edgecolor = 'black', color = '#76ff7a') #Creating a histogram for tracks released per year
plt.title("Distribution of Tracks Based on Released Year") #Setting a title for the graph
plt.xlabel("Released year") #Setting x-label
plt.ylabel("Count") #Setting y-label
plt.grid(axis = 'y',linestyle = '--', linewidth = 0.5) #Adding a grid to the graph

#Creating a subplot for distribution of tracks based on artist count
plt.subplot(1,2,2) #Second subplot
plt.hist(final['Artist_count'], bins = 8,edgecolor = 'black', linewidth = 1, color = '#fb4f14') #Creating a histogram for count of songs based on the artist count of the song
plt.title("Distribution of Tracks Based on Artist Count") #Setting a title for the graph
plt.xlabel("Artist count") #Setting x-label
plt.ylabel("Count") #Setting y-label
plt.grid(axis = 'y',linestyle = '--', linewidth = 0.5) #Adding a grid to the graph 
plt.tight_layout()
```
<p align="center">
  <img width="626" alt="17" src="https://github.com/user-attachments/assets/d22f9fd1-482f-489b-9e9b-0471bee71ae3">
</p>

```python
#Creating a function for calculation of outliers
def findoutlier(final):
    q1=final.quantile(0.25)
    q3=final.quantile(0.75)
    iqr=q3-q1
    outliers = final[((final<(q1-1.5*iqr)) | (final>(q3+1.5*iqr)))].shape[0] #Shape[0] gives only the number of rows
    return outliers

yearoutliers = findoutlier(final['Released_year']) #Calculating the outliers for 'Released_year'
artistoutliers = findoutlier(final['Artist_count']) #Calculating the outliers for 'Artist_count'

print("The number of outliers in 'Released year column' is:",yearoutliers) #Printing outliers for 'Released_year' column
print("The number of outliers in 'Artist_count column' is:",artistoutliers) #Printing outliers for 'Artist_count' column
```
<p align="center">
  <img width="650" alt="14" src="https://github.com/user-attachments/assets/5b069e9a-a1c0-4879-b320-30658c09b805">
</p>

```
- What are the mean, median, and standard deviation of the streams column?

    The mean, median, and standard deviation of the streams column is:
  468922407.2521525, 263453310.0, and 523981505.32150424 respectively.

- What is the distribution of released_year and artist_count? Are there any noticeable trends or outliers?

    Since the data is for streams from 2023, it is only natural that songs released after 2020 would have the highest
  count. Songs with only 1 artist also had the most count. This is also natural as a large part of the data come from
  indie artists. There are 180 outliers in the 'Released Year' column and 24 outliers in the 'Artist_count' column.

```

## Top Performers
```python
#Sorting the cleaned data by number of streams from highest to lowest
#Using .head() to display only the first 5 rows
top5streams = final.sort_values(by = 'Streams', ascending = False).head().reset_index(drop=True)
top5streams
```
<p align="center">
  <img width="550" alt="topstreams" src="https://github.com/user-attachments/assets/7c26a640-8e82-4269-9c4d-229bdb309a1f">
</p>

```python
#Using .value_counts() function to count how many times each artist shows up in the 'Artist' column
#Using .head() function to display only the first 5 rows
#Using .reset_index() function to convert the data into a dataframe
top5frequent = final['Artist'].value_counts().head().reset_index()
top5frequent
```
<p align="center">
  <img width="300" alt="16" src="https://github.com/user-attachments/assets/6ba3dd13-6d9f-485a-b0e6-1942d52768d7">
</p>

```
- Which track has the highest number of streams? Display the top 5 most streamed tracks.

            The top 5 most streamed tracks is:
  1) "Shape of You" by Ed Sheeran with 3.562544e+09 streams
  2) "Sunflower" by Post Malone, Swae Lee with 2.808097e+09 streams
  3) "One Dance" by Drake, WizKid, Kyla with 2.713922e+09 streams
  4) "STAY" by Justin Bieber, The Kid Laroi	with 2.665344e+09
  5) "Believer" by Imagine Dragons with 2.594040e+09 streams

- Who are the top 5 most frequent artists based on the number of tracks in the dataset?

  Top 5 most frequent artists based on number of tracks is
                1) Taylor Swift with 29 tracks
                2) SZA with 17 tracks
                3) Bad Bunny with 16 tracks
                4) The Weeknd with 14 tracks
                5) Harry Styles with 12 tracks

```

## Temporal Trends
```python
#Getting how many tracks were released each year
trackperyear = final['Released_year'].value_counts()

#Setting values for x and y axis
tpyi = trackperyear.index #Index of trackperyear will be used for the x-axis
tpyv = trackperyear.values #Values of trackperyear will be the value of each year on the y-axis

#Setting graph details
plt.figure(figsize = (10,5))
plt.bar(tpyi, tpyv, width = 0.6, edgecolor='black',linewidth=1, color = '#76ff7a') #Using bar graph to show comparison of songs over time
plt.xticks(tpyi, rotation = 90, fontsize = 7) #Setting the label of each bar in the graph on the x-axis
plt.title("Number of Tracks Released per Year") #Setting a title for the graph
plt.xlabel("Year") #Setting x-label
plt.ylabel("Amount of songs") #Setting y-label
plt.grid(axis = 'y', linestyle = '--', linewidth = 0.5) #Adding a grid to the graph
plt.tight_layout()
plt.show()
```
<p align="center">
  <img width="565" alt="18new" src="https://github.com/user-attachments/assets/2ad48f9b-884e-424f-aaa7-bee401b06f43">
</p>

```python
#Getting how many tracks were released each month and sorting its index from 1-12 (January to December)
trackpermonth = final['Released_month'].value_counts().sort_index()

#Creating list for the x-axis of the graph
months = ['January','February','March','April','May','June','July','August','September','October','November','December']

#Setting values for x and y axis
tpmi = trackpermonth.index #Index of trackpermonth will be used for the x-axis
tpmv = trackpermonth.values #Values of trackpermonth will be used for the y-axis

#Setting graph details
plt.figure(figsize = (10,5))
plt.bar(tpmi, tpmv, width = 0.9, edgecolor='black',linewidth=0.7, color = '#32cd32') #Using bar graph to show comparison of songs over time
plt.xticks(tpmi, months, rotation = 90, fontsize = 10) #Changing then x-ticks to month names
plt.title("Number of Tracks per Month") #Setting a title for the graph
plt.xlabel("Month") #Setting an x-label
plt.ylabel("Amount of songs") #Setting a y-lael
plt.grid(axis = 'y',linestyle = '--', linewidth = 0.5) #Adding a grid to the grpah
plt.tight_layout()
plt.show()
```
<p align="center">
  <img width="569" alt="19new" src="https://github.com/user-attachments/assets/fa4c33b4-8042-4f3d-a56e-08c5697598e2">
</p>

```
- Does the number of tracks released per month follow any noticeable patterns? Which month sees the most releases?

    As seen from the graph, there is a spike in Jauary and May with May having more tracks by a small amount. There is
  no noticeable pattern in which month the track is released.

```

## Genre and Music Characteristics
```python
#Creating a matrix for correlation to be used for the heatmap
cor = final[['Streams','BPM','Danceability%','Energy%','Valence%','Acousticness%']]
hm = cor.corr()

#Setting graph details
plt.figure(figsize = (6,6))
sns.heatmap(hm, cmap = 'viridis', vmax = 1, vmin = -1, annot = True) #Using heatmap to show correlation between streams and musical attributes
plt.title("Correlation Between Streams and Musical Attributes") #Setting a title for the graph 
plt.show()
```
<p align="center">
  <img width="378" alt="20new" src="https://github.com/user-attachments/assets/f6c68ada-06e4-4e74-a4f7-5fb68f9fe25a">
</p>

```
- Examine the correlation between streams and musical attributes like bpm, danceability_%, and energy_%.
  Which attributes seem to influence streams the most?

    As seen from the generated heatmap, the correlation between Streams and BPM, Danceability%, Energy%, Valence%,
  Acousticness% are all negative and are very close to 0. This means that there is little to no relationship or no
  relationship at all between streams and musical attributes. However, even though by a small amount, Danceability%
  influences streams the most.

- Is there a correlation between danceability_% and energy_%? How about valence_% and acousticness_%?

    Danceability% and Energy% have a very weak correlation of 0.16. Similarly, Valence% and Acousticness%
  also have a very weak correlation of -0.062.

```

## Platform Popularity
```python
#Getting the amount of tracks in spotify_playlists, Deezer_playlists, and apple_playlists
playliststotal = [final['Spotify_playlists'].sum(),final['Deezer_playlists'].sum(),final['Apple_playlists'].sum()]

label = ['Spotify_playlists','Deezer_playlists','Apple_playlists'] #Creating a list for the label of the pie chart

color = ['#32cd32', '#9400d3', '#ff0000'] #Creating a list for the colors of the pie chart

#Setting graph details
plt.figure(figsize=(7,7))
plt.pie(playliststotal, labels = label, autopct = '%0.1f%%', colors = color, explode = [0.1, 0, 0], wedgeprops = {'edgecolor':'black', 'linewidth' : 0.6}) #Creating a pie chart for comparison between the different platforms
plt.title("Comparison of Tracks in Spotify, Apple, and Deezer Playlists") #Setting a title for the graph
plt.legend() #Adding a legend
plt.show()
```
<p align="center">
  <img width="446" alt="21" src="https://github.com/user-attachments/assets/1a2f439b-6a10-459c-a6f7-60018cff4832">
</p>  

```python
#Getting the amount of tracks in spotify_playlists, Deezer_playlists, and apple_playlists only from the top 5 songs based on streams
topplayliststotal = [top5streams['Spotify_playlists'].sum(),top5streams['Deezer_playlists'].sum(),top5streams['Apple_playlists'].sum()]

color = ['#32cd32', '#9400d3', '#ff0000'] #Creating a list for the colors of the pie chart

#Setting graph details
plt.figure(figsize=(7,7))
plt.pie(topplayliststotal,labels=label,autopct='%0.1f%%', colors = color, explode = [0.1, 0, 0], wedgeprops = {'edgecolor':'black', 'linewidth' : 0.6}) #Creating a pie chart for comparison between the different platforms
plt.title("Comparison of Top Tracks in Spotify, Apple, and Deezer Playlists") #Setting a title for the graph
plt.legend()
plt.show()
```
<p align="center">
  <img width="451" alt="22" src="https://github.com/user-attachments/assets/fa5424f3-66e7-456a-a933-4d9ac1a703de">
</p>

```
- How do the numbers of tracks in spotify_playlists, spotify_charts, and apple_playlists compare?
  Which platform seems to favor the most popular tracks?

    Spotify dominates over Deezer and Apple in both overall tracks and top 5 tracks.

```

## Advanced Analysis
```python
#Grouping data by minor mode and major mode and getting the sum of streams for each key
minor = final[final['Mode'] == 'Minor'].groupby(['Key'])['Streams'].sum()
major = final[final['Mode'] == 'Major'].groupby(['Key'])['Streams'].sum()

#Setting the data for the plot
x = np.arange(11) #This is needed for the x-axis labels of the graph
keys = minor.index.tolist() #The index made by the .groupby() function will replace the previously set x variable
minorbar = minor.values #Getting the values of the total streams of each key in minor mode for each bar on the graph
majorbar = major.values #Getting the values of the total streams of each key in major mode for each bar on the graph

#Setting plot details
plt.figure(figsize = (10, 6))

#Creating a grouped bar graph for better comparison of the number of streams by key and mode
plt.bar(x - 0.2, majorbar, width = 0.4, label = "Major", edgecolor = 'black',linewidth = 0.7, color = '#32cd32')
plt.bar(x + 0.2, minorbar, width = 0.4, label = "Minor", edgecolor = 'black', linewidth = 0.7, color = '#fb4f14') 
 
plt.title("Streams per Key and Mode") #Setting a title for the graph
plt.xticks(x, keys) #Changing np.arange(11) to the index of minor
plt.xlabel("Key") #Setting an x-label
plt.ylabel("Streams (x10^10)") #Setting a y-label
plt.legend(title = "Mode") #Adding a legend
plt.grid(axis = 'y',linestyle = '--', linewidth = 0.5) #Adding a grid to the graph
plt.tight_layout()
plt.show()
```
<p align="center">
  <img width="526" alt="23" src="https://github.com/user-attachments/assets/26270de2-4b7b-4a6d-9901-cd98f40120dc">
</p>

```
- Based on the streams data, can you identify any patterns among tracks with the same key or mode (Major vs. Minor)?

    From the bar graph generated, in both major and minor mode, the key of C# garnered the most streams. This can
  be due to the a high number of tracks having the key of C# or songs that are in the key of C# sound more pleasing
  compared to other keys.

```

```python
#Getting the index of the top 10 most frequent appearing artists and turning it into a list
topartists = final['Artist'].value_counts().head(10).index.tolist()

#Creating a dataframe only containing rows which have the name of the most frequent appearing artists based on the list 'topartists'
topdf = final[final['Artist'].isin(topartists)]

#Setting the data for the plot
topplaylists = topdf.groupby('Artist')[['Spotify_playlists', 'Deezer_playlists', 'Apple_playlists']].sum() #Creating a dataframe that sums each artist's 'Spotify_playlists', 'Deezer_playlists', and 'Apple_playlists'
topplaylists['Total playlists'] = topplaylists.sum(axis = 1) #Creating another column for the total playlists of the artist by summing the whole row
topplaylists = topplaylists.sort_values(by = 'Total playlists', ascending = False) #Sorting the dataframe by its 'Total playlists' column from highest to lowest

topcharts = topdf.groupby('Artist')[['Spotify_charts', 'Deezer_charts', 'Apple_charts', 'Shazam_charts']].sum() #Creating a dataframe that sums each artist's 'Spotify_charts', 'Deezer_charts', 'Apple_charts', 'Shazam_charts'
topcharts['Total charts'] = topcharts.sum(axis = 1) #Creating another column for the total charts of the artist by summing the whole row
topcharts = topcharts.reindex(topplaylists.index) #Sorting the dataframe to have same arranegemnt of index as topplaylists

#Setting the data for the plot
xnames = np.arange(10) #This is needed for the x axis labels of the graph
names = topplaylists.index.tolist() #The index made by the .groupby() function will replace the previously set x variable
playlistsbar = topplaylists['Total playlists'].values #Getting the values of the 'Total playlists' column from topplaylists for each bar on the graph
chartsbar = topcharts['Total charts'].values #Getting the values of the 'Total charts' column from topcharts for each bar on the graph

#Setting plot details
plt.figure(figsize = (10, 6))

#Creating a grouped bar graph for better comparison of playlists and charts of the top 10 frequent sppearing artists
plt.bar(xnames - 0.2, playlistsbar, width = 0.4, label = "Playlists", edgecolor = 'black',linewidth = 0.7, color = '#32cd32') 
plt.bar(xnames + 0.2, chartsbar, width = 0.4, label = "Charts", edgecolor = 'black', linewidth = 0.7, color = '#fb4f14') 

plt.title("Playlists and Charts of the Top 10 Frequently Appearing Artists") #Setting a title for the graph
plt.xticks(xnames, names, rotation = 45) #Changing np.arange(10) to the index of topplaylists
plt.xlabel("Artists") #Adding an x-label
plt.ylabel("Count") #Adding a y-label
plt.legend() #Adding a legend
plt.tight_layout()
plt.show()
```
<p align="center">
  <img width="546" alt="24" src="https://github.com/user-attachments/assets/d6f07da3-50fd-41e1-b2de-5fa1f2542e69">
</p>

```
- Do certain genres or artists consistently appear in more playlists or charts? Perform an analysis to compare the most
frequently appearing artists in playlists or charts.

  The top 10 frequently appearing artists all have much more playlists than charts.

```

## Insights
`Loading the csv file using the pd.read_csv() function was did not suffice due to the data having characters outside of the  ASCII range. To fix this, simply add encoding='latin1', encoding='iso-8859-1' or encoding='cp1252' at the end of the function.`

`Upon cleaning the data, it is found to have missing values (Null/NaN values), object data types in the columns 'streams', 'in_deezer_playlists' and 'in_shazam_charts' which should only have numeric values (int or float), and also 3 rows which have duplicates in the 'track_name' and 'artist(s)_name' column. All of these can be error in the collection of data, especially the object data types.`

`Upon visualizing the data for temporal trends, it can be deduced that there is no pattern in the month of release of the song. However, there is a noticable spike in teh streams of songs released in the years ranging 2019 - 2023. This is natural as the data is from streams during the year 2023. This indicates that the majority of users listen to more recent released songs.`

`From the generated heatmap of the correlation between streams and musical attributes, all correlations are close to 0 . This indicates that there is little to no correlation or no correlation at all between streams and any of the musical attributes.`

`Spotify dominated in both playlists and tracks of all the songs in the data. This can be due to the date being collected mainly from Spotify.`

`In all keys, major mode had more views than minor mode. This indicates that the majority of listeners have a preference to songs in major mode regardless of the key of the song.`

## Recommendations
`In the "Genre and Music Characteristics" section, it noteworthy that while the generated correlations from the heatmap are negative, this should be treated as positive as a song can't lose streams. The negative correlation can be due to the heatmap having a low value of -1 . One way to fix this is to change the low value to 0 so that no correlation will be negative.`

`In the "Advanced Analysis" section, since majority of the users are listening to recently released songs, it would be better to also have a comparison of the songs from the last 20 years and see which key and what mode is best tailored to the preferences of the users.`
## Author

#### Lorenzo G. Viacrucis
#### 2ECE-D

## References
* Rob Mulla. (2021, December 31). Exploratory Data Analysis with Pandas Python [Video]. YouTube.https://www.youtube.com/watch?v=xi0vhXFPegw
* Malli. (2024, March 28). Use pandas.to_numeric() Function. Spark by Examples. https://sparkbyexamples.com/pandas/use-pandas-to-numeric-function/#:~:text=Key%20Points%20%E2%80%93-,Pandas.,errors%2C%20coercion%2C%20and%20downcasting.
* GeeksforGeeks. (2024, June 26). How to rename columns in Pandas DataFrame.GeeksforGeeks.https://www.geeksforgeeks.org/how-to-rename-columns-in-pandas-dataframe/
* Kleppen, E. (2023, May 11). How to find outliers using Python [Step-by-Step guide]. CareerFoundry. https://careerfoundry.com/en/blog/data-analytics/how-to-find-outliers/#:~:text=Finding%20outliers%20using%20statistical%20methods,-Since%20the%20data&text=Using%20the%20IQR%2C%20the%20outlier,Q1%20(Q3%E2%80%93Q1).
* Malli. (2024b, March 28). Use pandas.to_numeric() Function. Spark by {Examples}. https://sparkbyexamples.com/pandas/use-pandas-to-numeric-function/#:~:text=Key%20Points%20%E2%80%93-,Pandas.,errors%2C%20coercion%2C%20and%20downcasting.
* GeeksforGeeks. (2024b, June 26). How to rename columns in Pandas DataFrame. GeeksforGeeks. https://www.geeksforgeeks.org/how-to-rename-columns-in-pandas-dataframe/
* Remove row with null value from pandas data frame. (n.d.). Stack Overflow. https://stackoverflow.com/questions/44548721/remove-row-with-null-value-from-pandas-data-frame
* W3Schools.com. (n.d.). https://www.w3schools.com/python/pandas/ref_df_drop_duplicates.asp#:~:text=The%20drop_duplicates()%20method%20removes,considered%20when%20looking%20for%20duplicates.
* How to remove commas from ALL the column in pandas at once. (n.d.). Stack Overflow. https://stackoverflow.com/questions/56947333/how-to-remove-commas-from-all-the-column-in-pandas-at-once
* freeCodeCamp. (2022, October 25). How the Python Lambda Function Works – Explained with Examples. freeCodeCamp.org. https://www.freecodecamp.org/news/python-lambda-function-explained/
* Kleppen, E. (2023b, May 11). How to find outliers using Python [Step-by-Step guide]. CareerFoundry. https://careerfoundry.com/en/blog/data-analytics/how-to-find-outliers/#:~:text=Finding%20outliers%20using%20statistical%20methods,-Since%20the%20data&text=Using%20the%20IQR%2C%20the%20outlier,Q1%20(Q3%E2%80%93Q1).
* Pandas .values_count() & .plot() | Python Analysis Tutorial - Mode. (2016, May 23). Mode Resources. https://mode.com/python-tutorial/counting-and-plotting-in-python
* Nelamali, N. (2024, October 15). Pandas convert float to integer in DataFrame. Spark by {Examples}. https://sparkbyexamples.com/pandas/pandas-convert-float-to-integer-type/#:~:text=Use%20astype()%20to%20convert,them%20and%20applying%20astype()%20.
* GeeksforGeeks. (2020, December 17). Create a grouped bar plot in Matplotlib. GeeksforGeeks. https://www.geeksforgeeks.org/create-a-grouped-bar-plot-in-matplotlib/
* Nelamali, N. (2024a, March 27). Convert Pandas index to list. Spark by {Examples}. https://sparkbyexamples.com/pandas/convert-pandas-index-to-list/#:~:text=To%20convert%20an%20index%20to,()%20and%20list()%20functions.
* Bhandari, A. (2024b, October 18). Pandas Groupby for data aggregation. Analytics Vidhya. https://www.analyticsvidhya.com/blog/2020/03/groupby-pandas-aggregating-data-python/
* How to Select Rows from a DataFrame Based on List Values in a Column in Pandas | Saturn Cloud Blog. (2023, October 28). https://saturncloud.io/blog/how-to-select-rows-from-a-dataframe-based-on-list-values-in-a-column-in-pandas/#:~:text=To%20select%20rows%20from%20a%20DataFrame%20based%20on%20a%20list,to%20select%20the%20desired%20rows.
* How to add a new column to an existing DataFrame. (n.d.). Stack Overflow. https://stackoverflow.com/questions/12555323/how-to-add-a-new-column-to-an-existing-dataframe
* W3Schools.com. (n.d.-b). https://www.w3schools.com/python/pandas/ref_df_reindex.asp#:~:text=The%20reindex()%20method%20allows,indexes%2C%20and%20the%20columns%20labels.&text=Note%3A%20The%20values%20are%20set,the%20same%20as%20the%20old.
