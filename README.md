### An Analysis of League of Legends
This is a analysis of League of Legends wins based on whether or not teams are leading in kill stats at 15 minutes. This is an analysis done for UCSD for the DSC 80 class.

---

## Introduction
The League of Legends dataset we use in this analysis specifically comes from 2022, titled the "League of Legends Competitive Matches" dataset. This datasets specifically looks at several competitive matches from several leagues throughout the world, collecting important stats such as kils, assists, etc. as well as who the players, teams, and winners were. The dataset was large, containing 123 columns and FILL IN rows.

With this dataset, we specifically sought to find out when players gain an edge in the game. Essentially, we were looking at how likely a team would be to win a game based at a certain point in the game. With this in mind, we reached the following question we wanted to address:

**How impactful is an early lead in kills and assists in league of legends?**

We primarily focused on 5 columns:
- killsat15: Number of kills team has at 15 minutes
- assistsat15: Number of assists team has at 15 minutes
- result: Overall result of game, win or loss
- opp_killsat15: team' opponents' kills at 15 mins
- opp_assistsat15: team's opponents' assists at 15 mins

---

## Cleaning and EDA ##
To begin cleaning this data, we first made sure to include only the columns that would be useful to answer our question. These are:
- killsat15: Number of kills team has at 15 minutes
- assistsat15: Number of assists team has at 15 minutes
- result: Overall result of game, win or loss
- gameid: ID specific to game
- date: Specific datetime of game
- position: Simply specifies whether is a player or team
- opp_killsat15: team' opponents' kills at 15 mins
- opp_assistsat15: team's opponents' assists at 15 mins
- league: The league the game was a part of 

We included result, gameid, date, position, and league to retain some general info about the games that could be useful. In order to make our data more manageable, we only selected the top leagues in the world for our analysis: 
**"LCK","LPL","LEC", "LCS", "PCS", "VCS", "CBLOL", "LJL", "LLA"**
Each represents a specific acronym for their respective league. Next, we only kept data that was useful for teams. We dropped all of the individual player data from the table, and kept the team summary statistics. 

We additionally created 4 different columns. The first two were **Kill_diff** and **Assist_diff**. These two columns represent the killsat15 statistic for the team-opp_killsat15, and the same for assists. Since we want to know if having more kills/assists than your opponent at 15 minutes makes it more likely to win, we need to find the difference between kills for both teams. We also wanted to generalize this better for our future statistical tests, so we created two additonal columns: **pos_kill** and **pos_ass**, which are simply boolean values that indicate whether a team has more kills/assists than their opponent at 15 mins.

---
Here's what the cleaned table looks like (first couple of rows):


|league|gameid|date|position|side|result|killsat15|assistsat15|opp_killsat15|opp_assistsat15|gamelength
| ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- |
|34|LPL	8401-8401_game_1|2022-01-10 09:24:26|team|Blue|1|NaN|NaN|NaN|NaN|1365|
|35|LPL	8401-8401_game_1|2022-01-10 09:24:26|team|Red|0|NaN|NaN|NaN|NaN|1365|
|58|LPL	8401-8401_game_2|2022-01-10 10:09:22|team|Blue|1|NaN|NaN|NaN|NaN|1444|
|59|LPL	8401-8401_game_2|2022-01-10 10:09:22|team|Red|0|NaN|NaN|NaN|NaN|1444|
|82|LPL	8402-8402_game_1|2022-01-10 11:26:11|team|Blue|1|NaN|NaN|NaN|NaN|1893|

---

## Assessment of Missingness ##
As is clearly seen from the head of the cleaned table, there seem to be plenty of null values. Specifically, both the killsat15 and assistsat15 seem to have a large number of null values, even when there is other data collected about that specific game, such as the result. We knew that having this many null values was going to be an issue when dealing with the "at15" data, so we decided to analyze this further. Initially, we figured that these data were simply null because that game did not last 15 minutes, which made intuitive sense. However, when querying the dataset for games shorter than 15 minutes, we found no data. Upon researching this further, we learned that no professionaly game has been shorter than 18 minutes, ruling out the possibility that the data were missing because of the game length. The next possibility we figured was that these data were missing simply because they were not recorded for these games. This would make the data **NMAR**. We believed that they were NMAR as the missingness of these columns simply depended on the "at15" values itself: some games simply did not care about this stat enough to record it, focusing on only general data such as overall kills and assists, which were present in the larger, uncleaned dataset even when the "at15" columns were null. 

An extra piece of information that would likely help address this issue is whether or not the league that the game was played in affected whether or not the data were null. Basically, did some leagues simply not record "at15" data? This would make the data **MAR** as the missingness of the "at15" columns could now be explained by and dependent on another column, **league**. 
