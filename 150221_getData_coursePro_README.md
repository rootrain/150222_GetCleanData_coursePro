---
title: "150221_gettingAndCleaningData_courseProject_README"
output: html_document
---
###project name :getting and cleaning data course project

###procedure
The purpose of this project is to make tidy data from [1].
The scripts work at each steps as described below.
All procedures almost followed the instruction of this course project[2].

1,making merged dataset
Download the data from [1].
Read 6 files in train and test folders using read.table() to make test and train data.
At every step, check the dimention of data using dim().

for train data
x_train<-read.table("./train/X_train.txt")
y_train<-read.table("./train/y_train.txt")
sub_train<-read.table("./train/subject_train.txt")

for test data
x_test<-read.table("./test/X_test.txt")
y_test<-read.table("./test/y_test.txt")
sub_test<-read.table("./test/subject_test.txt")

Combine each dataset using(cbind().
trainset<-cbind(x_train,y_train,sub_train)
testset<-cbind(x_test,y_test,sub_test)

Merge train and test dataset using rbind().
all<-rbind(trainset,testset)

2,calculate the mean or standard deviation of each measurement
Using the whole dataset, all, calculate the mean or stadard deviation of each measurement.
mn<-apply(all[1:561],2,mean)
s<-apply(all[1:561],2,sd)

3,change the name of activity column
Using gsub(), change the y names to activity names descripted in activity_labels.txt.

all[,562]<-gsub("1","WALKING",all[,562])
all[,562]<-gsub("2","WALKING_UPSTAIRS",all[,562])
all[,562]<-gsub("3","WALKING_DOWNSTAIRS",all[,562])
all[,562]<-gsub("4","SITTING",all[,562])
all[,562]<-gsub("5","STANDING",all[,562])
all[,562]<-gsub("6","LAYING",all[,562])

4,label the variable in the data set
By using features.txt,label the variables in the dataset.
nameList<-read.table("features.txt")
names(all)<-c(as.character(nameList_vec),"actionType","subjectName")

5,make the new tidy dataframe with average with each variable for each activity for each subject.
Calculate the each variable's mean in each subject using apply().
mn_sub[i,]<-as.vector(apply(subdf[,1:561],2,mean))

Store the dataframe into .txt file using write.table().
write.table(mn_sub,"mn_s_dataset.txt",row.names=FALSE)


###references
[1]https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

[2]https://class.coursera.org/getdata-011/human_grading/view/courses/973498/assessments/3/submissions
