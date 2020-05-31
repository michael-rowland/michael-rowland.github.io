---
layout: post
title: Betting on ELO ratings
comments: false
---

Does a betting strategy based on a ratings based system, lead to better results?

The National Football League is America’s most popular sport to not only watch, but bet on. With more states beginning to offer legal avenues to bet on games, participation in betting markets will also likely continue to increase.

There are many strategies ordinary football viewers often think lead to positive results. Betting on one’s favorite team, public teams (Dallas Cowboys, New England Patriots, etc.), or the hot team coming off of a big win, are all common. As much as I too have my own “favorite team” bias, I have often wondered if a data-driven strategy would lead to a better outcome.

**Enter FiveThirtyEight’s NFL predictions model, [ELO](https://projects.fivethirtyeight.com/2019-nfl-predictions/).**

This model works by calculating each team an ELO Rating. These ratings are then used to generate win probabilities for each week’s games. Then following each game, a team’s ELO rating is adjusted. The adjustment is based on the outcome of the game, and how unexpected the outcome compared to the probability expectations going into the game. For instance, as of writing this, the Baltimore Ravens have the highest ELO rating of 1753 (an average team would be around 1500) and are given an 87% win probability for their upcoming playoff game against the Tennessee Titans, who have an ELO rating of 1615.

You can read more about this system [here](https://fivethirtyeight.com/methodology/how-our-nfl-predictions-work/), and [here](https://en.wikipedia.org/wiki/Elo_rating_system).

The ELO model is relatively free from week-to-week fan bias. Sportsbooks, however, are not immune from public pressure. This is because a well run sportsbook will aim to establish their odds so 50% of the wagers they take are on either side of a particular bet. As a result, odds for a specific event will factor in public perception, and may not necessarily represent the expected result.

If we assume the ELO model is a better indicator of expected performance, a strategy that incorporates this may have an opportunity to gain an edge.

**My proposed strategies:**
1. Bet on the three highest rated favorites each week
2. Bet against the three lowest rated underdogs each week

### Methodology

To answer this question I need two main data sets. First, I need historical ELO projections. These are easily downloaded from FiveThirtyEight’s website.

Second, I need historical line data for each game. While data did not appear to be readily available to download, there are many websites who provide this data. I settled on pro-football-reference, as their data seemed fairly standardized, meaning it would be fairly straightforward to scrape. Like many other websites, pro-football-reference only provided points spread data. As my analysis will be for straight-up (moneyline) picks, I need a way to convert from point spreads to moneyline payouts. While these payouts can often vary between sportsbooks, [this chart](https://wizardofodds.com/games/sports-betting/appendix/11/) provided a reasonable mapping between the two.

With these two data sets, we can filter our data to meet the conditions outlined in the strategies above, and then use the game outcomes to estimate potential payouts and losses for our selected games.

### Strategy #1 Results

(/assets/betting-1.svg)
(/assets/betting-2.svg)

These results are very convincing. For the past five years, assuming a bet amount of $100, we see a **positive final earnings of $2,440.24**, with our strategy leading to a correct bet 72.83% of the time. When applied to the current (2019) regular season, we see **positive final Earnings of $1,003.96**, and a winning bet percentage of 78.43%. This tends to match the general narrative of a greater number of dominant “superteams.”

### Strategy #2 Results

(/assets/betting-3.svg)
(/assets/betting-4.svg)

The results of betting against the lowest rated teams is equally as striking. Again, over the past five years, our strategy results in **positive final earnings of $2,785.97**, and a correct bet percentage of 71.94%. Over the most recent season, our strategy results in **final earnings of $80.77**, which is notably lower than the results of our first strategy in the same time period, however still positive.

With legalized sports gambling becoming more widespread, and more individuals entering gambling markets, I hope this analysis may inform future football wagers, and cause others to realize an edge may be gained by using data to inform their bets.

For more details, see this project on [github](https://github.com/michael-rowland/ELO-Betting-Strategy).

### Further Research Ideas
- The decision to choose the top and bottom three values was fairly arbitrary, this value could be adjusted in either direction.
- Instead of using moneyline bets, analyzing parlay bets or bets against the spread would be interesting.
- Instead of using a team’s ELO rating for making bet decisions, bets could be placed based on ELO probabilities a team has to win.