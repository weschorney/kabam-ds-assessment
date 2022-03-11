# Kabam Take-Home Assignment Summary

See the Jupyter notebook in this repository for the entire work. Broadly, our approach can be divided into three sections: reading in the data, preprocessing/EDA, and modelling.

## Introduction

We are asked to identify those users who are likely to spend money in the game after finishing the tutorial, and given a few tables of game data in order to answer this question. Translated into a data science problem, we propose the following:

**Solution:** Create a model that can tell whether a user, after having finished the tutorial, is likely to convert or not. This way, different prices can be given to those users who are likely to spend money in the game. 

## Reading in the data

We are given data in three formats: `.csv`, `.parquet`, and `.db`. The `pandas` library naturally supports reading `.csv` and `.parquet` files, so we read these in using pandas. For the `.db` file, we use the `sqlite3` library to connect to the `.db` file. Next, we pull each row of the data, selecting all of the columns (since a priori, we cannot eliminate any), before transforming these into a DataFrame object. We then create a `converted` column (indicating whether a given user has spent money in the store or not), and create a DataFrame including only those users who have finished the tutorial (since these are the users of interest, based on the question).

## Preprocessing/EDA

Because the amount of NaN values is so small compared to the size of our data, we drop them. Next, we look at the amount of converted users and note that this is an imbalanced classification problem. We also look at those users who have completed the tutorial, and are relieved that we have a sufficient amount of data. Finally, we turn our attention to keeping or dropping columns. Since we have a limited amount of time with the assessment, we take a somewhat naïve approach, where we look at the distributions of converted versus non-converted users for each column, and keep those columns where they look sufficiently different. This produces figures such as:

[alt](./figures/distribution.png)

For some categorical columns, we also reduce the number of unique entries in them, by keeping the most popular entries, and grouping all less popular entries together (since we will have to one-hot encode some of the columns, we want to reduce the width of our data as much as possible).

## Modelling

We scale the data, then oversample using SMOTE and downsample the majority class. We test an XGBoost model as a baseline, and train a neural network. Because we are comparing models, we ensure to use a validation dataset to prevent biased results when testing. Based on the performance, the neural network seems to perform better (although this could be analyzed further with estimated costs for different accuracies/inaccuracies). An additional bonus with the neural network is we can output a probability of conversion of users, instead of just a 0 or 1. 

For more details, please see the notebook, where code and reasoning is given in detail.