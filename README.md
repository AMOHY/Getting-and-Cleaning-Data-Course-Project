I've only one script and below is a summary for the Code
***Step 1
#reading trainging set data
X_train <- read.table("./Data/X_train.txt")
#reading testing set  data
X_test <- read.table("./Data/X_test.txt")
#reading training label data
Y_train <- read.table("./Data/Y_train.txt")
#reading testing label data
Y_test <- read.table("./Data/Y_test.txt")
#reading testing subjects data
sub_test <- read.table("./Data/subject_test.txt")
#reading training subjects data
sub_train <- read.table("./Data/subject_train.txt")
#reading featutres and activity names
feat <- read.table("./Data/features.txt")
act <-  read.table("./Data/activity_labels.txt")
#creating deferentiation between between training and testing data
train_size <- rep("Train",each=7352)
test_size <- rep("Test",each=2947)
#combining all Training data in one data set
train <- cbind(X_train,Y_train,sub_train,train_size)
#combining all testing data in one data set
test <- cbind (X_test,Y_test,sub_test,test_size)
#combing testing and training data
names(train) <- names(test)
Data <- rbind(train,test)
***Step 2
#extracting means for each variable
Means <- colMeans(Data[,1:561])
#extracting standard deviation for each variable
R <- apply(Data[,1:561], 2, sd)
***Step 3
#Uses descriptive activity names to name the activities in the data set
final_data =merge(Data,act,by.x="activity",by.y="V1",all=TRUE)
***Step 4
#changing naming of the column according to the features names
values <- C("activity","subject","type")
N<-feat[,2]
N_1 <- as.character(N)
N_1 <- append(N_1,values)
names(Data)<-N_1
***Step 5
#choose unique names for the columns
valid_column_names <- make.names(names=names(final_data), unique=TRUE, allow_ = TRUE)
names(final_data) <- valid_column_names
#independent tidy data set with the average of each variable for each activity and each subject.
final_data_1 <- final_data %>% group_by(activity,subject)
final_data_2 <- final_data_1 %>% summarise_each(funs(mean))
***Exporting Step
#exporting fina_data_mean to an output file
write.table(final_data_2,file="./data/output.txt",row.names=FALSE,col.names = TRUE,sep="\t",quote=FALSE)