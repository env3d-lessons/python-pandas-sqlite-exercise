Dataset is from https://www.kaggle.com/datasets/shahjhanalam/movie-data-analytics-dataset

You can load the 3 tables in this database into python dataframes using the
following code:

```
import pandas
import numpy
import sqlite3

# You can directly access the database from codespaces
conn = sqlite3.connect('movie.sqlite')

genre_df = pandas.read_sql('select * from genre', conn)
earning_df = pandas.read_sql('select * from earning', conn)
imdb_df = pandas.read_sql('select * from IMDB', conn)
```

The above code reads the entire table into a dataframe object.  It works for
this small dataset but will not work if the sqlite file is really large.

Your job is to convert each of the dataframe manipulation into SQL query.

For example:

```
# Retireve all movie titles with IMDB Rating > 8.0
titles = imdb_df[ imdb_df['Rating'] > 8.0 ]['Title']

# Equivalent SQL version
titles = pandas.read_sql('select Title from IMDB where Rating > 8.0', conn)
```

# Exercises
```

# Retieve all titles with over 1 million votes
imdb_df[ imdb_df['TotalVotes'] > 1000000 ]['Title']

# Retrieve all titles that has both over 8.5 on IMDB Rating and over 1 million votes
imdb_df[ (imdb_df['Rating'] > 8.5) & (imdb_df['TotalVotes'] > 1000000) ]['Title']

# Retrieve all titles with over 90% score on metacritic
imdb_df[ imdb_df.replace('', numpy.nan)['MetaCritic'] > 90 ]['Title']

# Retrieve all movie ids with earnings less than 1 million
earning_df[ earning_df['Domestic'] < 1000000 ]['Movie_id']

# Retrieve all movie ids where worldwide earnings is only 10% more than domestic earnings
earning_df[ earning_df['Domestic'] * 1.1 > earning_df['Worldwide'] ]['Movie_id']

# Retrieve all Titles where worldwide earnings is only 10% more than domestic earnings
low_worldwide_earners = earning_df[ earning_df['Domestic'] * 1.1 > earning_df['Worldwide'] ]['Movie_id']
imdb[ imdb['Movie_id'].isin(low_worldwide_earners) ]['Title']

# Reteive all movie titles that earns over 300 million dollars domestically
imdb[ imdb['Movie_id'].isin(earning_df[ earning_df['Domestic'] > 300000000 ]['Movie_id']) ]['Title']

# Top 5 movies with the highest WorldWide earnings in the dataset
imdb[ imdb['Movie_id'].isin(earning_df.sort_values(by='Worldwide', ascending=False).head()['Movie_id']) ]['Title']

# Movies where the difference between female ratings (VoteF) is higher than male ratings (VoteM) by more than 0.5
imdb_df = imdb_df.astype( {'VotesM': 'float64', 'VotesF': 'float64'} )
imdb_df[ imdb_df['VotesF']-imdb_df['VotesM'] > 0.5 ]

# Retrieve top 5 comedies by Worldwide earnings
comedies = genre_df[ genre_df['genre'] == 'Comedy' ]['Movie_id']
top_world_comedies = earning_df[ earning_df['Movie_id'].isin(comedies) ].sort_values(by='Worldwide').tail()['Movie_id']
imdb_df[ imdb_df['Movie_id'].isin(top_world_comedies) ]['Title']

# Count how many movies in each genre
genre_df.groupby('genre').count()

```


