# draftideas
Sandbox and exploration


#draft ideas sandbox

#setworking directory first!!!
setwd("C:/Users/Soon Guan/Desktop/Text Analysis Tutorials")

#readLines from the source force
readLines("")

#paste to combine the entire the vector into 1 element and separate them with a space
text <- paste(readLines(""), collapse=" ")

#CLEANING process

#Removing punctuations and numbers, lower casing all the words to make it not caps sensitive
text.1 <- gsub(pattern="\\W", replace= " ", text)  
text.1 <- gsub(pattern="\\d", replace= " ", text.1)
text.1 <- tolower(text.1)

#from package "tm" remove stop words that doesn't provide much meaning from stopwords()
library(tm)
text.2 <- removeWords(text.1, stopwords())

#remove the single letters following 's removal and previous cleaning filters
text.2 <- gsub(pattern="\\b[A-z]\\b{1}", replace=" ", text.2)

#(OPTIONAL) remove specific words from the text that may confound the outcome (remove #in next line)
#remember to lowercase it as you have lowercase all your elements, c() if necessary
##text.2 <- removeWords(text.2, "wordyouwanttoremove")

#clean up extra white spaces from previous steps
text.2 <-stripWhitespace(text.2)


## Sentiment analysis segment ## 

#load librariesrequired for sentiment analysis
#install.packages("tm",dependencies=TRUE) or install.packages("slam") <- use this if it fails

library(stringr)
library(wordcloud)

#opposite of c() function, split strings into elements
text.bag <- str_split(text.2, pattern="\\s+")
#change list to character 
text.bag <- unlist(text.bag)

#check your working directory and scan in lexicon
getwd()
positive.words <- scan("positive.words.txt", what='character', comment.char=";")
negative.words <- scan("negative.words.txt", what='character', comment.char=";")

#check score for each sentiment | negative and positive
match.pos <- match(text.bag, positive.words)
match.neg <- match(text.bag, negative.words)
neutral <- sum(!is.na(match(match.pos, match.neg)))

positive.score <- sum(!is.na(match(text.bag, positive.words)))
negative.score <- sum(!is.na(match(text.bag, negative.words)))

#compute the final score
score <- positive.score - negative.score
positive.score
negative.score
neutral
score

#wordcloud
wordcloud(text.bag, min.freq = 3, random.order=FALSE, scale=c(5,0.5), colors=rainbow(5))
