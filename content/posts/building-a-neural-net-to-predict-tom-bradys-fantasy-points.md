---
title: "Building a Neural Net to Predict Tom Brady's Fantasy Points"
date: 2017-10-19T08:04:14-05:00
tags : ["R","Rlang","Machine Learning","NFL", "DraftKings", "Daily Fantasy", "Predictive Math"]
draft: false
---

<link rel="stylesheet" href="https://openmonstervision.github.io/prism.css" />
<script src="https://openmonstervision.github.io/prism.js" type="text/javascript"></script>
# The Who

I don't know how to tie my shoes, this will come into play later. 

# The Why

So we can all make lots of money gambling. Wait, is it even gambling then? Oh no, it isn't remember DraftKings is legal because it's a game of skill. ![I dont gamble, I go Winning - DJ Khaled](https://i.imgur.com/TM36czP.jpg "DJ Khaled")

# The Code 

Let's Build this thing. I used R, you can use whatever plebian language you prefer. ![NeuralNet Visual Representation] (https://i.imgur.com/Duj63gI.png "Neural Net Visual")

``` r
#Import Tom Brady Data
tombrady <- read.csv('~/work/TomBrady.csv')
str(tombrady)
#Random Seed
set.seed(Sys.time())
#Some Time after Seven Thirty, but before 7 fifty
train_sample <- sample(52, 40)
str(train_sample)

#Seperate Training and Test Data
tom_train <- tombrady[train_sample, ]
tom_test <- tombrady[-train_sample, ]

prop.table(table(tom_train$Act))
prop.table(table(tom_test$Act))

#load neuralnet Library
library(neuralnet)
sansact <- tom_train
sansact$Act <- NULL

# I've choosen player rating and Projected Points as the formula, since those will be available before the game
tom_model <- neuralnet(Act ~  Proj  + rating, data = tom_train, hidden = 5)
plot(tom_model)
model_results <- compute(tom_model,tom_test[,c("Proj","rating")])
predicted_tom <- model_results$net.result
cor(predicted_tom, tom_test$Act)
```

# Proof is in the Pudding, what about the performance

![Proof] (https://i.imgur.com/bHafCql.png "Proof")

Holy shit! .92 correlation....ZOMG....Wait, let me check something. Oh wow, just luck with my random seed. Half the time the code doesn't even work. This is your fault though, you thought someone who couldn't tie their shoes, and made DJ Kahled jokes was going to give you the secret of wealth. Well why doesn't it work? Probably because football players don't generate enough data, in a career to do a real analysis? I don't know, remember I can't tie my shoes. Actual numeric results, Predicted Points VS Actual Points:

![Numbers] (https://i.imgur.com/JSFQoJD.png "Numbers")

You can see these numbers are relatively close, but again, results this good were more of a fluke, of randomness, and they aren't worth betting on.

# What you should do from here?

Probably buy a book from [someone](https://www.amazon.com/gp/product/0143125087/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&tag=bsdpblog-20&camp=1789&creative=9325&linkCode=as2&creativeASIN=0143125087&linkId=830177ea55aad172e6957277bb7c6c32) who actually knows what they're talking about.
