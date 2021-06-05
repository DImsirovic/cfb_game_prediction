# Data Collection and Cleaning

The data used in this project was collected from the API at [collegefootballdata.com](https://www.collegefootballdata.com/), whose documentation can be found [here](https://api.collegefootballdata.com/api/docs/?url=/api-docs.json#/) and [here](https://github.com/CFBD/cfbd-python). Complete code documentation with Jupyter notebooks can be found in this Github folder.

## Overview
This phase fell largely into 2 categories: Collecting data from the API at several endpoints and merging appropriate files, culminating into the creation of 2 files: ***team_data.csv*** and ***game_data.csv***. Creating these files fulfilled 2 purposes:
1. Creating data to train, evaluate, and optimize our chosen classification model (*game_data.csv*)
2. Allow for easy creation of a new prediction data point in our streamlit application (*team_data.csv*)

## API Access
* api.collegefootballdata.com/games
* api.collegefootballdata.com/talent
* api.collegefootballdata.com/stats/season
* api.collegefootballdata.com/player/returning

## Merging
Now that we have 4 files comprising different pieces of information, we need to merge them into our final 2 files. Because we need to merge several columns of several files, doing this in python would be somewhat cumbersome and clunky. Luckily, SQL is designed for such a thing. To solve this problem, I created a small SQLite database with 3 tables: `season_stats`, `talent`, and `production`.

Our SQL statement:
```SQL
SELECT season_stats.*, talent.talent, production.totalPPA 
FROM talent 
LEFT JOIN production ON 
    talent.year=production.season AND talent.school=production.team 
LEFT JOIN season_stats ON 
    production.season= season_stats.season AND production.team=season_stats.team;
```
