---
title: "Data Science Capstone : NLP ~ Natural Language Processing ~"
output: html_document
---
1. **Understanding the problem**
1. **Data acquisition and cleaning**
1. Exploratory analysis
1. Statistical modeling
1. Predictive modeling
1. Creative exploration
1. Creating a data product
1. Creating a short slide deck pitching your product

### loading data
Data are in [this link](https://d396qusza40orc.cloudfront.net/dsscapstone/dataset/Coursera-SwiftKey.zip)

trick : split the document to speed test our algo 
into 10M files starting with x000

> system('split ./en_US/en_US.twitter.txt --bytes=10M -d --numeric-suffixes --suffix-length=3 --additional-suffix="en_US.twitter.txt"')
>

Or as written in [TASK One](https://www.coursera.org/learn/data-science-project/supplement/IbTUL/task-1-getting-and-cleaning-the-data) readlines by steps...
```{r}
linesTOread<-10000
con <- file("en_US/en_US.twitter.txt", "r") 
twitter<-readLines(con, linesTOread) ## Read the N first line of text 
twitter<-readLines(con, linesTOread) ## Read the N next line of text 
close(con) ## It's important to close the connection when you are done. 
```

or just **_Recommanded_** pick a sample with ?sample

```{r}
### hidden to speed lecture of code ###
blog <- readLines("en_US/en_US.blogs.txt",skipNul = TRUE, warn = TRUE)
news <- readLines("en_US/en_US.news.txt",skipNul = TRUE, warn = TRUE)
twitter <- readLines("en_US/en_US.twitter.txt", linesTOread, skipNul = TRUE, warn = TRUE)
#print(twitter)
```

Now we are getting a brief summary of our files. Note: I choose the en_US directory only
**THIS MAKE TAKE SOME TIME TO BE EXECUTED**

```{r}
myFiles<-NULL
myFiles$file<-dir("en_US")[1:3] #I have some other files in this directory thats why I select only the 3 ones
print(myFiles)
#add the directory
for (i in 1:3){ 
  myFiles$file[i]<-paste("en_US/",myFiles$file[i],sep = "")
  myFiles$nrow[i]<-NROW(readLines(myFiles$file[i], 1050000, skipNul = TRUE))# you should delete 1050000 to have the True result
  myFiles$sizeMB[i]<-round(file.info(myFiles$file[i])['size']/1024**2, digits = 3)
  myFiles$sizeMB[i]<-myFiles$sizeMB[i]
  print(paste("File : ", myFiles$file[i], "has", myFiles$nrow[i], " rows and size : ", myFiles$sizeMB[i], "MB"))
  
}

print("In blog file")
print(paste("The line number",  M<-which.max(lapply(blog,nchar)), "is the longest with ", lapply(blog[M],nchar), "characters"))
print("In news file")
print(paste("The line number",  M<-which.max(lapply(news,nchar)), "is the longest with ", lapply(news[M],nchar), "characters"))
print("In Twitter file")
print(paste("The line number",  M<-which.max(lapply(twitter,nchar)), "is the longest with ", lapply(twitter[M],nchar), "characters"))


```