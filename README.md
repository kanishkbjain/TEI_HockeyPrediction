# TEI_HockeyPrediction

# tmstats Columns Name Definition
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

# skaters Columns Name Definition
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
