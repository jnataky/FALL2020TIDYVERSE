My analysis includs analysing the sales of video games all around the world, I used more than one Tidyverse packges, and collected the data from Kaggle. 

Any additional analysis is welcome.
=======

# TidyVerse Recipe CREATE

Name: Arushi Arora

#### Introduction:
The core tidyverse package includes "readr", "tidyr" and "dplyr"
- `readr` provides a fast and friendly way to read rectangular data
- `tidyr` provides a set of functions that help you get to tidy data. Tidy data is data with a consistent form: in brief, every variable goes in a column, and every column is a variable
- `dplyr` provides a grammar of data manipulation, providing a consistent set of verbs that solve the most common data manipulation

#### Approach:
Read the csv for the article https://fivethirtyeight.com/features/how-americans-like-their-steak/ published on Five Thirty Eight from Github using `readr`. Rename all the variables using `dplyr` package and remove the first row with no real reponses. Mutate the variable type to factor using the function `mutate` for further analysis
 
#### Conclusion:
The dataset was imported and cleaned using packages in TidyVerse

---

#### SYNTAX
- Import CSV from Github
    ```
    urlfile="https://raw.githubusercontent.com/fivethirtyeight/data/master/steak-survey/steak-risk-survey.csv"

    steakdata<- readr::read_csv(url(urlfile))
    ```

- Rename variables

    ```
    steakdata1 = dplyr::rename(steakdata, 
    "lottery" = "Consider the following hypothetical situations: <br>In Lottery A, you have a 50% chance of success, with a payout of $100. <br>In Lottery B, you have a 90% chance of success, with a payout of $20. <br><br>Assuming you have $10 to bet, would you play Lottery A or Lottery B?", 
    "smoke_cigs" = "Do you ever smoke cigarettes?" ,
    "drink_alcohol" = "Do you ever drink alcohol?", 
    "gamble" = "Do you ever gamble?",
    "skydiving" = "Have you ever been skydiving?",
    "overspeeding" = "Do you ever drive above the speed limit?",
    "cheat_patner" = "Have you ever cheated on your significant other?",
    "eat_steak" = "Do you eat steak?",
    "steak_prep" = "How do you like your steak prepared?",
    "hh_income" = "Household Income",
    "location" = "Location (Census Region)")
    ```
- Remove first row

  ```
    steakdata2 <- steakdata1[-c(1), ]
  ```

- Code for populating the tables from .csv file stored locally
    ```
    steakdata3 <- steakdata2 %>% as_tibble() 

    steakdata4 <- steakdata3 %>%
    mutate(lottery = as.factor(lottery)) %>%
    mutate(smoke_cigs = as.factor(smoke_cigs)) %>%
    mutate(drink_alcohol = as.factor(drink_alcohol)) %>%
    mutate(gamble = as.factor(gamble)) %>%
    mutate(skydiving = as.factor(skydiving)) %>%
    mutate(overspeeding = as.factor(overspeeding)) %>%
    mutate(cheat_patner = as.factor(cheat_patner)) %>%
    mutate(eat_steak = as.factor(eat_steak)) %>%
    mutate(steak_prep = as.factor(steak_prep)) %>%
    mutate(Gender = as.factor(Gender)) %>%
    mutate(Age = as.factor(Age)) %>%
    mutate(hh_income = as.factor(hh_income)) %>%
    mutate(Education = as.factor(Education)) %>%
    mutate(location = as.factor(location))

    ```
=======
# FALL2020TIDYVERSE

Ian Costello Tidyverse Create

# Tidyverse Create

I decided to pick a data set regarding the senate race fundamentals. Using dplyr and stringr, I created a new column "state_ID for just the two character initials of states. I also filtered based on what I believe are the most competitive states this cycle. 
=======
CUNY DATA 607 TIDYVERSE Collaborative project

---
title: "Getting to know dplyr & stringr"
author: "Jered Ataky"
date: "`r Sys.Date()`"
output: 
    html_vignette:
      toc: true
vignette: >
  %\VignetteIndexEntry{Getting to know dplyr & stringr}
  %\VignetteEngine{knitr::rmarkdown}
  %\VignetteEncoding{UTF-8}
---

```{r setup, include = FALSE}
knitr::opts_chunk$set(
  collapse = TRUE,
  comment = "#>"
)
```


# 1. Introduction

"dplyr" is one of the tidyverse packages, and it is used for data manipulation.
In other words, it is a grammar of data manipulation providing verbs that help
to solve many problems faced in data manipulation.

"stringir" in the other is also one of a tidyverse package, and it is focused 
on string manipulation. stringir is a grammar of string manipulation. 
It provides set of functions designed to make 
working with strings easier.

Note that tidyverse is just a collection of R packages underlying same 
design philosophy, grammar, and data structure.
For more info about dplyr and stringir, read [here](http://r4ds.had.co.nz/).

Here, we are going to explore five major verbs for dplyr: 
filter(), select(), arrange(), mutate(), summarize()

and seven functions for stringr:
str_subset(), str_detect(), str_count(), str_locate(), str_extract(),
str_split(), str_replace(), str_match() 



# 2. dplyr


Throughout this part of the vignette, we make use of "student performance", a dataset containing a sample of 1000 observations of 8 variables from [kaggle](https://www.kaggle.com/spscientist/students-performance-in-exams) datasets.

```{r message=FALSE}
# library
library(dplyr)

# load in the dataset

data <- read.csv("https://raw.githubusercontent.com/jnataky/DATA-607/master/A2_Various_dataset_transformation/students_performance.csv")

# take a look at its structure
glimpse(data)

```

## filter(): picking cases by their values

The filter function picks cases by their values. Let say you want to pick students 
which math score is  100, you might write:

```{r}

data %>%
  filter(math.score == 100)

```


Similarly, if you want to pick students 
which math, writing, and reading score are all 100, you might write:

```{r}

data %>%
  filter(math.score == 100, writing.score == 100, reading.score == 100)

```


## select(): selecting variables based on their names

This second verb lets you select variables based on their names. 
Let say you want to select only the variables gender, math.score, and 
writing.score, you might write:

```{r}

data2 <- data %>%
  select(gender, writing.score, reading.score)

# Print five first students
head(data2, 6)
```


## arrange(): Reordering the cases

Arrange function lets reordering the cases in the order that you want.
Let say that you want to reorder the head of previous selected variables data frame (data2)
in descending order of students math score, you might write:

```{r}

# name the head of data2 as data3 

data3 <- head(data2, 6)

# Reorder in descending order of math score

data3 %>%
  arrange(desc(writing.score))

```


## mutate(): creating new variables that are functions of existing variables

The mutate function creates new variables that are functions of the existing variables. 
Let say you want to create "english.score" which is the average 
of writing.score and reading.score, you might write:

```{r}

data3 %>%
  mutate(english.score = (writing.score + reading.score) / 2)

```


If you are interesting in keeping only the new variable from the existing variables,
let say you want to keep only english.score and not the two others, 
you might use another function called transmuse():

```{r}

data4 <- data3 %>%
  transmute(gender, english.score = (writing.score + reading.score) / 2)

data4

```

## summarize(): summarizing multiple values to a single value

Here we will introduce the function group_by which helps grouping by the variable 
you want to do your summary. Let say you are interested in the summary of the average score
of different gender in math, you might write:

```{r}

data %>%
  group_by(gender) %>%
  summarize( math_score = sum (math.score)/ n())

```



# 3. stringr


The strings functions take for arguments one vector of strings and a second argument
being the pattern. For this entire part of the vignette, 
we will use one vector of strings v and same pattern p which will be the regular
expression matching any single character that is a vowel.

```{r}

# library

library(stringr)

# Define the vector of strings

v <- c("sonority", "meal", "try", "cocktail", "cinema", "maximum", "mass")

# Pattern matching any vowel

p <- "[aeiou]"

```


## str_subset(): extracting the matching components

subset function will let you extract strings that contain vowels.

```{r}

str_subset(v, p)

```

## str_detect(): telling if there is any pattern matching


detect function detects if there is any match pattern.
If you want to detect if there is any vowel in any components of v, you might write:

```{r}

str_detect(v, p)

```

## str_count(): counting the patterns

if you want to count the number of vowels in each components of the vector of strings,
you might write:

```{r}

str_count(v, p)

```


## str_locate(): locating the position of the match

locate function helps you locate where there is the match.
Let say that you want to know the position of vowels in each component of v,
you might write:

```{r}

str_locate(v, p)

```


## str_extract(): extracting the text of the match

extract function lets you extracting the first match pattern in the string.
The following will let you extracting the first vowel in each components of v:

```{r}

str_extract(v, p)

```


## str_split(): splitting up strings

Here we are going to split up strings separated by comma in different pieces

```{r}

s <- c("dada, mum", "uncle, auntie, cousin", "men, women")
str_split(s, ",")

```


## str_replace(): replacing the matches with new text

replace function let you replace the first match pattern by the replacement 
argument that you specify. If you want to replace the first vowel in components 
of v by "/", you might write:

```{r}

str_replace(v, p, "/")

```

## str_match(): extracting parts of the match defined by parentheses

If you want to extract the letter before the first vowel in components of vector of strings v, 
you might write:

```{r}

str_match(v, "(.)[aeiou]")

```



For more about tidyverse packages:

[R for Data Science](http://r4ds.had.co.nz/)
=======
ggplot2_vignettes: By Jordan Tapke 
=======
Zhouxin Shi CREATE - function(read_csv,filter,select), data(Public Use Microdata Sample)
=======

### group_split() vignette by Jack Wright

-added an instructional vignette on group split
=======
### Vignettes

In the vignettes folder we will hold the different vignette files. I added one called `readr-package-gc` which serves as an introduction to *readr*


### Data folder

The data folder holds several supporting files for the different examples. 
=======

Josef Waples - power plant data added as 19th pull request but am I first change to readme.md file? 
=======

# Description
arrange() sorts the rows according to the values of the specified column, with the lowest values appearing near the top of the data frame.Place desc() around a column name to cause arrange() to sort by descending values of that column.


```{r}
library(tidyverse)

senFunURL <- "https://projects.fivethirtyeight.com/2020-general-data/senate_fundamentals.csv"
senFun <- read.csv(file = senFunURL, header = TRUE, sep = ",")

senFun <- senFun %>%
  dplyr::mutate(state_ID = str_extract(district, "^[:alpha:]{2}")) %>%
  filter(state_ID == "ME" | state_ID == "MI" | state_ID == "AL" | state_ID == "CO" | state_ID == "IA" | state_ID == "GA" | state_ID == "AZ" | state_ID == "NC" | state_ID == "SC")
```
=======
#read csv file 
file<- read.csv("https://raw.githubusercontent.com/hrensimin05/Data_607/master/2019.csv")
#view(file)

list<-as.data.frame(file)%>% 
  arrange(desc(Healthy.life.expectancy))

head(list)
```
=======
---
title: "TidyVerse Recipe"
author: "Dariusz Siergiejuk"
date: "10**22**2020"
output: html_document
---

## TidyVerse CREATE assignment

In this assignment, you’ll practice collaborating around a code project with GitHub.  You could consider our collective work as building out a book of examples on how to use TidyVerse functions.

Your task here is to Create an Example.  Using one or more TidyVerse packages, and any data set from fivethirtyeight.com or Kaggle, create a programming sample “vignette” that demonstrates how to use one or more of the capabilities of the selected TidyVerse package with your selected data set. (25 points)

### Loading TidyVerse.

```{r, echo = FALSE}
library(tidyverse)
```

### Using readr to read data from a csv file.

```{r, echo = FALSE}
drivers <- read_csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/bad-drivers/bad-drivers.csv")
```

```{r, echo = FALSE}
head(drivers)
```

### Utlizing ggplot2 to visualizae data; with pipe operation %>% from dplyr

```{r, echo = FALSE}
drivers %>% ggplot(aes(x=reorder(State, -`Car Insurance Premiums ($)`), y=`Car Insurance Premiums ($)`, fill=State)) + 
  geom_bar(stat = "identity") + 
  guides(fill = FALSE) +
  theme(axis.text.x = element_text(angle = 60, hjust = 1))
```

End of File and Text.
=======

Tidyverse CREATE Assignment - Atina Karim

In this assignment I looked at Water Quality Data from the City of Austin's online data portal:[https://data.austintexas.gov/Environment/Water-Quality-Sampling-Data/5tye-7ray](https://data.austintexas.gov/Environment/Water-Quality-Sampling-Data/5tye-7ray). The dataset contains the results of about a 1000 water quality tests performed on water bodies in Austin, in 2020.

I used tidyverse packages such as tidyr and dplyr to clean the dataset, and then visualized the data using ggplot2.

=======
John Mazon TidyVerse Create Assignment - 
Analysis of Diamond clarity and depth correlation/frequency as well as ratio [pertaining to price to depth] using multiple TidyVerse functionalities 
=======

* Data Analysis of Grocery items sold using Groceries_dataset.csv
=======
Change Log:
26 October: Added vignette w/ examples for purrr and forcats, Cameron Smith


=======

Jered extending Zhouxin Shi's recipe: 

As Zhouxin vignette is about dplyr, I extended his work by adding 
different other functions of the same package.
Since the tidyverse CREATE explored the functions select() and filter(),  
I added three other main verbs of dplyr in tidyverse EXTEND: arrange(), mutate(), and summarize().
See the update tidyverse.RMD file for the details of my work.

=======


