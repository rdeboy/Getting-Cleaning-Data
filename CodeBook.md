---
title: "CodeBook.md"
author: "Robert DeBoy"
date: "December 20, 2015"
output: html_document
---

This document, "CodeBook.md", describes the variables and summaries I made while creating a tidy data set for the project assignment for the online Coursera class called "Getting and Cleaning Data".

The following files should appear in my Github repo:

(1) tidyDataSet.txt
(2) run_analysis.R
(3) REAME.md
(4) CodeBook.md

This data set follows the tidy data principles, as described by Hadley Wickham:

(1) Each variable you measure should be in one column.
(2) Each different observation of that variable should be in a different row.
(3) Each type of observational unit forms a table.

The tidy data set has dimensions of 180 rows (observations) and 68 columns (variables).

The tidy data set has 180 rows (observations) because there are 6 different types of activities (activity column) for 30 different people (subject column).

The 6 types of activities are:

"WALKING" "WALKING_UPSTAIRS" "WALKING_DOWNSTAIRS" "SITTING" "STANDING"  LAYING"

A complete description of the activities and the data collected can be found in the downloaded file called "README.txt".

It is important to note that the tidy data set is NOT the original data, but a SUMMARY of that data. Here are some details about my tidy data set.

The 68 columns (variables) are of three types:

(1) "activity" is what the subject was doing when data was collected from their Samsung Galaxy S smartphone.
(2) "subject" is a number between 1 and 30 that is the identifier for each subject for which data was collected.
(3) "data" in columns (3-68) are the means of the actual data collected from their Samsung Galaxy S smartphones.

Note #1: only 66 data variables were extracted from a larger data set of 561 data variables in the raw data, and these 66 data variables were subsequently summarized so that the tidy data set has only measurements on the mean and standard deviation for each measurement.

Note #2: only 180 rows were summarized from a larger data set of 10,299 observations. Specifically, the means of data were collected for each activity (N=6) and each subject (N=30). That is, there were multiple data measurements for each subject and each activity, but only one summary statistic (mean) is presented in the tidy data set.

Note #3: the tidy data set consists of combined sets of data from "training data" (N=21 subjects) and "testing data" (N=9 subjects). The procedure I used to assemble this data and label it is described in my README file found in the Github repo. Briefly, files were dowloaded and read into data frames that were subsequently assembled for "training" and "testing" using the following pair of files:

(1) The measurements in the tidy data come from "X_train.txt" (training) and X_test.txt" (testing).
