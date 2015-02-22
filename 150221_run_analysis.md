---
title: "150221 getting and cleaning data Course project"
output: html_document
---
###background and course project instruction
The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The
goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on a
series of yes/no questions related to the project. You will be required to submit: 1) a tidy data set as
described below, 2) a link to a Github repository with your script for performing the analysis, and 3) a
code book that describes the variables, the data, and any transformations or work that you performed to
clean up the data called CodeBook.md. You should also include a README.md in the repo with your
scripts. This repo explains how all of the scripts work and how they are connected.

One of the most exciting areas in all of data science right now is wearable computing see
for example
this article (http://www.insideactivitytracking.com/datascienceactivitytrackingandthebattlefortheworldstopsportsbrand/).
Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most
advanced algorithms to attract new users. The data linked to from the course website represent data
collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available
at the site where the data was obtained:
http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones

Here are the data for the project:
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

You should create one R script called run_analysis.R that does the following.
1. Merges the training and the test sets to create one data set.
2. Extracts only the measurements on the mean and standard deviation for each measurement.
3. Uses descriptive activity names to name the activities in the data set
4. Appropriately labels the data set with descriptive variable names.
5. From the data set in step 4, creates a second, independent tidy data set with the average of each
variable for each activity and each subject.

###analysis


1. Merges the training and the test sets to create one data set.
At first, we read the 6 data(x_train.txt,y_train.txt,sub_train.txt,x_test.txt,y_test.txt,sub_test.txt) from the folder downloaded from https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip into x_train, y_train, sub_train, x_test, y_test, sub_test.
We checked the dimension of these objects and merge the dataset to make trainset and testset.
After that, by merging trainset and testset, make whole dataset called all.

```r
x_train<-read.table("./train/X_train.txt")
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2-1.png) 

```
## Error in UseMethod("depth") : 
##   no applicable method for 'depth' applied to an object of class "NULL"
```

```r
#head(x_train)
#dim(x_train) #7352,561

y_train<-read.table("./train/y_train.txt")
#dim(y_train) #7352,1
#head(y_train)

sub_train<-read.table("./train/subject_train.txt")
#dim(sub_train) #7352,1
#head(sub_train)

trainset<-cbind(x_train,y_train,sub_train)
#head(trainset)
#dim(trainset) #7352,563

x_test<-read.table("./test/X_test.txt")
#head(x_test)
#dim(x_test) #2947,561

y_test<-read.table("./test/y_test.txt")
#dim(y_test) #2947,1
#head(y_test)

sub_test<-read.table("./test/subject_test.txt")
#dim(sub_test) #2947,1
#head(sub_test)

testset<-cbind(x_test,y_test,sub_test)
#dim(testset) #2947,563


#merge test and train dataset

all<-rbind(trainset,testset)
#head(all)
#dim(all) #10299,563
```

2. Extracts only the measurements on the mean and standard deviation for each measurement.
We calculated the mean or sd of each variable, and put the result into mn or s.

```r
mn<-apply(all[1:561],2,mean)
s<-apply(all[1:561],2,sd)
#length(mn)
#head(mn)
#head(s)
```
3. Uses descriptive activity names to name the activities in the data set
Using activity_labels.txt, we changed the y names to activity names.

```r
#names(all)
all[,562]<-gsub("1","WALKING",all[,562])
all[,562]<-gsub("2","WALKING_UPSTAIRS",all[,562])
all[,562]<-gsub("3","WALKING_DOWNSTAIRS",all[,562])
all[,562]<-gsub("4","SITTING",all[,562])
all[,562]<-gsub("5","STANDING",all[,562])
all[,562]<-gsub("6","LAYING",all[,562])
#all[,562]
```

4. Appropriately labels the data set with descriptive variable names.
We used features.txt to label the variables.


```r
nameList<-read.table("features.txt")
nameList_vec<-nameList[,2]
#length(nameList_vec) #561.
names(all)<-c(as.character(nameList_vec),"actionType","subjectName")
#names(all)
```


5. From the data set in step 4, creates a second, independent tidy data set with the average of each
variable for each activity and each subject.
We calculated the mean of each variable for each activity and each subject, and put the result into mn_sub object.


```r
mn_sub<-(all[1:30,1:561]) #dammy data
names(mn_sub)<-names(all[,1:561])

for (i in 1:30){
subdf<-subset(all,all[,563]==i)
mn_sub[i,]<-as.vector(apply(subdf[,1:561],2,mean))
}
#mn_sub
#dim(mn_sub) #30,561
#head(mn_sub)
```

6,write the dataframe into the .txt file.

```r
write.table(mn_sub,"mn_s_data.txt",row.names=FALSE)
```


