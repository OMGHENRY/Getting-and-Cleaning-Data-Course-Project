# Getting-and-Cleaning-Data-Course-Project

unzip("./dataset.zip")

### test data
XTest<- read.table("./UCI HAR Dataset/test/X_test.txt")

YTest<- read.table("./UCI HAR Dataset/test/Y_test.txt")

SubjectTest <-read.table("./UCI HAR Dataset/test/subject_test.txt")

### train data:
XTrain<- read.table("./UCI HAR Dataset/train/X_train.txt")

YTrain<- read.table("./UCI HAR Dataset/train/Y_train.txt")

SubjectTrain <-read.table("./UCI HAR Dataset/train/subject_train.txt")

### features and activity

features<-read.table("UCI HAR Dataset/features.txt")

activity<-read.table("UCI HAR Dataset/activity_labels.txt")


##1. Merges the training and the test sets to create one data set.

X<-rbind(XTest, XTrain)

Y<-rbind(YTest, YTrain)

Subject<-rbind(SubjectTest, SubjectTrain)

##2. Extracts only the measurements on the mean and standard deviation for each measurement.

index<-grep("mean\\(\\)|std\\(\\)", features$V2)

X<-X[,index]

##3. Uses descriptive activity names to name the activities in the data set

Y[,1]<-activity[Y[,1],2] ## replacing numeric values with lookup value from activity.txt


##4. Appropriately labels the data set with descriptive variable names.

names_of_feat<-features[index,2] ## getting names for variables

names(X)<-names_of_feat

names(Subject)<-"SubjectID"

names(Y)<-"Activity"

CleanedData<-cbind(Subject, Y, X)

##5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

library(data.table)

CleanedData<-data.table(CleanedData)

TidyData <- CleanedData[, lapply(.SD, mean), by = 'SubjectID,Activity']

write.table(TidyData, file = "Final.txt", row.names = FALSE)
