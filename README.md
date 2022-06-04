# TEI_HockeyPrediction

<!-- ## About SkyLab Team
Team members: -->

# Table of Contents
0. [Description of Repository](#Description-of-Repository)
1. [Introduction](#Introduction)
2. [Data Collection](#Data-Collection)
3. [Exploratory Data Analysis](#Exploratory-Data-Analysis)
4. [Predictability of Hockey Results](#Predictability-of-Hockey-Results)
5. [Modeling Approach](#Modeling-Approach)
6. [Conclusions and Future Directions](#Conclusions-and-Future-Directions)
7. [tmstats Columns Name Definition](#tmstats-Columns-Name-Definition)
8. [skaters Columns Name Definition](#skaters-Columns-Name-Definition)


## Description of Repository
```bash
|____README.md								Readme file.
|____team_abbreviations.csv		Standard abbreviations for all teams in NHL.

|____notebooks
|    |____EDA.ipynb					  Introductory EDA
|    |____create_XY_inonefile.ipynb		Create training X and training Y dataset (contains team and players data) for NN model in onefile.
|    |____simple NN model TF_tm_players.ipynb	NN model with Tensorflow keras
|    |____Team_EDA.ipynb			EDA to explore correlations in team level statistics for predicting playoff success.
|    |____NN_log_rf_ada_models.ipynb	Final models, including Neural Network, Logistic Regression, Random Forest and Adaboost, to predict the probability of winning Stanley Cup.
|    |____Pulling Info Using nhlpy.ipynb	Notebook to pull out players data using nhlpy API. For each player data, we pulled the roster from each game, and calculated a weighted average of stats at each position. Weights were based of the mean time on ice per player.
|    |____Positions Logistic Regression.ipynb Logistic regression based on skater positions based metrics to predict a win.
|    |____Team_probability_graphic.ipynb	Plot predicted win probability scaled team logos.  
|    |____skater_EDA.ipynb			More EDA combining skaters and team stats.
|    |____EAD_Seasonal_data.ipynb		Check for home team advantage and W/L streaks.


|____tmstats				Most of team data are scraped from hockey-reference.com and NHL.com
|    |____TeamData.csv			Combine all XXXX_XXXX_tmstats.csv dataset into one. (can be deleted)
|    |____tm_player_stats_2005_2021.csv	Regular season team-level and players-level stats of years from 2005 to 2021.
|    |____playoff_stats_2005_2021.csv	Playoff games team-level and players-level stats of years from
|    |____TeamCleaned.csv			Regular season team-level stats of years from 2005 to 2021, add ‘ENG’, ‘MsS’, ‘5v5 TOI/GP’, ‘SAT%’, ‘Playoffs%’, ‘Playoffs’, ‘WonCup’ features.
|    |____points_rate_allyears.csv		Percentage of Points (over the maximum points of that year) for each team in all years
|    |____win_rate_allyears.csv		Percentage of Wins for each team in all years
|    |____2011_2012_tmstats.csv		Team level stats averaged over skaters for this season.
|    |____2009_2010_tmstats.csv
|    |____2000_2001_tmstats.csv
.
.
.

|____Positions by Game		file which contains .csv datasets about player information based on their position, datasets are created by Pulling Info Using nhlpy.ipynb notebook.
|    |____PlayerData.csv
|    |____playoff positional data.csv			Position level statistics of playoff games
|    |____playoff positional data.xlsx			Position level statistics of playoff games
|    |____2016-2017_positions_by_game.csv	Position level statistics (forward/defense/goalie) for each game in the season.
|    |____2009-2010_positions_by_game.csv
|    |____2005-2006_positions_by_game.csv
.
.
.

|____logos					NHL team logos for plotting/visualization.
|	 |____NSH.gif
|	 |____WPG.gif
.
.
.

|____skaters				Skater data from hockey-reference.com
|    |____skaters_allyears.csv			Skater statistics for each year in one file.
|    |____2018_2019_skater.csv
|    |____2014_2015_skater.csv
|    |____2010_2011_skater.csv
.
.
.
```

## Introduction
The Stanley Cup is the trophy awarded to the NHL Playoff champion. It’s the oldest trophy in North American sports and generally considered the hardest trophy to win in professional sport. Naturally, we want to predict who is going to win it because a lot of money is bet on it - by teams, fans and gamblers alike.

This information is useful for:
1. Any hockey fan with a vested interest in a particular team, including sports betters, and Dylan’s Dad who participates in a hockey pool every year.
2. Casinos can use this information to offer lucrative odds on bets.
3. Sports analysts and media houses could use these for better predictive insights.
4. Team managers can leverage this information to make decisions about their roster.

Using just regular season data, we want to answer the following questions:
1. Which features predict playoff success? Goals, wins, penalties, hits?
2. What influence do players have on team outcome? Do changes in the team roster affect performance?
3. Ultimately, who will win the Stanley Cup?


## Data Collection
We collected a wealth of player and team-level data on wins, goals, shots, hits, blocks, penalties, etc. from going back to the 2005-06 season (There were significant rule changes after the 2004-05 lockout, including removing the possibility of a tie game. Data from before this was also missing important features that could not be easily inferred.
).

Data collection method including:
1. Directly scrape from websites to get team-level data for each season, this gives us 47 features of team-level data/
2. Using NHL API Python wrapper:  nhlpy. For the player data, we collect data from an API from each player from each of the 20,000 regular-season games since 2005. For each position, we averaged their stats to get a predicted number of goals, assists, penalties, and so on per game, weighted by the amount of ice-time they get. This way, when a player is injured, the stats reflect this change. This gave us another 15 features (per team per game) that we used as input to a variety of models.

## Exploratory Data Analysis
1. The total number of features:  62 (47 features from team-level data, 15 from player-level data).
2. The most important features in predicting playoff success: 7, including Wins (W), Losses (L), Simple Rating System (SRS), Goals Against (GA), Corsi (SAT%), Penalty Kill (PK%), and the calculated feature Takeaways/Giveaways (TA/GA).
3. For the player data, plus/minus (+/-), goals, and assists were most predictive of wins. Penalty infraction minutes, hits, and blocks were also slightly predictive
4. We also found that there was a notable home-team advantage, although it varied per team, and not much of an emphasis on momentum.

## Predictability of Hockey Results

Why this problem is inherently challenging? Two teams, with the same players playing back-to-back games at the same location can result in two different outcomes. This isn’t a learnable function. [Other research](https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.841.8005&rep=rep1&type=pdf) has shown that 38% of hockey success is inherently un-modelable: it’s just luck. So instead we recognize that game-winning-prediction capability will be capped at 62%, and focus on outputting a probability of success.

This lets us calculate a conditional probability of a team winning a best-of-7 series in the playoffs, and assign each team an overall  probability of winning the Stanley Cup.


## Modeling Approach

For our final model, we collected the roster from each game and created a weighted average of player statistics at each position (forward, defenseman, and goalie). Weights were based on the average time on ice of each player in a given season. This data, along with the home and away team statistics were fit to three separate models to predict which team will win any given matchup. For all models, the training and validation set were from the regular season, and the test set was from the playoff data.

1. Sklearn classifiers: We add Logistic Regression, Adaboost and Random Forest.
2. TensorFlow: We are using TensorFlow Keras Sequential Neural Network. Hyperparameters are optimization algorithms, loss function, layer structure of NN, learning rate, number of epochs, batch size, dropout rate.

The accuracys and winner predictions of each method are shown in table below.

|Model|Train accuracy|Val. accuracy|Test accuracy|Playoff Winner|
|---|:---:|:---:|:---:|:---:|
|Neural Network|68.87%|67.22%|66.29%|Calgary Flames|
|Log. Regression|61.90%|62.82%|61.64%|Calgary Flames|
|AdaBoost|62.28%|62.09%|56.16%|Calgary Flames|
|Random Forest|62.15%|61.55%|53.09%|Calgary Flames|

Overall, we could consistently model a game win with a 62% accuracy, and decided on the Neural Network as the final model to predict game wins in the playoffs. Knowing the teams who made the playoffs and their matchups, we calculated the likelihood for each team to go onto win the Stanley cup. 

(modify this: Our predicted winner for the 2021-2022 season is the Colorado Avalanche with a 27.58% probability of winning. In actuality, the Colorado Avalanche are leading 2-0 against the Edmonton Oilers in the semifinals as of June 4, 2022.)

As of June 4, 2022, we know that the Colorado Avalanche lead 2-0 against the Edmonton Oilers in the semifinals, along with the New York Rangers leading 2-0 against the Tampa Bay Lightning. Based on this updated information, we can update the probabilities of success.

## Conclusions and Future Directions
Hockey is an inherently difficult sport to predict. It is noisy and unpredictable, with underdogs winning far more than in any other professional sport.  Luck makes up nearly 40% of winning a hockey game, which is impossible to model without training on test (playoff) data. Our model was able to account for most of the predictability, correctly predicting winning teams 65% of the time, and producing a realistic Bayesian model (like the weather) that accounts for this. By using data from individual players instead of aggregated team data, we are better able to account for  recent player behavior and roster changes from injuries and trades than existing models.


In the future, we could expand our model to not only predict the winner, but assign a probability for winning that can be used to set odds in a sports-betting conext.
Because we used regular-season data to predict playoff results, there were some factors unique to playoff games that were not accounted for, and test accuracy was lower than validation accuracy in all models. For example, the expected time on ice for a given player may change in the playoffs based on their performance as a strategic decision. In the future, we could possibly estimate changes in parameters from the regular season to the playoffs and use that to improve model accuracy.


## tmstats Columns Name Definition

some description from wikipedia Ice hockey statistics

||Columns Name|Definition|
|---|:---:|:---|
||Rk | Rank|
||AvAge | Average age of team weighted by time on ice.|
||GP | Games Played|
||W | Wins|
||L | Losses|
||OL | Overtime/Shootout Losses (2000 season onward)|
|Scoring|||
||PTS | Team points, calculated from W, OTW, OTL, L, SOL and SOW. As 2 points for a W, 2 points for an OTW or SOW, 1 point for a T or OTL or SOL, and zero for a L.|
||PTS% | Points percentage (i.e., points divided by maximum points)|
|Scoring|||
||GF | Number of goals the team has scored|
|Goalie Stats|||
||GA | Number of goals scored against the team|
||SOW | Shootout Wins|
||SOL | Shootout Losses|
||SRS | Simple Rating System; a team rating that takes into account average goal differential and strength of schedule. The rating is denominated in goals above/below average, where zero is average.|
||SOS | Strength of Schedule; a rating of strength of schedule. The rating is denominated in goals above/below average, where zero is average.|
||GF/G | Goals For Per Game|
||GA/G | Goals Against Per Game|
|Special Teams|||
||PP | Power Play Goals|
||PPO | Power Play Opportunities|
||PP% | Power Play Percentage|
||PPA | Power Play Goals Against|
||PPOA | Power Play Opportunities Against|
||PK% | Penalty Killing Percentage|
||SH | Short-Handed Goals|
||SHA | Short-Handed Goals Against|
||PIM/G | Penalties in Minutes Per Game|
||oPIM/G | Opponent Penalties in Minutes Per Game|
|Shot Data|||
||S | Shots on Goal|
||S% | Shooting Percentage|
||SA | Shots Against|
||SV% | Save Percentage: Percentage of the total shots faced the goaltender has saved|
|Goalie Stats|||
||SO | Shutouts: Number of games where the goaltender had no goals against him and was the only goaltender from his team to play in the game|
||5v5 TOI/GP |5v5 Time on Ice per Game Played *5v5 is only from 2009-2022.|
|| SAT% |Shot Attempts Percent/CORSI |
||(Hits/60)|Hits, Hits per 60 minutes|
||(BkS)|Blocked Shots|
||(BkS/60)|Blocked shots per 60 minutes|
||(GvA)|Giveaways |
||(GvA/60)|Giveaways per 60 minutes |
||(TkA)|Takeaways |
||(TkA/60)|Takeaways per 60 minutes|
||(ENG)|Empty Net Goals|
|| (MsS)| Missed Shots (MsS)|
||Playoffs |is a categorical ranking from a to f, where a = first round, b = 2nd round, c = quarter finals, d = semifinals, e = finals. f means didn't qualify for playoffs.|
||Playoffs%| is a numerical ranking from 0 to 1, describing how well a team did in the playoffs. 0 means they didn't win any games. 1 means they won every series (even if they didn't win all the games in the series). Since different teams play different numbers of games, this was the best I could do. -1 means didn't qualify for playoffs.|
||WonCup |is 1 if they won the Stanley Cup, 0 if they did not, and again -1 if they failed to qualify for the playoffs.|

## skaters Columns Name Definition
||Columns Name|Definition|
|---|:---:|:---|
||Rk | Rank|
||Age | Age at time of finale|
|Scoring|||
||GP | Games Played|
||G | Goals|
||A | Assists|
||PTS | Points|
||+/- | Plus/Minus|
||PIM | Penalties in Minutes|
|Point Shares|||
||PS | Point Shares; an estimate of the number of points contributed by a player.|
|Goals|||
||EV | Even Strength Goals|
|Special Teams|||
||PP | Power Play Goals|
||SH | Short-Handed Goals|
|Goals|||
||GW | Game-Winning Goals|
|Assists|||
||EV | Even Strength Assists|
||PP | Power Play Assists|
||SH | Short-Handed Assists|
|Shot Data|||
||S | Shots on Goal|
||S% | Shooting Percentage|
|Ice Time|||
||TOI | Time on Ice (in minutes)|
||ATOI | Average Time on Ice|
