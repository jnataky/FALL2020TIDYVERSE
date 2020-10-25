# FALL2020TIDYVERSE
CUNY DATA 607 TIDYVERSE Collaborative project


---
title: "Getting to know dplyr & stringr"
author: "Jered Ataky"
date: "`r Sys.Date()`"
output: rmarkdown::html_vignette
vignette: >
  %\VignetteIndexEntry{Getting to know dplyr}
  %\VignetteEngine{knitr::rmarkdown}
  %\VignetteEncoding{UTF-8}
---

```{r setup, include = FALSE}
knitr::opts_chunk$set(
  collapse = TRUE,
  comment = "#>"
)
```

**1. Introduction**

**2. dplyr**

   * filter(): picking cases by their values
   * select(): selecting variables based on their names
   * arrange(): Reordering the cases
   * mutate(): creating new variables that are functions of existing variables
   * summarize(): summarizing multiple values to a single value
   
**3. stringr**

   * str_subset(): extracting the matching components
   * str_detect(): telling if there is any pattern matching
   * str_count(): counting the patterns
   * str_locate(): locating the position of the match
   * str_extract(): extracting the text of the match
   * str_split(): splitting up strings
   * str_replace(): replacing the matches with new text
   * str_match(): extracting parts of the match defined by parentheses


## 1. Introduction

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


## 2. dplyr


Throughout this part of the vignette, we make use of "student performance", 
a dataset supplied by infer containing a sample of 1000 observations of 8 variables from kaggle datasets.

```{r message=FALSE}
# library
library(dplyr)

# load in the dataset

data <- read.csv("https://raw.githubusercontent.com/jnataky/DATA-607/master/A2_Various_dataset_transformation/students_performance.csv")

# take a look at its structure
glimpse(data)

```

### filter(): picking cases by their values

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


### select(): selecting variables based on their names

This second verb lets you select variables based on their names. 
Let say you want to select only the variables gender, math.score, and 
writing.score, you might write:

```{r}

data2 <- data %>%
  select(gender, writing.score, reading.score)

# Print five first students
head(data2, 6)
```


### arrange(): Reordering the cases

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


### mutate(): creating new variables that are functions of existing variables

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

### summarize(): summarizing multiple values to a single value

Here we will introduce the function group_by which helps grouping by the variable 
you want to do your summary. Let say you are interested in the summary of the average score
of different gender in math, you might write:

```{r}

data %>%
  group_by(gender) %>%
  summarize( math_score = sum (math.score)/ n())

```



## 3. stringir


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


### str_subset(): extracting the matching components

subset function will let you extract strings that contain vowels.

```{r}

str_subset(v, p)

```

### str_detect(): telling if there is any pattern matching


detect function detects if there is any match pattern.
If you want to detect if there is any vowel in any components of v, you might write:

```{r}

str_detect(v, p)

```

### str_count(): counting the patterns

if you want to count the number of vowels in each components of the vector of strings,
you might write:

```{r}

str_count(v, p)

```


### str_locate(): locating the position of the match

locate function helps you locate where there is the match.
Let say that you want to know the position of vowels in each component of v,
you might write:

```{r}

str_locate(v, p)

```


### str_extract(): extracting the text of the match

extract function lets you extracting the first match pattern in the string.
The following will let you extracting the first vowel in each components of v:

```{r}

str_extract(v, p)

```


### str_split(): splitting up strings

Here we are going to split up strings separated by comma in different pieces

```{r}

s <- c("dada, mum", "uncle, auntie, cousin", "men, women")
str_split(s, ",")

```


### str_replace(): replacing the matches with new text

replace function let you replace the first match pattern by the replacement 
argument that you specify. If you want to replace the first vowel in components 
of v by "/", you might write:

```{r}

str_replace(v, p, "/")

```

### str_match(): extracting parts of the match defined by parentheses

If you want to extract the letter before the first vowel in components of vector of strings v, 
you might write:

```{r}

str_match(v, "(.)[aeiou]")

```



For more about tidyverse packages:

[R for Data Science](http://r4ds.had.co.nz/)
