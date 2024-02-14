---
layout: post
title:  "Checking out the Discogs API — API Practice 2" 
date: "2024-02-14"
categories: projects
tags: [python]
---

## Introduction: Polars & Discogs API 

For this project, I'm calling the Discogs API to create a DF of any album I want within Discog's album catalog so that I can make it into a data frame. Any album can be called by any artist, but be mindful of the name of the artist and file's name when saving to .csv 

If its desired to do an entire artist's discography, it would be possible to save multiple URLs to their own objects, source the data using the code below, and then concatenate all the resulting dataframes together using a loop to iterate over them. 

---

### Setup 

```python
# Import libraries: 
import requests as req
import datetime as dt
import polars as pl
import json

# Set URL object and user agent
url = ('https://api.discogs.com/masters/45947')

user_agent = "API-call-Practice/1.0 (************@gmail.com)"

# You can generate your own API token here: https://www.discogs.com/settings/developers
auth_token = "***********************************"

headers = {
        "User-Agent": user_agent,
        "oauth_token": auth_token
}

res = req.get(url).json()
```
Some APIs need authentication in order to enable calls to be made to their servers, so in the above lines, I'm establishing an ID and name for the project that I'm calling the API from via the "User Agent," along with a "token" which serves as the secret/password for identification purposes. 

The URL in the snippet takes you to the Discogs developers page where you can generate a free-use token and sign up for their services if you're doing more involved work, like creating an app or service.  

### Sourcing the Data 

```python
# Checks if API call is successful
if res:
    print('Success!')
else:
    print('Error!')

flat_data = []

for track in res.get("tracklist", []):
    extra_artists_names = ", ".join(artist['name'] for artist in track.get("extraartists", []))

    track_info = {
        "master_id": res["id"],
        "album_title": res["title"],
        "track_title": track.get("title", ""),
        "track_position": track.get("position", ""),
        "track_duration": track.get("duration", ""),
        "extra_artists": extra_artists_names,
        "year": res["year"]
    }
    flat_data.append(track_info)
        
df = pl.DataFrame(flat_data)       
```

This code snippet is what's "extracting" the data from the API using the credentials I specified above. The first line checks and tells me if the API connection was established or not, printing a statement if success or otherwise. 

From there, I make an empty list for the data to go in, I then establish a for loop to iterate over the sections of the JSON that house the "tracklist" data. 

I then create another object called "track_info" that pulls the needed columns and flattens them, then appends the track_info object into the empty "flat_data" list I made eariler. From there, I use Polars to make that list into an actual data frame. 

...

```python
# Comment out if using on other albums. Change the name in the pl.lit call to the correct artist 
df = df.with_columns(pl.lit('A Tribe Called Quest').alias('artist'))

df = df.select(['master_id', 'album_title', 'track_title', 
                'track_position', 'track_duration', 'artist', 'extra_artists', 'year'])

# Print data frame to check data: 
print(df)
```
When I thought of this ETL script, though a little basic, I wanted for it to be able to be reused for other artists as well. Essentially, any artist you find on the Discogs page can have their data pulled in the same manner as I did 'A Tribe Called Quest' using this script. You'll just need their API url. 

I learned that if the album Midnight Maraders was here: https://www.discogs.com/master/45947-A-Tribe-Called-Quest-Midnight-Marauders

Then the API equivalent of that same album on their site is here: https://api.discogs.com/masters/45947

All you do is switch 'www.' to 'api.', turn 'master' into its plural form 'masters' and then give the album ID which is the sequence of numbers at the end without the descriptor. 


```python
# Comment out, change file name and then re-run 
#df.write_csv("c:\\Users\\xxxxx\\Documents\\atcq_artist_data.csv", separator= ',')
```

Lastly, all you need to do is write the data to .csv using the line above. The resulting .csv you can use for other projects. 

## Outtro

That's all there is to it! I'm working on thinking about how I can further expand on this script to do more involved work in the ETL process but this here is just a proof of concept. 

Thank you for reading! 