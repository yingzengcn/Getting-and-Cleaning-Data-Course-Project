# Getting-and-Cleaning-Data-Course-Project
final project


## step 1: download zip file from website
if(!file.exists("./data")) dir.create("./data")
fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(fileUrl, destfile = "./data/getCleanData.zip")

## step 2: unzip data
listZip <- unzip("./data/getCleanData.zip", exdir = "./data")

## step 3: load data into R
train.x <- read.table("./data/UCI HAR Dataset/train/X_train.txt")
train.y <- read.table("./data/UCI HAR Dataset/train/y_train.txt")



## step 4: merge train and test data

Extract only the measurements on the mean and standard deviation for each measurement. 
## : load feature name into R
##  extract mean and standard deviation of each measurements
##. Uses descriptive activity names to name the activities in the data set
## load activity data into R
## replace 1 to 6 with activity names
## . Appropriately labels the data set with descriptive variable names.
train.subject <- read.table("./data/UCI HAR Dataset/train/subject_train.txt")
test.x <- read.table("./data/UCI HAR Dataset/test/X_test.txt")
test.y <- read.table("./data/UCI HAR Dataset/test/y_test.txt")
test.subject <- read.table("./data/UCI HAR Dataset/test/subject_test.txt")
trainData <- cbind(train.subject, train.y, train.x)
testData <- cbind(test.subject, test.y, test.x)
fullData <- rbind(trainData, testData)

##step5
## . From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

names(finalData) <- gsub("\\()", "", names(finalData))
names(finalData) <- gsub("^t", "time", names(finalData))
names(finalData) <- gsub("^f", "frequence", names(finalData))
names(finalData) <- gsub("-mean", "Mean", names(finalData))
names(finalData) <- gsub("-std", "Std", names(finalData))
library(dplyr)
groupData <- finalData %>%
    group_by(subject, activity) %>%
summarise_each(funs(mean))
write.table(groupData, file ="./data/MeanData.txt", row.names = FALSE)
