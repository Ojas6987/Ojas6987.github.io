### Ojas6987.github.io
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

| Syntax | Description |
| ----------- | ----------- |
| Header | Title |
| Paragraph | Text |


