# Capstone
## Creating an Iterated Prisoner's Dilemma Strategy Using Machine Learning Techniques
My Spring 2025 Data Analytics Capstone repository

The goal is to create a machine learning model to compete against traditional strategies in an iterated prisoner's dilemma tournament.

This repository uses the [Axelrod](https://github.com/Axelrod-Python/Axelrod) Python package to run the prisoner's dilemma tournament and track results

## [Tableau Story With Summarized Results](https://public.tableau.com/app/profile/james.wallace3109/viz/CapstoneStory_17465611131830/Story1?publish=yes)

### Tableau Radar Plots (Not Supported by Tableau Public)

#### Decision Rates of MyStrategy
![image](https://github.com/user-attachments/assets/9b121e1f-e996-483a-b9d7-2998b333471a)

#### Outcome Rates of MyStrategy
![image](https://github.com/user-attachments/assets/a6c2295d-35e7-4b1c-ac2c-1a7d414dfea7)

Rates of other strategies can be found in the CapstoneStory.twb file

## Additional Visualizations

### Median Scores of each strategy across 100 tournaments
![image](https://github.com/user-attachments/assets/816e55ff-4399-472e-9928-d38d61872791)

### Wins in each tournament over 100 tournaments
![image](https://github.com/user-attachments/assets/7a35f74d-4413-48d2-b11b-15a552cc9a93)

### Payoff Matrix
![image](https://github.com/user-attachments/assets/18153b04-ce9c-4331-aeac-b495ea1c5461)


## Description of Files

The only files required to run a tournament with this strategy are the FinalTournament.ipynb file and optionally the model_base.csv file that it references for model creation. The second one is optional as FinalTournament file imports the model_base.csv file from this github repository. The rest of the files show the steps made to create generate a data file, transform it, and test models. 

### Sample Tournament

#### SampleTournament.ipynb
The first tournament I ran using the strategies from Axelrod's first tournament to obtain data with which to train models

#### SampleTournament.csv
Raw data output for this first tournament

#### SplitActions.csv
Initial transformation of the raw data file to split "Actions" column from one string into 200 columns with a 1 or 0 to indicate each strategies action each turn

### SplitActions.ipynb
Python (Jupyter Notebook) code used to create SplitActions.csv file and perform transformations

### splitactions_dropcolumns.sql
SplitActions.csv saved in an sql file with most of the columns not necessary for model creation removed.

This could have been done in Python, but I decided to use SQL as an opportunity to learn how to use a PostgreSQL server on my own machine and DBeaver to import the data and trim the file down.

### model_base.csv
Fully transformed csv file that I am using for model creation

### FirstModels.ipynb
Initial attempts to create and evaluate models to predict opponents next moves and final scores

I attempted to use a Logistic Regression model to predict opponents future moves and evaluated both a Linear Regression and an ANN model to predict final scores.

Logistic Regression showed promising accuracy and Linear Regression models showed lower average errors compared to ANN model.

Primary issue with these models was the inability to use the full dataset to train them because of the way I formatted the data. 

### LookbackModels.pynb

Continued to utilize Linear Regression and Logistic Regression models but elected to not create any more ANN models as I am inexperienced with them and they did not prove to be significantly better than Linear Regression models.

Used a lookback variable to reformat data into shapes taken at intervals specified by the lookback variable, N, tied to their respective final scores.

For example, for N = 20 as my lookback variable the data is reshaped into 180 rows for each game from turns 20 to 200 with each set of 20 turns used as predictors for the final scores of their respectives games for both strategies. 

Proved much more successful than previous models and repeated this process for Logistic Regression model, though this model's accuracy did not change much.

### FinalTournament

#### FinalTournament_1iteration.csv and summary.csv
The former csv file contains the raw data, similar to my starting point, showing each action from each strategy for each game. However, this could only be saved when the tournament had 1 iteration. 

summary.csv takes the summarized results from 100 iterations of this tournament included my custom strategy.

#### FinalTournament.ipynb

Takes models from LookbackModels.ipynb and uses them to predict future moves from opposing strategies and final scores in a strategy. Initial plan was to physically add this strategy to a fork of the Axelrod python package, but I discovered that I can simply create the strategy in this file and add it to a group of strategies in the package. 

### CapstoneStory.twb

Tableau workbook for public tableau visualizations, needed for radar plots along with LaDataViz extension.

### FinalReport.docx

Written paper describing process, findings, and conclusion with more in depth descriptions than provided here. 
