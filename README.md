- brief intro
- Prerequisites
- walkthrough using review_analysis_stardew_valley.ipynb with images and some comments
- how to analyse your own game
    - find steam app id
    - fill out appid and name in scrape_and_sentiment_analyze_steam_reviews.ipynb
    - run scrape_and_sentiment_analyze_steam_reviews.ipynb
    - will create extended file in folder
    - put name in review_analysis.ipynb (maybe rename to your game?)
    - run review_analysis.ipynb, will use extended file

- watch out for:
    - possible outliers
    - sometimes log axis makes a relationship clearer
    - select a appropriate time span for accumulations
    - try to used pairs of variables that have more than 0.5 r-squared

# Steam Review Analysis User`s Guide
This user guide is intended for guiding the other users/researchers who may want to analyze steam reviews for various purposes. To make things clearer for the prospective users/researchers, this guide briefly lists the prerequisites, walks them through the analysis process through a specific game review (Stardew Valley), and lastly provides instructions about how users can analyze their own games of interest. In this way, I hope the other users/researchers may go through steps of analyzing reviews. I will also conclude with some warnings that new users/researchers may find useful in their own analysis. Let`s now walk you through the necessary steps to set up and run the project. which involve scraping review data, processing it, and visualizing various statistics and correlations.

## Prerequisites:

- Before you begin, ensure you have Python 3.10 or higher installed (I used 3.12)
- After cloning the repo, install the required Python packages by running:
```
pip install requests pandas seaborn matplotlib plotly nbformat scipy --upgrade
```
## Walk-through via a Stardew Valley
This walk-through takes place via the analysis of the review of the Stardew Valley. In this section, you will find out how the analysis took place and what key/interesting findings are available. Here are several major steps of instructions that will hell you.

- set the base name for the file (I used Stardew Valley).
- include the same name in the extended file name.
- if needed make a new folder of the base name.
- load the processed CSV file.
- add variables of interest (I added number of sentences and average number of words per sentence into the previously created extended file. You can add your own variables of interest).
- display the first few rows of your dataset and summary statistics to explore your data.

Now I will provide you with some of the key findings from the analysis of the Stardew Valley reviews.

### Plotting aggregated sentiment values over time
It appears that exploring the aggregated values over time is a good starting point. To do this, I analyzed the monthly means of sentiment values (positive, negative, objective) from 2016 to 2024. Fig 2 below shows the values over the mentioned time period. As can be seen, the positive scores have kept always higher than the negative scores over time while objective score is visibly much higher than both scores. This plot seems to support the finding steampowered.com has found as "overwhelmingly positive."

![aggregate data over time](plot_over_time.png)

Exploring aggregated values over time yields intriguing findings. For example, Fig 3 displays values such as author_vote, votes_funny, and other_votes bewteen 2016 and 2024. It is seen that votes_funny values in 2017 have spiked extraordinarily compared to the rest of the data. It can be speculated that those spikes could be outliers or something new in the game might have caused an enormous increase in reviewes found funny. Another factor would be the count of all the votes in all languages reviewed. 

![aggregate data over time](plot_over_time2.png)

### Ratio of Part of Speech (POS) in the review text
I aimed the comprare the percentages of POS in the dataset. As the following table indicates, nouns are the most frequently used POS category, followed by verbs, adjectives, and adverbs. The values for reviews with less than 1% of these POS corroborate the earlier finding that nouns and verbs are the more frequently used in the reviews compared to adjectives and adverbs. That is, while there are only 18 reviews with less than 1% of nouns, there are 2304 reviews witl less than 1% of adverbs. 

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
Previous work on determining the strength of subjective expressions in text or specifically reviews use various POS such as adjectives, verbs, and nouns. In this analysis, I examined how adjectives relate to the sentiment of the reviews. The Seaborn KDE plot below can be interpreted as a two-dimentional histogram mapping the bivariate distribution between the frequency adjectives and positive sentiment score. It is seen that the most common positive sentiment score is around 0.08 with 5 adjectives. It can be deduced that the higher number of adjectives does not lead more positive sentiment score.

![KDE plot adj_count and positive sentiment](KDE_positive_sentiment.png)

I also looked at the distribution of adjectives in relation to negative sentiment score. It seems that the data is bimodal around 5 adjectives with negative sentiment scores accumulating at 0.01 and 0.08 respectively.

![KDE plot adj_count and negative sentiment](KDE_negative_sentiment.png)

### Correlations
I examined the Spearman correlations between the variables of interest. When you run the script, you can find all the correlation pairs. However, I listed the most interesting correlations over 0.5 threshold. As the following table shows, num_sentences is highly correlated with verb_count, noun_count, and adj_count. Please note that there are other correlation pairs that can be found when yiou run the data.

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

The correlation matrix below indicates that part-of-speech variables (e.g., adj_count, verb_count, noun_count) are highly correlated with each other. In addition, as can be seen weighted_vote_score and other_votes are also higly correlated, which is understandable considering that the reviews that receive positive votes should have higheer weighted_vote_score. I expect correlations between votes_funny and weighted_vote_score, but it did not result in my expectation. 

![Correlation matrix](correlation_matrix.png)

I looked at the relationship between num_sentences and word_count via a scatterplot with a regression line. As the plot shows, the number of sentences increases as the word count increases. As expected, there is a strong positive relationship. 

![Scatterplot with a regression line](scatterplot_regression_line.png)

Using the plotly, I also examined the relationship between avg_words_per_sentence and num_sentences interactively. The following interactive plot showcases it.

![Interactive plot](interactive_plotly.png)

## How to analyze your own game
So far you have seen a walk-through of the review through a specific game (Stardew Valley). Now this section provides instructions about how you can analyze your own game.

- find the steam app id of for game whose review you will analyze. To do this, go to the following link and search for the game in the search bar. This will take you to the game page (URL). You can locate numeric app id in the URL.
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
- sometimes log axis makes a relationship clearer. While working on the 
- select a appropriate time span for accumulations. Depending on the game and specificities in your case, you can select monthly, weekly, or daily spans. 
- try to use pairs of variables that have more than 0.5 r-squared. In this way, more meaningful relationships could be examined. 


#### Setup 

#### File Structure

<img width="390" alt="Screenshot 2024-07-31 at 11 08 52 PM" src="https://github.com/user-attachments/assets/3d323a1a-e5d3-4d51-b889-d52f82cc5065">


Ensure the following file structure is maintained. Please note that stardew_valley is a game whose review is scraped and analyzed in this project. However, your project might include different games so your file structure may change. Make sure that you have three csv files. One has the scraped reviews, another has the results, and the extended.csv includes the variables you are interested in your project as well.
 
By following this guide, you should be able to successfully set up, run, and utilize the review analysis project. For any further assistance, refer to the detailed documentation or contact the project maintainers.
![image](https://github.com/user-attachments/assets/a2e39f4a-7055-4954-83de-994a6a21d90a)
