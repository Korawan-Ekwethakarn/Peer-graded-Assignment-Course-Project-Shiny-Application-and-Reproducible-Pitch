# Peer-graded-Assignment-Course-Project-Shiny-Application-and-Reproducible-Pitch
Shiny application and deploy it on Rstudio's servers
head(mtcars)
data <- read.csv("Diamond_price.csv", header=TRUE)
str(data)
data$Price <- gsub('\\$', '', data$Price)
data$Price <- gsub(',', '', data$Price)
mydata <- data[,c(1,2,3,4,5,7,10)]
mydata$Price <- as.numeric(as.character(mydata$Price))
mydata <- mydata[mydata$Price <15000,] # remove outliers
head(mydata)
library(caret)
library(randomForest)
inTrain <- createDataPartition(mydata$Price, p=0.7,list = FALSE)
traindata <- mydata[inTrain,]
testdata <- mydata[-inTrain,]
model.forest <- train(Price~., traindata, method = "rf", trControl = trainControl(method = "cv", number = 3))
testdata$pred <- predict(model.forest, newdata = testdata)
ggplot(aes(x=actual, y=prediction),data=data.frame(actual=testdata$Price, prediction=predict(model.forest, testdata)))+ 
   geom_point() +geom_abline(color="red") +ggtitle("RandomForest Regression in R" )
