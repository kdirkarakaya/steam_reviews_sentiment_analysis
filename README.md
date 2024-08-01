This project is designed to analyze reviews of games on SteamReview platform.  It involves scraping review data, processing it, and visualizing various statistics and correlations. This guide will walk you through the necessary steps to set up and run the project.

Prerequisites:

Before you begin, ensure you have the following installed on your system:
•	Python 3.x (I used 3.12)
•	pip (Python package installer)

Installation
After cloning the repo, install the required Python packages by running:
•	pip install requests pandas seaborn matplotlib plotly

Setup

API Keys If your project requires any API keys, make sure to obtain them and store them in a secure place. For this project, no API keys are needed as it operates on a local CSV file. However, it is likely that you may need API keys to be able to scrape the reviews of interest. To do so, use the store.steampowered.com/appreviews/{appid}

<img width="390" alt="Screenshot 2024-07-31 at 11 08 52 PM" src="https://github.com/user-attachments/assets/3d323a1a-e5d3-4d51-b889-d52f82cc5065">

File Structure

Ensure the following file structure is maintained. Please note that stardew_valley is a game whose review is scraped and analyzed in this project. However, you project might include different games so your file structure may change. Make sure that you have three csv file. One has the scraped reviews, another has the results, and the extended.csv includes the variables you are interested in your project as well.
 
Running the Project
1.	Load the Processed CSV File Ensure that the reviews_stardew_valley_extended.csv file is in the correct directory (stardew_valley/).

2.	Run the Script
Execute the main script to perform data analysis: python main.py 
Note: you may have a different file name; make sure to adjust. If you use Jupiter notebook, you your notebook contains a working run through all the cells. 
Using the Project
This project is comprised of several parts.

A.	Basic Information and Data Exploration

Display Basic Information: The script will print basic information about the dataset:
Sentence and Word Count Analysis: The script will analyze and display the number of sentences and average words per sentence in each review:
Summary Statistics: Display summary statistics of numeric data in the dataset.

B.	Aggregating and Visualizing Data

Aggregation Over Time: Aggregate data over different time periods (e.g., monthly):
Plotting Data Over Time: Use Seaborn and Plotly to visualize aggregated data over time.
Distribution of Numerical Variables: Visualize the distribution of numerical variables using histograms.
Percentage Analysis Calculate and visualize the percentage of different parts of speech in reviews

C.	Correlation Analysis

Calculate and display correlation matrices:

D.	Interactive Visualizations

Interactive Scatterplots: Create interactive scatterplots using Plotly:

Troubleshooting

Common Issues
1.	Missing Files or Directories: Ensure that all necessary files are in the correct directories as per the file structure mentioned above.
2.	Module Not Found Errors: If you encounter errors related to missing modules, ensure that all required packages are installed using pip install.

Developer Notes

Future Enhancements

•	Implement additional data visualization techniques.
•	Enhance error handling and user feedback.
•	Expand analysis to include other sentiment analysis metrics.

Known Issues

•	Some visualizations may become cluttered with large datasets.
•	Outliers (e.g. May 2017) may pop up. Make sure to look into such data points. 
By following this guide, you should be able to successfully set up, run, and utilize the review analysis project. For any further assistance, refer to the detailed documentation or contact the project maintainers.
![image](https://github.com/user-attachments/assets/a2e39f4a-7055-4954-83de-994a6a21d90a)
