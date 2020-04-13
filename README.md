# Project summary

## Project description

In this project, an extraction-transfer-load pipeline is built using Python. In total, five tables should be created and populated using data from two different local sources:

1. *songplays*, a fact table listing who played which song by which artist at what time, extracted from log data
2. *users*, a dimension table containing user information like name and level, extracted from log data
3. *songs*, a dimension table containing song information like title and length, extracted from song data
4. *artists*, a dimension table containing artist information like name and location, extracted from song data
5. *time*, a dimension table that lists the units of time (hours, minutes etc.) for each timestamp in the fact table, extracted from log data


## Project files and running the project

The relevant files for submission are `etl.py`, `sql_queries.py` and `readme.md`. `create_tables.py` was provided and not altered. The project can be executed from the command line by running
```
python create_tables.py
python etl.py
```
Unfortunately, only a single in songplay data is augmented with artist_id and song_id. This is explained by the limited scope of testing data. 


## Considerations for CREATE and INSERT statements

* For the fact table (songplays), no unique id is given in the data. I chose to create a unique primary key during ingestion using SERIAL PRIMARY KEY. This key is incremented at insertion time; therefore no conflict management is required.

* For the dimension tables (users, songs, artists, time) unique ids are provided in the data. These are used as PRIMARY KEYs. Since they are vital for joins, I set them as NOT NULL. 

* For songs, artists and time tables, I decided to ignore new rows that have conflicting ids at insertion time, as I don't expect the values to change. In the case of users, a user-id might have been re-assigned, or the user might have change its name or level. I therefore chose to replace all values with the more recent ones. 

* In order to enrich the songplays rows with the song_id and artist_ids, a lookup search is required. Since the combination of song title, artist name and song duration is seen as sufficient to uniquely identify the song, a join of songs and artists (which both contain the arist_id) is used to generate the five required columns: song_id, artist_id, songs.title, artists.name, songs.duration.


## Considerations for etl.py

The implementation was very straight-forward. There are probably ways to insert time, user and songplay data in bulk (per file) instead of line-by-line; however, I chose to stick with the implementation that was provided. 
