### An Analysis of League of Legends
This is a analysis of League of Legends wins based on whether or not teams are leading in kill stats at 15 minutes. This is an analysis done for UCSD for the DSC 80 class.

---

## Introduction
The League of Legends dataset we use in this analysis specifically comes from 2022, titled the "League of Legends Competitive Matches" dataset. This datasets specifically looks at several competitive matches from several leagues throughout the world, collecting important stats such as kils, assists, etc. as well as who the players, teams, and winners were. The dataset was large, containing 123 columns and 149,400 rows.

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

We additionally created 5 different columns. The first two were **Kill_diff** and **Assist_diff**. These two columns represent the killsat15 statistic for the team-opp_killsat15, and the same for assists. Since we want to know if having more kills/assists than your opponent at 15 minutes makes it more likely to win, we need to find the difference between kills for both teams. We also wanted to generalize this better for our future statistical tests, so we created two additonal columns: **pos_kill** and **pos_ass**, which are simply boolean values that indicate whether a team has more kills/assists than their opponent at 15 mins. We then combined kills and assists to create one statistic, creating the column **pos_kill_assist** by adding **Kill_diff** and **Ass_diff** and seeing if there was a net positive or not. This is the key column we will be using for our analysis. 

---
Here's what the cleaned table looks like (first couple of rows):


|    | league   | gameid           | date                | position   | side   |   result |   killsat15 |   assistsat15 |   opp_killsat15 |   opp_assistsat15 |   gamelength |   Kill_diff |   Assist_diff | pos_kill   | pos_ass   | pos_kill_assist   |
|---:|:---------|:-----------------|:--------------------|:-----------|:-------|---------:|------------:|--------------:|----------------:|------------------:|-------------:|------------:|--------------:|:-----------|:----------|:------------------|
| 34 | LPL      | 8401-8401_game_1 | 2022-01-10 09:24:26 | team       | Blue   |        1 |         nan |           nan |             nan |               nan |         1365 |         nan |           nan | False      | False     | False             |
| 35 | LPL      | 8401-8401_game_1 | 2022-01-10 09:24:26 | team       | Red    |        0 |         nan |           nan |             nan |               nan |         1365 |         nan |           nan | False      | False     | False             |
| 58 | LPL      | 8401-8401_game_2 | 2022-01-10 10:09:22 | team       | Blue   |        1 |         nan |           nan |             nan |               nan |         1444 |         nan |           nan | False      | False     | False             |
| 59 | LPL      | 8401-8401_game_2 | 2022-01-10 10:09:22 | team       | Red    |        0 |         nan |           nan |             nan |               nan |         1444 |         nan |           nan | False      | False     | False             |
| 82 | LPL      | 8402-8402_game_1 | 2022-01-10 11:26:11 | team       | Blue   |        1 |         nan |           nan |             nan |               nan |         1893 |         nan |           nan | False      | False     | False             |

---

We then proceeded to do some exploratory analysis of the data, specificially focusing on the columns we were going to use. 

# Univariate Analysis #
Here is a histogram of the **Killdiff** distribution:

<iframe src="assets/kill-difference-distribution.html" width=800 height=600 frameBorder=0></iframe>

This plot shows the overall distrubtion of the difference of kills at 15 minutes between two teams throughout the dataset. We would expect the data to look normal since each positive kill differnce has a corresponding negative difference. This histogram showed us that our data and new columns were working as expected and we had cleaned appropriately. 

# Bivariate Analysis #
We then wanted to do a simple explatory analysis of how win rate increases as the kill difference increases. We did some additional cleaning for this, removing repetitive date by focusing only on positive kill differences. 

We decided to create a grouped table to represent this, grouping by every type of positive kill difference to find the mean wins for each kill difference. Here's the head of the table: 

|    |   Kill_diff |   result |   killsat15 |   assistsat15 |   opp_killsat15 |   opp_assistsat15 |   gamelength |   Assist_diff |   pos_kill |   pos_ass |   pos_kill_assist |
|---:|------------:|---------:|------------:|--------------:|----------------:|------------------:|-------------:|--------------:|-----------:|----------:|------------------:|
|  0 |           0 | 0.5      |     2.61859 |       4.30929 |         2.61859 |           4.30929 |      2012.15 |       0       |          0 |  0.362179 |          0.362179 |
|  1 |           1 | 0.570513 |     3.30128 |       5.57532 |         2.30128 |           3.7516  |      2008.55 |       1.82372 |          1 |  0.754808 |          0.863782 |
|  2 |           2 | 0.745726 |     4.15812 |       6.92094 |         2.15812 |           3.63034 |      2001.08 |       3.2906  |          1 |  0.899573 |          0.963675 |
|  3 |           3 | 0.7557   |     5.08795 |       9.06189 |         2.08795 |           3.52117 |      1930.24 |       5.54072 |          1 |  0.960912 |          0.993485 |
|  4 |           4 | 0.836283 |     6.07965 |      10.4204  |         2.07965 |           3.44248 |      1881.09 |       6.97788 |          1 |  0.964602 |          0.995575 |

Here is a bar graph representing the relationship:
<iframe src="assets/kill-diff-win-rate.html" width=800 height=600 frameBorder=0></iframe>

The plot clearly illustrates that as the overall positive kill difference increases, win rate also increases, showing that our kill differnce could have an effect on what we are asking in our question. 

---

## Assessment of Missingness ##
As is clearly seen from the head of the cleaned table, there seem to be plenty of null values. Specifically, both the killsat15 and assistsat15 seem to have a large number of null values, even when there is other data collected about that specific game, such as the result. We knew that having this many null values was going to be an issue when dealing with the "at15" data, so we decided to analyze this further. Initially, we figured that these data were simply null because that game did not last 15 minutes, which made intuitive sense. However, when querying the dataset for games shorter than 15 minutes, we found no data. Upon researching this further, we learned that no professionaly game has been shorter than 18 minutes, ruling out the possibility that the data were missing because of the game length. The next possibility we figured was that these data were missing simply because they were not recorded for these games. This would make the data **NMAR**. We believed that they were NMAR as the missingness of these columns simply depended on the "at15" values itself: some games simply did not care about this stat enough to record it, focusing on only general data such as overall kills and assists, which were present in the larger, uncleaned dataset even when the "at15" columns were null. 

An extra piece of information that would likely help address this issue is whether or not the league that the game was played in affected whether or not the data were null. Basically, did some leagues simply not record "at15" data? This would make the data **MAR** as the missingness of the "at15" columns could now be explained by and dependent on another column, **league**. 

We then decided to see whether or not the impact of type of league was present on the null values through data manipulation. Specifically, we grouped by table and checked the null values for each league. We made 1 represent null values, and 0 non-null, grouped by league, and then found the mean of null values. Here are the results: 

| league   |   is_missing |
|:---------|-------------:|
| CBLOL    |            0 |
| LCK      |            0 |
| LCS      |            0 |
| LEC      |            0 |
| LJL      |            0 |
| LLA      |            0 |
| LPL      |            1 |
| PCS      |            0 |
| VCS      |            0 |

Here is a bar graph that illustrates the same values but using the count of null values: 

<iframe src="assets/miss-per-league.html" width=800 height=600 frameBorder=0></iframe>

The results make it evident that not only does league have a clear effect on null values, but specifically one league, "LPL", contains all of the null values for the  data we are using. Essentially, our conclusions were correct, LPL simply does not collect "at15" data. In order to confirm these results, we ran a simple permutation test to determine that this data was truly **MAR** and not **MCAR**. 

We tested to see if the distrubtion of league when **killsat15** is null is the same as when it's not null, finding a p-value of exactly **0.0**, in line with what we found earlier in the table and bar graph. Essentially, only one league affects our table with null values, and we really only have to worry about the "LPL" league. 

Here's a plot representing our permutation test for the **league** column:

<iframe src="assets/miss_perm.html" width=800 height=600 frameBorder=0></iframe>

We also did one more test to see if the result was also affected by null values. Running a similar permutation test as before, we came to a p-value of **0.496**, illustrating that knowing the result did not say anything about the missingness of 'at15' data. With this in mind, we proceeded to our actual hypothesis test knowing we only had to handle "LPL" data. 

Here's a plot for the **result** permutation test:

<iframe src="assets/result.html" width=800 height=600 frameBorder=0></iframe>

---

## Hypothesis Testing ##
After assessing the missing values for **killsat15** and **assistsat15**, we concluded that only the "LPL" league contained these missing values. Therefore, before our test, we decided to simply not consider LPL data for our test, as we had enough data from other leagues. Finally, we were prepared for our actual test. Again, here is our central question:
**How impactful is an early lead in kills and assists in league of legends?**
In order to answer this question, we developed our null and alternate hypotheses:
**Ho**: There is no difference between the win rate for teams leading in kills and assists at 15 minutes and the win rate for teams behind in kills and assists at 15 minutes.
**Ha**: Teams leading in kills and assists at 15 minutes have a higher win rate than teams behind in kills/assists at 15 minutes. 
(Win rate is simply the avg. number of wins here)

We decided that a directional hypothesis was best to determine whether there was a specific **advantage** to leading in wins/kills at the 15 minute timeframe. We essentiallly wanted to test whether or not teams leading in this categories around halfway throught the game hang on to win the game at a higher rate. 

Our test statistic here is a **difference of mean** specifically for wins. This was the most intuitive statistic as we wanted to compare win rate, and mean wins made the most sense for that. We recorded this statistic to be **0.368**.

After conduction a hypothesis with 1000 simulations, we came to a very strong p-value of exactly 0.00. At a significance level of 0.05, or any significance level, we reject our null hypothesis. There seems to be strong evidence that teams leading at both kills and assists at 15 minutes win at a higher rate. 

This a plot that represents our hypothesis test, showing clearly the observed difference in win rate between teams with a positive kill/assist difference and those with a negative, as well as the simulated differences: 

<iframe src="assets/hyp.html" width=800 height=600 frameBorder=0></iframe>

These conclusions also make intuitive sense. When leading in kills and assists, a teams is in essence "winning the game" and likely has a much higher chance of just hanging on to win the game in the next 15 minutes. 
