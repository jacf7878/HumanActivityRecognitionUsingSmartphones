CODEBOOK – JOSE ANTONIO
PROBLEM
The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions related to the project. You will be required to submit: 1) a tidy data set as described below, 2) a link to a Github repository with your script for performing the analysis, and 3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected.
One of the most exciting areas in all of data science right now is wearable computing - see for example this article . Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained:
http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones
Here are the data for the project:
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip
You should create one R script called run_analysis.R that does the following.
1. Merges the training and the test sets to create one data set.
2. Extracts only the measurements on the mean and standard deviation for each measurement.
3. Uses descriptive activity names to name the activities in the data set
4. Appropriately labels the data set with descriptive variable names.
5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

Data Description
The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 
The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain. See 'features_info.txt' for more details. 
For each record it is provided:
======================================
- Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration.
- Triaxial Angular velocity from the gyroscope. 
- A 561-feature vector with time and frequency domain variables. 
- Its activity label. 
- An identifier of the subject who carried out the experiment.
The dataset includes the following files:
========================================
- 'README.txt'
- 'features_info.txt': Shows information about the variables used on the feature vector.
- 'features.txt': List of all features.
- 'activity_labels.txt': Links the class labels with their activity name.
- 'train/X_train.txt': Training set.
- 'test/X_test.txt': Test set.
- 'test/y_test.txt': Test labels.
The following files are available for the train and test data. Their descriptions are equivalent. 
- 'train/subject_train.txt': Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30. 
- 'train/Inertial Signals/total_acc_x_train.txt': The acceleration signal from the smartphone accelerometer X axis in standard gravity units 'g'. Every row shows a 128 element vector. The same description applies for the 'total_acc_x_train.txt' and 'total_acc_z_train.txt' files for the Y and Z axis. 
- 'train/Inertial Signals/body_acc_x_train.txt': The body acceleration signal obtained by subtracting the gravity from the total acceleration. 
- 'train/Inertial Signals/body_gyro_x_train.txt': The angular velocity vector measured by the gyroscope for each window sample. The units are radians/second. 
Code executed:
#q1 - Merges the training and the test sets to create one data set.
#Phase 1.1 - prepare training set - putting all others data together
#1.1.1 Reads features to prepare for setting features columns names
    features <- read.csv("data\\features.txt", header =  F, col.names = c('id','f'), sep = ' ')
    names_features <- as.vector(features$f)
    
#1.1.2 Reads Training Set with 561 features for 7352 measurements of 30 subjects doing 6 types of activities
    x_train <- read.fwf("data\\train\\X_train.txt", header = F, width=c(rep(16,561)), col.names = c(names_features))
    glimpse(x_train)
    rm(features)
#1.1.3 Reads activity labels and append it in training set
    y_train <- read.fwf("data\\train\\y_train.txt", header = F, width=c(1), col.names = c('activity'))
    glimpse(y_train)
    x_train <- cbind(x_train,y_train)
    rm(y_train)    
#1.1.4 Reads volunteer ids and append it in training set
    subject_train <- read.csv("data\\train\\subject_train.txt", header = F, col.names = c('volunteer'))
    glimpse(subject_train)
    x_train <- cbind(x_train,subject_train)
    View(x_train)
    rm(subject_train)    
#1.1.5 Gets inertial signals vector of 128 readings and saves as a column, for each axis (x,y,z) and separating for acceleration,  acceleration total and gyroscope
    #for acceletarion x
    bodyaccx <- read.fwf("data\\train\\Inertial Signals\\body_acc_x_train.txt", header = F, width=c(rep(16,128)))
    glimpse(bodyaccx)    
    #converts each row of signals in a list do save in the respective column 
    x_train$body_acc_x_train <- sapply(bodyaccx, function(x) as.list(x))
    rm(bodyaccx)
    #for acceletarion y
    bodyaccy <- read.fwf("data\\train\\Inertial Signals\\body_acc_y_train.txt", header = F, width=c(rep(16,128)))
    glimpse(bodyaccy)    
    #converts each row of signals in a list do save in the respective column 
    x_train$body_acc_y_train <- sapply(bodyaccy, function(x) as.list(x))
    rm(bodyaccy) 
    #for acceletarion z
    bodyaccz <- read.fwf("data\\train\\Inertial Signals\\body_acc_z_train.txt", header = F, width=c(rep(16,128)))
    glimpse(bodyaccz)    
    #converts each row of signals in a list do save in the respective column 
    x_train$body_acc_z_train <- sapply(bodyaccz, function(x) as.list(x))
    rm(bodyaccz)        
    #for gyroscope x
    bodygyrox <- read.fwf("data\\train\\Inertial Signals\\body_gyro_x_train.txt", header = F, width=c(rep(16,128)))
    glimpse(bodygyrox)    
    #converts each row of signals in a list do save in the respective column 
    x_train$body_gyro_x_train <- sapply(bodygyrox, function(x) as.list(x))
    rm(bodygyrox)    
    #for gyroscope y
    bodygyroy <- read.fwf("data\\train\\Inertial Signals\\body_gyro_y_train.txt", header = F, width=c(rep(16,128)))
    glimpse(bodygyroy)    
    #converts each row of signals in a list do save in the respective column 
    x_train$body_gyro_y_train <- sapply(bodygyroy, function(x) as.list(x))
    rm(bodygyroy)    
    #for gyroscope z
    bodygyroz <- read.fwf("data\\train\\Inertial Signals\\body_gyro_z_train.txt", header = F, width=c(rep(16,128)))
    glimpse(bodygyroz)    
    #converts each row of signals in a list do save in the respective column 
    x_train$body_gyro_z_train <- sapply(bodygyroz, function(x) as.list(x))
    rm(bodygyroz)    
    #for total acceleration x
    totalaccx <- read.fwf("data\\train\\Inertial Signals\\total_acc_x_train.txt", header = F, width=c(rep(16,128)))
    glimpse(totalaccx)    
    #converts each row of signals in a list do save in the respective column 
    x_train$total_acc_x_train <- sapply(totalaccx, function(x) as.list(x))
    rm(totalaccx)    
    #for total acceleration y
    totalaccy <- read.fwf("data\\train\\Inertial Signals\\total_acc_y_train.txt", header = F, width=c(rep(16,128)))
    glimpse(totalaccy)    
    #converts each row of signals in a list do save in the respective column 
    x_train$total_acc_y_train <- sapply(totalaccy, function(x) as.list(x))
    rm(totalaccy)    
    #for total acceleration z
    totalaccz <- read.fwf("data\\train\\Inertial Signals\\total_acc_z_train.txt", header = F, width=c(rep(16,128)))
    glimpse(totalaccz)    
    #converts each row of signals in a list do save in the respective column 
    x_train$total_acc_z_train <- sapply(totalaccz, function(x) as.list(x))
    rm(totalaccz)    
    
#Phase 1.2 - Does the same for testing set - putting all others data together
    
    #1.2.1 Reads features to prepare for setting features columns names
    features <- read.csv("data\\features.txt", header =  F, col.names = c('id','f'), sep = ' ')
    names_features <- as.vector(features$f)
    
    #1.2.2 Reads testing Set with 561 features for 2947 measurements of 30 subjects doing 6 types of activities
    x_test <- read.fwf("data\\test\\X_test.txt", header = F, width=c(rep(16,561)), col.names = c(names_features))
    glimpse(x_test)
    rm(features)
    
    #1.2.3 Reads activity labels and append it in testing set
    y_test <- read.fwf("data\\test\\y_test.txt", header = F, width=c(1), col.names = c('activity'))
    glimpse(y_test)
    x_test <- cbind(x_test,y_test)
    rm(y_test)     
    #1.2.4 Reads volunteer ids and append it in testing set
    subject_test <- read.csv("data\\test\\subject_test.txt", header = F, col.names = c('volunteer'))
    glimpse(subject_test)
    x_test <- cbind(x_test,subject_test)
    rm(subject_test)
    #1.2.5 Gets inertial signals vector of 128 readings and saves as a column, for each axis (x,y,z) and separating for acceleration,  acceleration total and gyroscope
    #for acceletarion x
    bodyaccx <- read.fwf("data\\test\\Inertial Signals\\body_acc_x_test.txt", header = F, width=c(rep(16,128)))
    glimpse(bodyaccx)    
    #converts each row of signals in a list do save in the respective column 
    x_test$body_acc_x_test <- sapply(bodyaccx, function(x) as.list(x))
    rm(bodyaccx)  
    #for acceletarion y
    bodyaccy <- read.fwf("data\\test\\Inertial Signals\\body_acc_y_test.txt", header = F, width=c(rep(16,128)))
    glimpse(bodyaccy)    
    #converts each row of signals in a list do save in the respective column 
    x_test$body_acc_y_test <- sapply(bodyaccy, function(x) as.list(x))
    rm(bodyaccy)    
    #for acceletarion z
    bodyaccz <- read.fwf("data\\test\\Inertial Signals\\body_acc_z_test.txt", header = F, width=c(rep(16,128)))
    glimpse(bodyaccz)    
    #converts each row of signals in a list do save in the respective column 
    x_test$body_acc_z_test <- sapply(bodyaccz, function(x) as.list(x))
    rm(bodyaccz)        
    #for gyroscope x
    bodygyrox <- read.fwf("data\\test\\Inertial Signals\\body_gyro_x_test.txt", header = F, width=c(rep(16,128)))
    glimpse(bodygyrox)    
    #converts each row of signals in a list do save in the respective column 
    x_test$body_gyro_x_test <- sapply(bodygyrox, function(x) as.list(x))
    rm(bodygyrox)        
    #for gyroscope y
    bodygyroy <- read.fwf("data\\test\\Inertial Signals\\body_gyro_y_test.txt", header = F, width=c(rep(16,128)))
    glimpse(bodygyroy)    
    #converts each row of signals in a list do save in the respective column 
    x_test$body_gyro_y_test <- sapply(bodygyroy, function(x) as.list(x))
    rm(bodygyroy)        
    #for gyroscope z
    bodygyroz <- read.fwf("data\\test\\Inertial Signals\\body_gyro_z_test.txt", header = F, width=c(rep(16,128)))
    glimpse(bodygyroz)    
    #converts each row of signals in a list do save in the respective column 
    x_test$body_gyro_z_test <- sapply(bodygyroz, function(x) as.list(x))
    rm(bodygyroz)        
    #for total acceleration x
    totalaccx <- read.fwf("data\\test\\Inertial Signals\\total_acc_x_test.txt", header = F, width=c(rep(16,128)))
    glimpse(totalaccx)    
    #converts each row of signals in a list do save in the respective column 
    x_test$total_acc_x_test <- sapply(totalaccx, function(x) as.list(x))
    rm(totalaccx)    
    #for total acceleration y
    totalaccy <- read.fwf("data\\test\\Inertial Signals\\total_acc_y_test.txt", header = F, width=c(rep(16,128)))
    glimpse(totalaccy)    
    #converts each row of signals in a list do save in the respective column 
    x_test$total_acc_y_test <- sapply(totalaccy, function(x) as.list(x))
    rm(totalaccy)    
    #for total acceleration z
    totalaccz <- read.fwf("data\\test\\Inertial Signals\\total_acc_z_test.txt", header = F, width=c(rep(16,128)))
    glimpse(totalaccz)    
    #converts each row of signals in a list do save in the respective column 
    x_test$total_acc_z_test <- sapply(totalaccz, function(x) as.list(x))
    rm(totalaccz)    
    #Phase 3 - Mergin the two complete data frames - test and train
    #adjust names removing test 
    names_human_act <- colnames(x_test)
    names_human_act <- gsub("_test","",names_human_act)
    colnames(x_test) <- names_human_act
    colnames(x_train) <- names_human_act
    #merges two data frames in one object
    human_act <- rbind(x_test, x_train)
    #q2 - Extracts only the measurements on the mean and standard deviation for each measurement. 
     human_act_mean_std <- human_act[grep('*mean|std*',names_human_act)]
    str(human_act_mean_std)

#q3 - Uses descriptive activity names to name the activities in the data set
    #uses mutate do add a new column with descriptive activity names 
    library(dplyr)
    #uses mutates and ifelse to create a new column with more descriptive activity names, using activity_labels.txt
    human_act <- mutate(human_act, activitydesc = ifelse(activity == 1, 'walking',ifelse(activity == 2, 'walking_upstairs',ifelse(activity == 3, 'walking_downstairs',ifelse(activity == 4, 'sitting',ifelse(activity == 5, 'standing',ifelse(activity == 6, 'laying','-'))))))  )

#q4 - Appropriately labels the data set with descriptive variable names
    names(human_act)<-gsub("std()", "sd", names(human_act))
    names(human_act)<-gsub("mean()", "mean", names(human_act))
    names(human_act)<-gsub("^t", "Time", names(human_act))
    names(human_act)<-gsub("^f", "Frequency", names(human_act))
    names(human_act)<-gsub("Acc", "Accelerometer", names(human_act))
    names(human_act)<-gsub("Gyro", "Gyroscope", names(human_act))
    names(human_act)<-gsub("Mag", "Magnitude", names(human_act))
    names(human_act)<-gsub("BodyBody", "Body", names(human_act))
    head(str(human_act),3)

    #temp- just for testing
    test <- head(human_act,3)
    test$body_acc_x[1,][128]
    class(test$total_acc_z[1,])
    View(test)    
    #q5 - From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.
        library(dplyr)
    human_act_grouped <- group_by(human_act,activity,volunteer)
    human_act_grouped_mean <- summarise_each(human_act_grouped,funs(mean(.,na.rm = T)))
write.table(human_act, "HumanActTidyData.txt", row.name=FALSE)    
    
    

