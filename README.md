# Predicting NBA All Stars

![banner](/docs/assets/all_stars_2022.png)

[SOURCE](https://www.nba.com/news/best-portraits-2022-nba-all-star)

# Links
- [Website](https://jacquelinekclee.github.io/nba-all-stars-logistic-regression/)
- [Repository (with all relevant files/data)](https://github.com/jacquelinekclee/nba-all-stars-logistic-regression)

# Table of contents

- [Background](#background)
- [The Statistics](#the-statistics)
- [Statistics Source](#statistics-source)
- [The Methodology](#the-methodology)
  - [Training](#training)
- [Usage](#usage)
- [Findings](#findings)
- [The Results](#the-results)
- [Source File](#source-file)
- [Legality](#legality)

## Background
As someone who has been playing basketball since the second grade and has been a Warriors fan since birth, the NBA and basketball in general have always held a special place in my heart. As time goes on, statistics and analytics have played an increasingly larger role in the world of basketball. With this project, and my [NBA Player Position Classifier](https://jacquelinekclee.github.io/nba-players-position-classifier/), I wanted to use my love of basketball in developing and praciticing new Data Science skills.

This classifier, specifically a Logistic Regression Classifier, uses statistics from nearly 19,000 players' seasons from 1980-2017. I will then test my classifier using statistics from the 2018-19 season, the 2020-21 season (skipping the season interrupted by the pandemic), the 2021-22 season, and the most recent season, 2022-23. Given that all star voting involves non-expert (fan) voting and some level of subjectivity, hopefully the results will help reveal which players' All Star designation(s) match the data, which players were overlooked, and which players were perhaps overrated. 

Logistic Regression is a good fit for this model because all features are numeric, and there are many features that are best served by a multiple regression. 

[(Back to top)](#table-of-contents)

## The Statistics
The NBA tracks almost 50 different statistics for every player in the league. Many statistics are often unknown to most basketball fans, so using only the common statistics will make the most sense for everyone. 
Here are some basic definitions of the statistics I will be using in my classifier:
- True shooting percentage (TS%): a metric that demonstrates how efficiently a player shoots the ball. Takes into consideration field goals, 3-pointers, and free throws (unlike other metrics like field goal percentage).
- Rebounds per game (RPG): a metric that shows how many total rebounds (both offensive and defensive) a player averages per game. 
- Assists per game (APG): a metric that shows how many total assists a player averages per game.
- Points per game (PPG): a metric that shows how many total points a player averages per game.
- Blocks per game (BPG): a metric that shows how many total blocks a player averages per game.
- Steals per game (SPG): a metric that shows how many total steals a player averages per game.

[(Back to top)](#table-of-contents)

## Statistics Source
The training data comes from [Kaggle](https://www.kaggle.com/drgilermo/nba-players-stats?select=Seasons_Stats.csv). The test data come from [Basketball Reference](https://www.basketball-reference.com/):
- [2018-19 Data](https://www.basketball-reference.com/leagues/NBA_2019_per_game.html)
- [2020-21 Data](https://www.basketball-reference.com/leagues/NBA_2021_per_game.html)
- [2021-22 Data](https://www.basketball-reference.com/leagues/NBA_2022_per_game.html)
- [2022-23 Data](https://www.basketball-reference.com/leagues/NBA_2023_per_game.html)

[(Back to top)](#table-of-contents)

## Usage

Please refer to the [Jupyter Notebook Viewer](https://nbviewer.org/github/jacquelinekclee/nba-all-stars-logistic-regression/blob/main/nba_all_stars_logregr.ipynb) or the [.ipynb file](https://github.com/jacquelinekclee/nba-all-stars-logistic-regression/blob/main/nba_all_stars_logregr.ipynb) to view all the code for the classifier. The notebook with all the data cleaning can be seen [here](https://github.com/jacquelinekclee/nba-all-stars-logistic-regression/blob/main/nba_players_data_cleaning.ipynb). 

The [source file](#source-file) contains all the functions used to clean/manipulate the data and DataFrames.

[(Back to top)](#table-of-contents)

## The Methodology
Since there are many features being used, a classifier involving distance like K-Nearest Neighbors would not be the most straightforward; K-NN loses interpretability with higher dimensions. Thus, with the relatively large (>2) amount of numerical features, logistic regression is the logical choice.  Additionally, instead of using accuracy as the evaluation metric, recall was chosen instead given my interest in capturing all the All Star players in each season. In addition to looking at recall, I also looked at the X highest probabilities for a given season, where X is the number of All Stars selected, to see how well the model could replicate the real life decision process.

[(Back to top)](#table-of-contents)

## Training
The main hyperparamter in logistic regression involve the type of regularization used. See below for the hyperparameters tested in grid search:

Parameters tested:
`{"regr__C":np.logspace(-3,3,7), "regr__penalty":["l1","l2"]}`

Best paramters:
`{'regr__C': 100.0, 'regr__penalty': 'l2'}`

All numeric features were scaled, and the only non-numeric feature, player position, was one-hot encoded. Another version was also trained in which numeric features were scaled with respect to the statistic's specific season. 

[(Back to top)](#table-of-contents)

## Findings
Below is a table summarizing the performances of the different test sets. V2 references the model that was trained/tested with scaling the numeric features with respect to season. 

| Classifier             | Season    |   Recall Score |   Precision Score |   Accuracy |   Top Players % Correct |
|:-----------------------|:----------|---------------:|------------------:|-----------:|------------------------:|
| Logistic Regression    | 2022-2023 |          1     |             0.574 |      0.962 |                   0.778 |
| Logistic Regression V2 | 2022-2023 |          1     |             0.303 |      0.884 |                   0.778 |
| Logistic Regression    | 2021-2022 |          0.889 |             0.6   |      0.969 |                   0.741 |
| Logistic Regression    | 2020-2021 |          0.963 |             0.553 |      0.955 |                   0.741 |
| Logistic Regression V2 | 2020-2021 |          1     |             0.397 |      0.916 |                   0.741 |
| Logistic Regression    | 2018-2019 |          0.769 |             0.571 |      0.96  |                   0.731 |
| Logistic Regression V2 | 2018-2019 |          0.846 |             0.524 |      0.954 |                   0.731 |
| Logistic Regression V2 | 2021-2022 |          0.926 |             0.309 |      0.904 |                   0.704 |

The classifier was able to find a majority of each season's all stars, performing best for the 2022-23 season. When looking at the players with the 26-27 highest probabilities to represent the 26-27 players chosen as all stars, 70%+ of these players were all stars. 

## The Results
See below for some discussion on the model's predictions. See the table linked [here](/all_star_logregr_summary.csv) to see a full summary of the 3 NBA seasons used in testing and the results for each player. 

### "Properly Rated" All Stars: The True Positives
There were only 6 players who were voted as All Stars for all 4 seasons (2018-19, 2020-21, 2021-22, and 2022-23) *and* were deemed "properly rated" for all 3 seasons. In this case, "properly rated" means that the all star voting seemed to match the data and the KNN model's findings.
- Giannis Antetokounmpo
- Joel Embiid
- Kevin Durant
- LeBron James
- Stephen Curry
- Nikola Jokic

### "Overrated" All Stars: The False Negatives
The "overrated" players are instances where the classifier predicted a player *wasn't* an All Star when in reality they were. These cases could include players that perhaps contribute in ways that don't show up on the stat sheet or players that a particular fan favorites. For instance, in the 2018-19 season, future hall-of-famers and NBA champions Dirk Nowitzki and Dwyane Wade were voted as All Stars, but the classifier thought differently. That season was both Nowitzki and Wade's last season in the NBA, so while their stats maybe weren't up to par, their legendary careers earned them the designation. 

For 2 of the 4 seasons used in testing, Khris Middleton (2018-19 and 2021-22), fell under the "overrated" category. Middleton might not have the opportunity to shine in the stats playing alongside generational talent Giannis Antetokounmpo, thus resulting in a false negative. Another notable "overrated" All Star is Draymond Green for the 2022 All Star Game. His selection may be due more in part to playing on the champions that season, the Golden State Warriors, and one of the best players in the league, Stephen Curry, than his statistical production. 

### "Underrated" Players: The False Positives
The "underrated" players are those that the classifier predicted to be all stars, but weren't voted as all stars in reality. Perhaps the stats of such a player were exceptional, but other aspects like their winning percentages weren't up to par. See below for the players from the 2018-19, 2020-21, 2021-22, and 2022-23 seasons that were deemed underrated and were never selected as an All Star in any of the 4 seasons:
- Brandon Ingram (2020-21, 2021-22, 2022-23)
- CJ McCollum (2020-21, 2021-22)
- John Wall (2018-19)
- Kristaps Porzingis (2020-21, 2021-22, 2022-23)

For 2 seasons in a row, young guard Shai Gilgeous-Alexander was "underrated" in the eyes of the model. His team, the OKC Thunder, had a terrible record this past season and finished in 14th place in the Western Conference. This lack of winning, and the fact that OKC is a smaller market, may explain why Gilgeous-Alexander was overlooked in all star voting. But in the most recent season (2022-23), he was finally chosen as an all star, perhaps because his team has done slightly better. 

[(Back to top)](#table-of-contents)

## Source File
- [nba_players_classification.py](https://github.com/jacquelinekclee/nba-all-stars-logistic-regression/blob/main/nba_players_classification.py)
  - Has all the functions used for mainly the data cleaning.

## Legality
This personal project was made for the sole intent of applying my skills in Python thus far and as a way to learn new ones. It is intended for non-commercial uses only.

[(Back to top)](#table-of-contents)
