---
layout: post
title: Baseball pitch predictions
comments: false
---

Baseball has long been one of America’s oldest and most popular sports. In addition to its long history, baseball has been on the forefront of the sports “analytics” movement. This analytical approach, called sabermetrics, was pioneered in the 1970’s and 1980’s by Bill James and others, and was brought to prominence in the early 2000’s by Billy Beane’s Oakland Athletics, chronicled by Michael Lewis’ book Moneyball. The history of analytics within the sport, combined with the discrete nature of the game — individual pitches and game events — make baseball data readily available and accessible. One such data source is MLB’s publicly available, PITCHf/x database, in which data is gathered on a per pitch basis using cameras and sensors throughout all 30 major league ballparks.

Throughout a game, each team’s pitchers will throw the ball over 100 times. Not only do each pitcher hold and throw the ball different from one another, but an individual pitcher often throws the ball with different grips and arm mechanics causing the ball to move differently over the 60 feet, 6 inches between the pitching mound and home plate. The variation between pitch speed, movement, and location, is the primary way a pitcher is able to gain an advantage over the batter. The mix of pitches a player throws varies within games, as well as throughout a season (see graph below). Should the batter know what pitch type will be thrown next, this advantage is reduced significantly. The recent sign stealing scandal involving the Houston Astros revolves around illegally intercepting pitch signs, and highlights the market for this information.

**Question:** Using machine learning techniques, is it possible to predict what type of pitch would come next?

For the sake of this blog post, and as a starting point of this analysis, I have limited my dataset to just one pitcher, Jose Berrios. I chose to focus on Berrios for a few reasons. First as a “proof of concept” that these ideas and techniques can be applied to other pitchers (see further development section). Second, Berrios is a starting pitcher resulting in more pitches thrown throughout an individual game, and more data than many other pitchers. Third, Berrios varies his pitches between four different pitch types, causing there to be greater gains from correct predictions.

## Data

To begin, I gathered all recorded pitches for Berrios using the pybaseball package. This data proved to be very thorough (89 different columns), with info on each pitch’s release point, speed, horizontal and vertical break, target, game situation, among others. This provided both useful features for the analysis, as well as a tricky exercise in data wrangling. For example, it would be relatively straightforward to accurately classify each pitch using it’s speed and vertical/horizontal break, however, this data is not known until after the pitch. So while this data would likely be useful, it was necessary to shift it into a historical context, as to prevent any data leakage. For more details, see this project on github.

## Analysis

With the data appropriately processed, we can begin the analysis. First, to establish a performance baseline, I selected the majority pitch type. For Berrios this was his 4-seam fastball, which he threw 32.3% of the time. First, I used a Random Forest Classifier to train my dataset. This made for a moderate improvement, correctly classifying 41.2% of pitches — an improvement of 8.9%. Next, I was curious how a Logistic Regression model would perform, should the pitch types be limited to two classes: fastball and off-speed (off-speed is used to refer to non-fastball pitch types — changeup, slider, curveball, etc.). Again, I established a majority baseline, which was a fastball, thrown 59.7% of the time. However, using the same data as was used in the Random Forest model, the Logistic Regression performed slightly worse, making correct predictions 59.6% of the time.

## Conclusion

While there is likely room for improvement in model selection, engineering additional useful features, and hyperparameter tuning, it is good to be reminded of the subject of this analysis: Jose Berrios. Berrios is the Minnesota Twins top starting pitcher, and is known for his balanced style, using all four pitches to keep hitters off balance. One of his strengths lies in this unpredictability. It makes sense then, that this seemingly random approach would prove difficult to predict.

### Further Development
- Expand the scope of the project to additional pitchers.
- Interesting academic work has been done on this subject, with greater predictive accuracy (see here, here, and here). It would be interesting to try to recreate these results.
- Given the “noise” in the data, perhaps fitting a neural network would be more successful.
- For more details, see this project on [github](https://github.com/michael-rowland/Pitch-Predictions).