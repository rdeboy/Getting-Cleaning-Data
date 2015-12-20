---
title: "README.md"
author: "Robert DeBoy"
date: "December 15, 2015"
output: html_document
---

HOW I OBTAINED THE RAW MATERIALS:
```{r}
fileURL <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(fileURL, destfile = "smartphone.zip", method = "libcurl")
#"Tue Dec 15 14:39:21 2015"
unzip("smartphone.zip")
#dir("UCI HAR Dataset/")
#[1] "activity_labels.txt" "features_info.txt"   "features.txt"        "README.txt"         
#[5] "test"                "train" 

```

PREPARE TO DOWNLOAD AND READ THE RAW FILES:
Set the working directory to "UCI HAR Dataset"
```{r}
getwd()
#[1] "/Users/Bob/Desktop/Coursera/3_Getting & Cleaning/Project/UCI HAR Dataset"
```

Read the raw files in the parent directory: "UCI HAR Dataset":
```{r}
#Link the class labels (1:6 in testSet and trainingSet) to their descriptive factor name.
activityLabels <- read.table("./activity_labels.txt")
#dim(activityLabels)     [1] 6 2
#Will use below to transform "testLabels" and "trainingLabels"

#List of all features, which are the names of the variables for the data, and...
#can be used as the column headers for both "trainingSet" and "testSet"
features <- read.table("./features.txt")
#dim(features) [1] 561   2
colnames(features) <- c("num", "name")
```

Read the raw files in the "test" directory:
```{r}
#The test data: 2947 observations of 6 types of activities by 21 people for 561 variables
testSet <- read.table("./test/X_test.txt")
#dim(testSet)  [1] 2947  561

testLabels <- read.table("./test/y_test.txt")
colnames(testLabels) <- "activity"
#dim(testLabels)    [1] 2947    1
#need to tranform 1:6 integer values to factor names (using the data frame "activityLabels")

testSubjects <- read.table("./test/subject_test.txt")
#dim(testSubjects)  [1] 2947    1
```

Read the raw files in the "train" directory:
```{r}
#The training data: 7352 observations of 6 types of activities by 9 people for 561 variables
trainingSet <- read.table("./train/X_train.txt")
#dim(trainingSet)   [1] 7352  561

trainingLabels <- read.table("./train/y_train.txt")
colnames(trainingLabels) <- "activity"
#dim(trainingLabels)     [1] 7352    1
#need to tranform 1:6 values to factor names (using the data frame "activityLabels")

trainingSubjects <- read.table("./train/subject_train.txt")
#dim(trainingSubjects)   [1] 7352    1
```

STEP 1: THE LABELS.
Create a new data frame called "dfLabels" that uses descriptive activity names. Remember that the training labels will go on the top, and the test labels will go on the bottom of the new data frame.

Step #1a: First Factorization (of trainingLabels and testLabels)
For each of the 2 "Labels" data.frames ("testLabels" and "trainingLabels"), tranform the 1:6 integer values to factors (using the data frame "activityLabels"). The 2 transformed objects will be called "LabelsByFactor"
```{r}
factorLabels <- activityLabels$V2  #this list will be used for the transformation

#transform the class of a variable from integer to factor:
testLabels$activity <- factor(testLabels$activity)
trainingLabels$activity <- factor(trainingLabels$activity)
#levels(trainingLabels$activity)   [1] "1" "2" "3" "4" "5" "6"
#levels(testLabels$activity)   [1] "1" "2" "3" "4" "5" "6"

#transform the value of the factor from 1:6 to its corresponding phrase:
#[1] "WALKING"            "WALKING_UPSTAIRS"   "WALKING_DOWNSTAIRS" "SITTING"           
#[5] "STANDING"           "LAYING"
levels(testLabels$activity) <- factorLabels
levels(trainingLabels$activity) <- factorLabels
```

Step #1b: Create a new data frame called "dfLabels" by using rbind to combine "trainingLabels" and "testLabels". Remember that the training labels will go on the top, and the test labels will go on the bottom of the new data frame.
```{r}
#dim(trainingLabels)     [1] 7352    1
#dim(testLabels)    [1] 2947    1
dfLabels <- rbind(trainingLabels, testLabels)
#dim(dfLabels) [1] 10299     1
```

STEP #2: THE CATEGORIES.
This is the second Factorization that I will do here.
Create a new data frame called "dfCategories" that contains a "category" variable with factor values of "training" (for the length of the trainingLabels data frame), and "testing" (for the length of the testingLabels data frame). Remember that "training" goes on the top, and "testing" goes on the bottom of the new data frame.
```{r}
category <- c(rep("training", nrow(trainingLabels)), rep("testing", nrow(testLabels)))
dfCategories <- data.frame(category)
#dim(dfCategories)  [1] 10299     1
#transform the class of a variable from character to factor...
#where levels = "testing"  "training"
dfCategories$category <- factor(dfCategories$category)
```

STEP #3: THE DATA.
Create a new data frame called "dfDataNarrow" by following the steps 3a, 3b and 3c. Remember, that the training data will go on the top, and the test data will go on the bottom of the new data frame.
(3a) Use rbind to create a new data frame called "dfData", by combining the 2 data frames "trainingSet" and "testSet".
(3b) Update the column names in dfData using the "features" data frame.
(3c) Create a new data frame called "dfDataNarrow" by selecting only those columns with names containing "mean" or "std"
```{r}
#step 3a (see description above)
dfData <- rbind(trainingSet, testSet)
#dim(dfData)   [1] 10299   561

#step 3b (see description above)
colnames(dfData) <- features$name

#step 3c (see description above)
criteria <- "[-]mean[(][)]|[-]std[(][)]"
dfDataNarrow <- subset(dfData, select = grep(criteria, names(dfData)))
#dim(dfDataNarrow)  [1] 10299    66
```

STEP #4: THE SUBJECTS.
Create a new data frame called "dfSubjects" by using rbind to combine "trainingSubjects" and "testSubjects". Remember that the training subjects will go on the top, and the test subjects will go on the bottom of the new data frame.
```{r}
#dim(trainingSubjects)   [1] 7352    1
#dim(testSubjects)  [1] 2947    1
dfSubjects <- rbind(trainingSubjects, testSubjects)
#dim(dfSubjects)    [1] 10299     1
colnames(dfSubjects) <- "subject"
```

STEP #5: ASSEMBLY OF ONE BIG DATA FRAME.
Create the final, new data frame called "dfFinal" by using cbind to combine the 4 "df" data frames that were created in Steps #1 to #4. Remember to assemble from left to right.
```{r}
dfFinal <- cbind(dfSubjects, dfCategories, dfLabels, dfDataNarrow)
#dim(dfFinal)  [1] 10299    69
#head(names(dfFinal))
#[1] "subject"           "category"          "activity"          "tBodyAcc-mean()-X"
#[5] "tBodyAcc-mean()-Y" "tBodyAcc-mean()-Z"
```

STEP #6: TIDY DATA.
Create a second, independent tidy data set of the average of each variable for each activity and each subject.

Will use the "dbFinal" data frame,
then group_by "activity" and "subject",
then perform mean on columns 4:69, 
and save to dfTidy.
```{r}
dfTidy <- dfFinal %>% group_by(activity, subject) %>% summarise_each(funs(mean), c(4:69))
#dim(dfTidy)   [1] 180  68
#note that I left out the "category" column, which is a factor variable with levels of "training" and "testing". So 68 columns instead of 69.
```

STEP #7: FILE CREATION.
Write the completed data frame to the working directory with write.table() using row.name=FALSE.
```{r}
write.table(dfTidy, row.name=FALSE, file = "tidyDataSet.txt")
```