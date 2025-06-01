# Steam Review Analysis User`s Guide
This user guide is intended for guiding the other users/researchers who may want to analyze steam reviews for various purposes. To make things clearer for them, the guide briefly lists the prerequisites, walks them through the analysis process through a specific game review (Stardew Valley), and lastly provides instructions about how users can analyze their own games of interest. In this way, I hope other users/researchers may go through steps of analyzing reviews. I will also conclude with some warnings that new users/researchers may find useful in their own analysis. Let`s now walk you through the necessary steps to set up and run the project which involves scraping review data, processing it, and visualizing various statistics and correlations.

## Prerequisites:

- Before you begin, ensure you have Python 3.10 or higher installed (I used 3.12)
- After cloning the repo, install the required Python packages by running:
```
pip install requests pandas seaborn matplotlib plotly nbformat scipy --upgrade
```
## Walk-through via Stardew Valley
This walk-through takes place via the analysis of the review of the Stardew Valley. In this section, you will find out how the analysis took place and what key findings are available. Here are several major steps of instructions that will help you.

- set the base name for the file (I used Stardew Valley).
- include the same name in the extended file name.
- if needed make a new folder of the base name.
- load the processed CSV file.
- add variables of interest (I added number of sentences and average number of words per sentence into the previously created extended file. You can add your own variables of interest).
- display the first few rows of your dataset and summary statistics to explore your data.

Now I will provide you with some of the key findings from the analysis of the Stardew Valley reviews.

### Plotting aggregated sentiment values over time
It appears that exploring the aggregated values over time is a good starting point. To do this, I analyzed the monthly means of sentiment values (positive, negative, objective) from 2016 to 2024. Fig 2 below shows the values over the mentioned time period. As can be seen, the positive scores have kept always higher than the negative scores over time while objective score is visibly much higher than both scores. This plot seems to support the finding steampowered.com has found as "overwhelmingly positive" for the game.

![plot_over_time](https://github.com/user-attachments/assets/474fe88e-b8e1-47bf-89a6-b694ddc0e7bf)

Exploring aggregated values over time yields intriguing findings. For example, Fig 3 displays values such as author_vote, votes_funny, and other_votes bewteen 2016 and 2024. It is seen that votes_funny values in 2017 have spiked extraordinarily compared to the rest of the data. It can be speculated that those spikes could be outliers or something new in the game might have caused an enormous increase in reviews. Another factor would be the count of all the votes in all languages reviewed. 

![plot_over_time2](https://github.com/user-attachments/assets/4a1f8c5c-18b8-4956-85ba-71f226bca2ac)

### Ratio of Part of Speech (POS) in the review text
I aimed the compare the percentages of POS in the dataset. As the following table indicates, nouns are the most frequently used POS category, followed by verbs, adjectives, and adverbs. The values for reviews with less than 1% of these POS corroborate the earlier finding that nouns and verbs are more frequently used in the reviews compared to adjectives and adverbs. That is, while there are only 18 reviews with less than 1% of nouns, there are 2304 reviews with less than 1% of adverbs. 

<table>
    <tr>
        <td>POS</td>
        <td>Percentage Mean</td>
        <td>Reviews with <1% of POS</td>
    </tr>
    <tr>
        <td>Noun</td>
        <td>39.65</td>
        <td>18</td>
    </tr>
    <tr>
        <td>Verb</td>
        <td>32.28</td>
        <td>200</td>
    </tr>
    <tr>
        <td>Adj</td>
        <td>14.17</td>
        <td>1213</td>
    </tr>
    <tr>
        <td>Adv</td>
        <td>13.90</td>
        <td>2304</td>
    </tr>
</table>

### Relationship between POS and Sentiment 
Previous work on determining the strength of subjective expressions in text or specifically reviews use various POS such as adjectives, verbs, and nouns. In this analysis, I examined how adjectives relate to the sentiment of the reviews. The Seaborn KDE plot below can be interpreted as a two-dimentional histogram mapping the bivariate distribution between the frequency adjectives and positive sentiment score. It is seen that the most common positive sentiment score is around 0.08 with 5 adjectives. It can be deduced that the higher number of adjectives does not lead to more positive sentiment score.

![KDE_positive_sentiment](https://github.com/user-attachments/assets/fc38f5f8-6228-4a48-88c4-efe345216e86)

I also looked at the distribution of adjectives in relation to negative sentiment score. It seems that the data is bimodal around 5 adjectives with negative sentiment scores accumulating at 0.01 and 0.08 respectively.

![KDE_negative_sentiment](https://github.com/user-attachments/assets/fa554dd7-391c-491d-84a4-c1dd883ed161)

### Correlations
I examined the Spearman correlations between the variables of interest. When you run the script, you can find all the correlation pairs. However, I listed the most interesting correlations over 0.5 threshold. As the following table shows, num_sentences is highly correlated with verb_count, noun_count, and adj_count. Please note that there are other correlation pairs that can be found when you run the data.

<table>
    <tr>
        <td>Select Positive Spearman Correlations</td>
    </tr>
    <tr>
        <td>other_votes</td>
        <td>weighted_vote_score</td>
        <td>0.909</td>
    </tr>
    <tr>
        <td>num_sentences</td>
        <td>verb_count</td>
        <td>0.636</td>
    </tr>
    <tr>
        <td>verb_count</td>
        <td>adj_count</td>
        <td>0.632</td>
    </tr>
    <tr>
        <td>noun_count</td>
        <td>num_sentences</td>
        <td>0.606</td>
    </tr>
    <tr>
        <td>adv percentage</td>
        <td>adv_count</td>
        <td>0.603</td>
    </tr>
    <tr>
        <td>adv_count</td>
        <td>adj_count</td>
        <td>0.563</td>
    </tr>
    <tr>
        <td>adj_count</td>
        <td>num_sentences</td>
        <td>0.533</td>
    </tr>
    <tr>
        <td>num_sentences</td>
        <td>adv_count</td>
        <td>0.538</td>
    </tr>
</table>

The correlation matrix below indicates that part-of-speech variables (e.g., adj_count, verb_count, noun_count) are highly correlated with each other. In addition, as can be seen, weighted_vote_score and other_votes are also higly correlated, which is understandable considering that the reviews that receive positive votes should have higher weighted_vote_score. I anticipated correlations between votes_funny and weighted_vote_score, but it did not result in my expectation. 

![correlation_matrix](https://github.com/user-attachments/assets/1759073d-95ee-4eae-98b1-0d78854deb98)

I looked at the relationship between num_sentences and word_count via a scatterplot with a regression line. As the plot shows, the number of sentences increases as the word count increases. As expected, there is a strong positive relationship. 

![scatterplot_regression_line](https://github.com/user-attachments/assets/416eb565-1768-47ca-b364-c8bb7037ae73)

Using the plotly, I also examined the relationship between word_count and num_sentences interactively. The following interactive plot showcases it. Please note that the interactivity may not be available over the markdown file. However, you can experience it when running your code.

![interactive plotly2](https://github.com/user-attachments/assets/049e0897-88ff-4064-b875-76a802051c11)

Stardew Valley is an overwhelmingly positive game according to Steam (2024). You can find details of the game at https://store.steampowered.com/app/413150/Stardew_Valley/. If you`d like, you can also analyze an overwhelmingly negative game such as Overwatch 2 and compare the games. 

## How to analyze your own game
So far you have seen a walk-through of the review through a specific game (Stardew Valley). Now this section provides instructions about how you can analyze your own game.

- find the steam app id of the game whose review you will analyze. To do this, go to the following link and search for the game in the search bar. This will take you to the game page (URL). You can locate numeric app id in the URL.
```
https://store.steampowered.com
https://store.steampowered.com/app/####/YourGame/
```
- Copy the app id from the steampowered URL and fill it out and name your game in the scrape_and_sentiment_analyze_steam_reviews.ipynb
- Run scrape_and_sentiment_analyze_steam_reviews.ipynb which will create an extended file in the base folder. This extended file includes the scraping and sentiment analysis on the reviews.
- Put the same name you used in the previous step into the review_analysis.ipynb where you will set base name for your game.
- Run review_analysis.ipynb which will use the extended CSV file you created via the scrape_and_sentiment_analyze_steam_reviews.ipynb. Running review_analysis.ipynb will include various statistical analysis and visualization of the findings. You can add different analysis at this stage based on how you would like to examine the data.


## User notes to watch out:
- possible outliers. Dive deeper into the data for cases where you think there are outliers such as aggregate over time data from May 2017.
- select an appropriate time span for accumulations. Depending on the game and specificities in your case, you can select monthly, weekly, or daily spans. 
- try to use pairs of variables that have more than 0.5 r-squared. In this way, more meaningful relationships could be examined. 
