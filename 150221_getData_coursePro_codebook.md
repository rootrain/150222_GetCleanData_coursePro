---
title: "150221_GettingAndCleaningData_courseProject_codebook"
output: html_document
---
###project name 
getting and cleaning data course project

###purpose
This project has been conducted to make tudy data set using Human Activity Recognition Using Smartphones Dataset[1].

###variables in the project

The data are normalized and there is no unit.
Information of variables using in this analysis are shown below, but more detail are described in the features_info.txt in the datafolder[2].

The features selected for this database come from the accelerometer and gyroscope 3-axial raw signals tAcc-XYZ and tGyro-XYZ. These time domain signals (prefix 't' to denote time) were captured at a constant rate of 50 Hz. Then they were filtered using a median filter and a 3rd order low pass Butterworth filter with a corner frequency of 20 Hz to remove noise. Similarly, the acceleration signal was then separated into body and gravity acceleration signals (tBodyAcc-XYZ and tGravityAcc-XYZ) using another low pass Butterworth filter with a corner frequency of 0.3 Hz. 

Subsequently, the body linear acceleration and angular velocity were derived in time to obtain Jerk signals (tBodyAccJerk-XYZ and tBodyGyroJerk-XYZ). Also the magnitude of these three-dimensional signals were calculated using the Euclidean norm (tBodyAccMag, tGravityAccMag, tBodyAccJerkMag, tBodyGyroMag, tBodyGyroJerkMag). 

Finally a Fast Fourier Transform (FFT) was applied to some of these signals producing fBodyAcc-XYZ, fBodyAccJerk-XYZ, fBodyGyro-XYZ, fBodyAccJerkMag, fBodyGyroMag, fBodyGyroJerkMag. (Note the 'f' to indicate frequency domain signals). 

These signals were used to estimate variables of the feature vector for each pattern:  
'-XYZ' is used to denote 3-axial signals in the X, Y and Z directions.

tBodyAcc-XYZ
tGravityAcc-XYZ
tBodyAccJerk-XYZ
tBodyGyro-XYZ
tBodyGyroJerk-XYZ
tBodyAccMag
tGravityAccMag
tBodyAccJerkMag
tBodyGyroMag
tBodyGyroJerkMag
fBodyAcc-XYZ
fBodyAccJerk-XYZ
fBodyGyro-XYZ
fBodyAccMag
fBodyAccJerkMag
fBodyGyroMag
fBodyGyroJerkMag

The set of variables that were estimated from these signals are: 

mean(): Mean value
std(): Standard deviation
mad(): Median absolute deviation 
max(): Largest value in array
min(): Smallest value in array
sma(): Signal magnitude area
energy(): Energy measure. Sum of the squares divided by the number of values. 
iqr(): Interquartile range 
entropy(): Signal entropy
arCoeff(): Autorregresion coefficients with Burg order equal to 4
correlation(): correlation coefficient between two signals
maxInds(): index of the frequency component with largest magnitude
meanFreq(): Weighted average of the frequency components to obtain a mean frequency
skewness(): skewness of the frequency domain signal 
kurtosis(): kurtosis of the frequency domain signal 
bandsEnergy(): Energy of a frequency interval within the 64 bins of the FFT of each window.
angle(): Angle between to vectors.

Additional vectors obtained by averaging the signals in a signal window sample. These are used on the angle() variable:

gravityMean
tBodyAccMean
tBodyAccJerkMean
tBodyGyroMean
tBodyGyroJerkMean



###data
The data using in this analysis was obtained from [2] and more detailed information including the process of obtaining data are [4].


###transformation
The data in [2] were already transformed, more detailed information are shown in the features_info.txt in the datafolder[2].
In this analysis, we did not any transformation.

###work
All procedures almost followed the instruction of this course project[3].

1,making merged dataset
Download the data from [2].
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

[1] Davide Anguita, Alessandro Ghio, Luca Oneto, Xavier Parra and Jorge L. Reyes-Ortiz. Human Activity Recognition on Smartphones using a Multiclass Hardware-Friendly Support Vector Machine. International Workshop of Ambient Assisted Living (IWAAL 2012). Vitoria-Gasteiz, Spain. Dec 2012, Jorge L. Reyes-Ortiz, Alessandro Ghio, Luca Oneto, Davide Anguita. November 2012.

[2]https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

[3]https://class.coursera.org/getdata-011/human_grading/view/courses/973498/assessments/3/submissions

[4]http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones
