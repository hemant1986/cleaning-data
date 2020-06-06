library(dplyr)
#read train data
x_train <- read.table("C:\\Users\\hemant jain.JIETJOD\\Desktop\\UCI HAR Dataset\\train\\X_train.txt")
y_train <- read.table("C:\\Users\\hemant jain.JIETJOD\\Desktop\\UCI HAR Dataset\\train\\Y_train.txt")
sub_train <- read.table("C:\\Users\\hemant jain.JIETJOD\\Desktop\\UCI HAR Dataset\\train\\subject_train.txt")
#read test data
x_test <- read.table("C:\\Users\\hemant jain.JIETJOD\\Desktop\\UCI HAR Dataset\\test\\X_test.txt")
y_test <- read.table("C:\\Users\\hemant jain.JIETJOD\\Desktop\\UCI HAR Dataset\\test\\Y_test.txt")
sub_test <- read.table("C:\\Users\\hemant jain.JIETJOD\\Desktop\\UCI HAR Dataset\\test\\subject_test.txt")
#read data descriptors
variables_names <- read.table("C:\\Users\\hemant jain.JIETJOD\\Desktop\\UCI HAR Dataset\\features.txt")
activity_labels <- read.table("C:\\Users\\hemant jain.JIETJOD\\Desktop\\UCI HAR Dataset\\activity_labels.txt")
# 1. Merges the training and the test sets to create one data set.
x_total <- rbind(x_train,x_test)
y_total <- rbind(y_train,y_test)
sub_total <- rbind(sub_train,sub_test)
# 2. Extracts only the measurements on the mean and standard deviation for each measurement.
selected_var <- variables_names[grep("mean\\(\\)|std\\(\\)",variables_names[,2]),]
x_total <- x_total[,selected_var[,1]]
# 3. Uses descriptive activity names to name the activities in the data set
colnames(y_total) <- "activity"
y_total$activitylabel <- factor(y_total$activity, labels = as.character(activity_labels[,2]))
activitylabel <- y_total[,-1]

# 4. Appropriately labels the data set with descriptive variable names.
colnames(x_total) <- variables_names[selected_var[,1],2]
# 5. From the data set in step 4, creates a second, independent tidy data set with the average
# of each variable for each activity and each subject.
colnames(Sub_total) <- "subject"
total <- cbind(x_total, activitylabel, Sub_total)
total_mean <- total %>% group_by(activitylabel, subject) %>% summarize_each(funs(mean))
write.table(total_mean, file = "./UCI HAR Dataset/tidydata.txt", row.names = FALSE, col.names = TRUE)

