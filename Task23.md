---
title: "Data Science Capstone : NLP ~ Natural Language Processing ~"
author: "Nabil B."
date: "October 2020"
output: html_document
---

1. Understanding the problem
1. Data acquisition and cleaning
1. **Exploratory analysis**
1. **Statistical modeling**
1. Predictive modeling
1. Creative exploration
1. Creating a data product
1. Creating a short slide deck pitching your product

### loading data
Data are in [this link](https://d396qusza40orc.cloudfront.net/dsscapstone/dataset/Coursera-SwiftKey.zip)

Tasks to do [TASK Two](https://www.coursera.org/learn/data-science-project/supplement/BePVz/task-2-exploratory-data-analysis) and [TASK Three](https://www.coursera.org/learn/data-science-project/supplement/2IiM9/task-3-modeling)


```r
linesTOread<-50000
blog <- readLines("en_US/en_US.blogs.txt", linesTOread, skipNul = TRUE, warn = TRUE)
```

```
## Warning in file(con, "r"): impossible d'ouvrir le fichier 'en_US/
## en_US.blogs.txt' : No such file or directory
```

```
## Error in file(con, "r"): impossible d'ouvrir la connexion
```

```r
#news <- readLines("en_US/en_US.news.txt", linesTOread, skipNul = TRUE, warn = TRUE)
#twitter <- readLines("en_US/en_US.twitter.txt", linesTOread, skipNul = TRUE, warn = TRUE)
#print(twitter)
```

#### Sampling

```r
set.seed(2020)
SampleBlogs  <- sample(blog, 0.01 * length(blog) ) # take a sample
```

```
## Error in sample(blog, 0.01 * length(blog)): objet 'blog' introuvable
```

```r
rm(blog)
```

```
## Warning in rm(blog): objet 'blog' introuvable
```

Word Counting and searching for the most frequent word 
We will use the **_ngram_ library**


```r
if (!require("ngram")) { install.packages("ngram") }
```

```
## Loading required package: ngram
```

```
## Warning in library(package, lib.loc = lib.loc, character.only = TRUE,
## logical.return = TRUE, : there is no package called 'ngram'
```

```
## Installing package into 'C:/Users/NABIL/Documents/R/win-library/3.6'
## (as 'lib' is unspecified)
```

```
## --- Please select a CRAN mirror for use in this session ---
## package 'ngram' successfully unpacked and MD5 sums checked
## 
## The downloaded binary packages are in
## 	C:\Users\NABIL\AppData\Local\Temp\Rtmp2jC2KO\downloaded_packages
```

```r
text<-preprocess(concatenate(SampleBlogs), case="lower", remove.punct = TRUE, fix.spacing = TRUE)
```

```
## Error in preprocess(concatenate(SampleBlogs), case = "lower", remove.punct = TRUE, : impossible de trouver la fonction "preprocess"
```

```r
head ( get.phrasetable(ng<- ngram (text , n =1)), 15)
```

```
## Error in get.phrasetable(ng <- ngram(text, n = 1)): impossible de trouver la fonction "get.phrasetable"
```

```r
print(head (DT<- get.phrasetable(ng2<- ngram (text , n =2)), 15))
```

```
## Error in get.phrasetable(ng2 <- ngram(text, n = 2)): impossible de trouver la fonction "get.phrasetable"
```

```r
head ( get.phrasetable(ng3<- ngram (text , n =3)), 15)
```

```
## Error in get.phrasetable(ng3 <- ngram(text, n = 3)): impossible de trouver la fonction "get.phrasetable"
```

```r
head ( DT4<-get.phrasetable(ng4<- ngram (text , n =4)), 15)
```

```
## Error in get.phrasetable(ng4 <- ngram(text, n = 4)): impossible de trouver la fonction "get.phrasetable"
```

### ploting
bigram


```r
if (!require("ggplot2")) { install.packages("ggplot2") }
```

```
## Loading required package: ggplot2
```

```
## Warning: package 'ggplot2' was built under R version 3.6.3
```

```r
ggplot(DT[1:15,], aes(x=ngrams, y=freq)) + 
    geom_bar(stat="Identity", fill="lightgreen") + 
    theme(axis.text.x = element_text(angle = 90, hjust = 1)) + xlab("2-grams") + 
    ylab("Frequency")
```

```
## Error in ggplot(DT[1:15, ], aes(x = ngrams, y = freq)): objet 'DT' introuvable
```

other VIZ

```r
ggplot(DT[1:15,], aes(x=ngrams, y=freq)) +
  geom_col(fill="lightblue") +
  coord_flip() +
  labs(x= "Word\n", y="\n Count", title="Most frequent words (bigram) in the text\n")+
  geom_text(aes(label=freq),hjust=1.2, color="darkblue", fontface="bold") +
  theme(plot.title = element_text(hjust=0.5), axis.title.x = element_text(face="bold",color="darkblue", size=12), 
        axis.title.y = element_text(face="bold",color="darkblue", size=12))
```

```
## Error in ggplot(DT[1:15, ], aes(x = ngrams, y = freq)): objet 'DT' introuvable
```


4-gram

```r
if (!require("ggplot2")) { install.packages("ggplot2") }
ggplot(DT4[1:10,], aes(x=ngrams, y=freq)) + 
    geom_bar(stat="Identity", fill="gold") + 
    theme(axis.text.x = element_text(angle = 90, hjust = 1)) + xlab("4-grams") + 
    ylab("Frequency")
```

```
## Error in ggplot(DT4[1:10, ], aes(x = ngrams, y = freq)): objet 'DT4' introuvable
```

### Some other feature : Frequency sorted

How many unique words do you need in a frequency sorted dictionary to cover 50% of all word instances in the language? 90%?

```r
for (percent in c(0.5,0.9)) {
  i<-1
  numberWords<-get.phrasetable(ng)$freq[i]
  while (sum(get.phrasetable(ng)$freq)*percent>numberWords) {
    i<-i+1
    numberWords<-numberWords+get.phrasetable(ng)$freq[i]
    }
  print(paste("we have ",toString(i) , "words to cover over", toString(percent*100), "% of our Datas"))
}
```

```
## Error in get.phrasetable(ng): impossible de trouver la fonction "get.phrasetable"
```
