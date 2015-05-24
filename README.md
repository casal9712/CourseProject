# README for R script to generate MeanIndSubGrp.txt data for Course Project

## This script takes the following data: Training sets, Test sets, Training label, Test label, Training subjects ID, Test subjects ID and merged them into one file to compute the mean of the mean and standard deviation for each feature selection.
Resulting output contain a data with the computed means group by each activity and volunteer in the experiment
(Source: "Human Activity Recognition Using Smartphones Data Set" from UCI Machine Learning Repository;
link: http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones)

###set working directory
	setwd("C:\\Users\\m i n g\\Desktop\\Getting and Cleaning data\\Course Project")

###read in all necessary data
	train_sub<-read.table("./UCI HAR Dataset/train/subject_train.txt")
	train_lab<-read.table("./UCI HAR Dataset/train/Y_train.txt")
	train<-read.table("./UCI HAR Dataset/train/X_train.txt")
	test_sub<-read.table("./UCI HAR Dataset/test/subject_test.txt")
	test_lab<-read.table("./UCI HAR Dataset/test/Y_test.txt")
	test<-read.table("./UCI HAR Dataset/test/X_test.txt")

###load necessary library "dplyr" to perform following task
	library(dplyr)
###merge all training set together (subject ID, activity, and actual measurements)
	train_sub<-rename(train_sub,ID=V1)
	train_lab<-rename(train_lab,activity=V1)
	train_all<-cbind(train_sub,train_lab,train)
###merge all test set together (subject ID, activity, and actual measurements)
	test_sub<-rename(test_sub,ID=V1)
	test_lab<-rename(test_lab,activity=V1)
	test_all<-cbind(test_sub,test_lab,test)
###merge training and test sets together
	alldata<-rbind(train_all,test_all)

###read in the list of values for later assigning the variables names for the alldata
	feat<-read.table("./UCI HAR Dataset/features.txt")

###load necessary library "dplyr" to perform following task
	library(tidyr)
###identify which of the feature contains keywords "mean" and "std" and assign it to a list
	feat_sep<-separate(feat,V2,1:3,sep="-",extra="merge")
	lista<-which(feat_sep[,3] %in% c("mean()","std()"))
	listb<-which(feat_sep[,3] %in% c("mean()","std()"))+2	
	listc<-c(1:2,listb)
###subset all data using the identified list
	sub<-alldata[,listc]

###extract the identified names from the list of names and assign them to the data
	varname<-feat[lista,2]
	varname_lista<-as.character(varname)
	varname_listb<-c("ID","activity",varname_lista)
	names(sub)<-varname_listb

###create a new variables that recode numeric activity to a more descriptive variable: activitycat
	attach(sub)
	sub$activitycat[activity==1]<-"WALKING"
	sub$activitycat[activity==2]<-"WALKING_UPSTAIRS"
	sub$activitycat[activity==3]<-"WALKING_DOWNSTAIRS"
	sub$activitycat[activity==4]<-"SITTING"
	sub$activitycat[activity==5]<-"STANDING"
	sub$activitycat[activity==6]<-"LAYING"
	detach(sub)
###take out the numeric activity
	finalsub<-subset(sub,select=-activity)

###perform means on all variables group by activitycar and ID and output it to a data
	ind<-group_by(finalsub, activitycat, ID) %>% summarise_each(funs(mean))

### Independent tidy data to be created below with the means of each variable for each
### combination of activity and subject:
write.table(ind,"MeanIndSubGrp.txt",row.name=FALSE)
