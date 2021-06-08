# Data Collection and Cleaning

The data used in this project was collected from the API at [collegefootballdata.com](https://www.collegefootballdata.com/), whose documentation can be found [here](https://api.collegefootballdata.com/api/docs/?url=/api-docs.json#/) and [here](https://github.com/CFBD/cfbd-python).

## Overview
This phase fell largely into 2 categories: Collecting data from the API at several endpoints and merging appropriate files, culminating into the creation of 2 files: ***team_data.csv*** and ***game_data.csv***. Creating these files fulfilled 2 purposes:
1. Creating data to train, evaluate, and optimize our chosen classification model (*game_data.csv*)
2. Allow for easy creation of a new prediction data point in our streamlit application (*team_data.csv*)

## API Access
The following links are the endpoints at which the API was accessed to collect our data.
* api.collegefootballdata.com/games
* api.collegefootballdata.com/talent
* api.collegefootballdata.com/stats/season
* api.collegefootballdata.com/player/returning

GET requests were made using Python's [`request`](https://docs.python-requests.org/en/master/) library and an API authentication key, like so:
```python
# Create a session object
my_session = requests.Session()
auth_key = 'Bearer my_secret_key'
base_url = 'https://api.collegefootballdata.com/'
# So we don't have to list our authentication token with every request
my_session.headers.update({'Authorization': auth_key})

# Accessing the 'conferences' endpoint to get a list of college football conferences
conference_url = base_url + 'conferences'
my_session.get(conference_url) # Results in a successful 200 OK server response
```
There are of course additional data cleaning steps and the full code and documentation can be found in this folder.

## Merging
Now that there are 4 files comprising different pieces of information, they need to be merged into a final 2 files. Because several files need to be merged on several different columns, doing this in python would be somewhat cumbersome and clunky. This is where SQL can help. To solve this, I created a local SQLite database with 3 tables ( `season_stats`, `talent`, and `production`) by importing previously created `csv` files. This way, a single file that has data on a team's talent, average statistics, and production on a per-year basis can be created.

Our SQL statement:
```SQL
SELECT season_stats.*, talent.talent, production.totalPPA 
FROM talent 
LEFT JOIN production ON 
    talent.year=production.season AND talent.school=production.team 
LEFT JOIN season_stats ON 
    production.season= season_stats.season AND production.team=season_stats.team;
```

The returned results can subsequently be exported to a `csv` file for easy read-in with Pandas. Now that we have cleaned files on game results and annual team statistics, the [modeling process can begin.](https://github.com/DImsirovic/cfb_game_prediction/blob/main/modeling.md)
