1. Data was loaded from files "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
   subject.txt is 1-30 numbers for 30 subjects
   activity_labels.txt is 1-6 numbers for 6 activities and the activities of each subject in training and test is stored in y_train.txt or y_test
   oberservations of each subject in each activity is stored in x_train.txt or y_train.txt
   names of each variables are stored in features.txt, with features_info.txt to explain
   readme.txt is helpful
2. subject-activity-x_train(text) were merged to give "train_text" with descriptive variable names
3. variables with "mean" and "std" in their names were extracted and gaven to an other object called "data_mean_std"
4. disdescriptive names for activities were assigned by a for loop, without changing object name (still "data_mean_std")
5. tidy data was generated with melt and dcast functions, and was named "tidy_data_set"
