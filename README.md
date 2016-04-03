# CleanData
Getting and Cleaning Data Course Project

##Clean data from "UCI HAR Dataset"
library(dplyr)

# 1.1 Get data from "train" and merge 
train_subject_train = read.table("./UCI HAR Dataset/train/subject_train.txt")
train_y_train = read.table("./UCI HAR Dataset/train/y_train.txt")
train_x_train = read.table("./UCI HAR Dataset/train/x_train.txt")
train <- as.data.frame(cbind(train_subject_train, train_y_train, train_x_train))
# 1.2 Get title from "features"
train_features <- read.table("./UCI HAR Dataset/features.txt")
titles <- c("subject","activity", as.character(train_features$V2))
# 1.3 Set title to train data.frame
train <- `colnames<-`(train,titles)



# 2.1 Get data from "test" and merge 
test_subject_test = read.table("./UCI HAR Dataset/test/subject_test.txt")
test_y_test = read.table("./UCI HAR Dataset/test/y_test.txt")
test_x_test = read.table("./UCI HAR Dataset/test/x_test.txt")
test <- as.data.frame(cbind(test_subject_test, test_y_test, test_x_test))
# 1.2 Get title from "features"
test_features <- read.table("./UCI HAR Dataset/features.txt")
titles <- c("subject","activity", as.character(test_features$V2))
# 1.3 Set title to test data.frame (Appropriately labels the data set with descriptive variable names)
test <- `colnames<-`(test,titles) 


# 3.1 Merge train and test dataset by "subject
train_test <- rbind(train, test)
rm(list = c("test", "train"))

# 4.1 Extracts the mean and standard deviation for each measurement
c_name <- names(train_test)
mean_n <- grep("mean", c_name)
std_n <- grep("std", c_name)
data_mean_std <- cbind(train_test[1], train_test[2], train_test[mean_n], train_test[std_n])
rm(list = c("c_name", "mean_n", "std_n"))

# 5.1 Descriptive activity names to the activities in the data set
activity_labels <- read.table("./UCI HAR Dataset/activity_labels.txt")
for (i in 1:nrow(data_mean_std)){
  data_mean_std[i,2] <- as.character(activity_labels[as.numeric(data_mean_std[i,2]),2])
}
rm(list = c("activity_labels", "train_test"))

# 6.1 Creates a second, independent tidy data set with the average of each variable for each activity and each subject.
dataMelt <- melt(data_mean_std, id.vars = c("subject", "activity"), measure.vars = names(data_mean_std)[3:81])
data_subject <- dcast(dataMelt, subject ~ variable, mean)
data_activity <- dcast(dataMelt, activity ~ variable, mean)
tidy_data_set <- rbind(data_subject, data_activity)

