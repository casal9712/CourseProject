# README for R script to generate MeanIndSubGrp.txt data for Course Project

## This script takes the following data: Training sets, Test sets, Training label, Test label, Training subjects ID, Test subjects ID
## (Source: "Human Activity Recognition Using Smartphones Data Set" from UCI Machine Learning Repository;
## link: http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones)
## and merged them into one file to compute the mean of the mean and standard deviation for each feature selection.
## Resulting output contain a data with the computed means group by each activity and volunteer in the experiment

#set working directory
#read in all necessary data
#load necessary library "dplyr" to perform following task
#merge all training set together (subject ID, activity, and actual measurements)
#merge all test set together (subject ID, activity, and actual measurements)
#merge training and test sets together
#read in the list of values for later assigning the variables names for the alldata
#load necessary library "dplyr" to perform following task
#identify which of the feature contains keywords "mean" and "std" and assign it to a list
#subset all data using the identified list
#extract the identified names from the list of names and assign them to the data
#create a new variables that recode numeric activity to a more descriptive variable: activitycat
#take out the numeric activity
#perform means on all variables group by activitycar and ID and output it to a data
## Independent tidy data to be created below with the means of each variable for each
## combination of activity and subject:
#write.table(ind,"MeanIndSubGrp.txt",row.name=FALSE)