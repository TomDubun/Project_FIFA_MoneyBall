# OBJECTIVE OF THE PROJECT

Purpose of this project is to run an end-to-end linear regresion in order to predict the market value of the players based on different dependent variables stored in the dataset fifa21_male2.csv in the data folder.

# DATASET EXPLORATION
While exploring the dataset, I have tried to answer three particular questions listed below

1) Which strikers (top 3) below 30 y.o. in France have the best contract (wage) based on their attacking stats
2) Are left foot players more skilled than right foot players as it is usually said by experts ?
3) Which striker nationality has the highest likelihood to score most if I have never seen that striker playing ?

In order to answer question 1, I created a variable named best_contract which is basically filtering the original dataframe with 
* column age below 30 y.o.
* nationality set to France
* best position set to striker
* attacking stats below 306, which is the 3rd interquartile range because 

We want to exclude striker having attacking stats higher than 306 since they are world class players
After filtering my dataframe, I am printing the 3 results sorted by wage in descending order and the conclusion is that french strikers A.Mendy, F.Ayé and M.Nzola are the three most paid strikers with lowest attacking stats.

In order to answer question 2, I have simply used a groupby function based on foot variable and queried the mean skills in column.
I can confirm the hypothesis that left foot players are more skilled than right foot players (283 vs 261 for right foot)

For the third question, I created a variable which is grouping by 5 nationalities having the highest means in attacking stats and then sorting it by attacking stats in descending order. Before that I queried to exclude countries which have less than 40 strikers.
The conclusion is that Morocco, Algeria, Brazil, Ivory Coast and Serbia are countries having strikers with best attacking stats. Thus, as a scoot, I would be more enclined to recruit a striker of these 5 countries, because we could assume that attacking is in their roots.

# DATA CLEANING
In order to perform a linear regression properly, there are some preliminary steps to clean the orignal dataframe. I will list below the steps I undertook.

First things first, I have decided to drop columns which I think did not have any impact on the market value of a player. The decision was made based on my domain knowldege. The variables I decided to keep and the operations I may have performed for each are listed below

* age
* ova
* bp
* pot
* release_clause
* height: 
  * replaced " character by blank
  * converted inches into cm using a lambda function
  * converted into integer
* wage: 
  * replaced € signs by blank, 
  * replacing K and M by *1000 and *1000000
  * converted to float
* release_clause: 
  * replaced € signs by blank, 
  * replacing K and M by *1000 and *1000000
  * converted to float
* pac
* sho
* dri: dropped after correlation matrix step
* pas
* def
* phy
* hits: 
  * replacing K and M by *1000 and *1000000
  * converted to float
* contract_end:
  * variable created based on contract original variable
  * splitted contract variable into two and kept whatever came after character ~ and stored it as variable "contract_end" in a new column. Any string before ~ was stored in a new column as variable "contract_start"
  * converted it to datetime format
  * A lot of strings appeared in the contract_start column. to remove them and keep only date formats here is what I did:
    * drop all on-loan players since I believe that market  for on-loan players is really different than the one for regular contract players (I did that because reason why strings appeared in contract_start column was due to on-loan players)
    * drop any string that appeared less than 20 times in the contract_start column
    * remove all free players from contract_start column
  * remove all players which had a contract ending prior to 2021 - which means droping players retired as we do not need any market value for these players
* value: 
  * replaced € signs by blank, 
  * replacing K and M by *1000 and *1000000
  * converted to float
  * removed players having a value = 0

After cleaning columns, our database dfd is 13 198 rows long compared to 17 125 at the begining. I dropped 23% of my initial dataset

# EDA
After plotting distribution plots, I noticed that data were not normalized. 

I plotted correlation matrix to see whether or not some variables were correlated between each other.

I found out that variables dri and pas were highly correlated (0.83) so I decided to drop dri since it is less correlated to value than pas

# FEATURE ENGINEERING

Next step, I plotted boxplots in order to visualize outliers in the variables

I defined a function to remove outliers on numerical columns based on the interquartile range * +-2

Then I had to encode my categorical variables so they can be part of the linear regression model

Last step of this section was to split dataset between features and target in order to perform normalization

# ANALYZING RESULTS
After performing my regression model, I got an r-squared of 0.9304, which means that 93% of the data selected fit the regression model, which means my model is quite good
