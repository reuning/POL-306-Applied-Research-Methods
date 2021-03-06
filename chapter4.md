---
title: 'Chi-Squared and Data Manipulation '
description: ""
---

## Chi-Squared

```yaml
type: NormalExercise
key: 45ef1d9d90
xp: 100
```

Today we are going to start by looking at how to do a chi-squared test and then we will look at some general data manipulation in R. 

The function to calculate the chi-squared test is `chisq.test()`. This expects the two variables as its input. The order of the variables does not matter (remember that from class the way the table is designed does not matter).

`@instructions`
You are going to analyze the relationship between party identification and what convention a protester protested at (either at the RNC or the DNC). This is the same data that is used as an example on this week's assignment. 

You should do two things in this assignment:

1. Create a cross-tab. Remember this can be done with `prop.table()` and `table()`. You'll want to set it up where party is the dependent variable and the location as the independent variable. You'll also want to use column percentages (margin=2). 
2. Calculate the chi-squared statistic. Remember `chisq.test()` expects just the two different vectors (you'll separate them with a comma).

Before you click "Submit Answer" you should look at what the output is from chisq.test().

`@hint`


`@pre_exercise_code`
```{r}
tmp.df <- read.csv("http://assets.datacamp.com/production/repositories/3406/datasets/41ae7a219de8ed396ebf3d49e6561a03fe27541a/protest_survey.csv")

write.csv(tmp.df, "Protest_Survey.csv", row.names=F)
rm(list=ls())



```

`@sample_code`
```{r}
df <- read.csv("Protest_Survey.csv")
names(df) 
```

`@solution`
```{r}
df <- read.csv("Protest_Survey.csv")
names(df) 

prop.table(table(df$party, df$convention), margin=2)

chisq.test(df$party, df$convention)

```

`@sct`
```{r}
ex() %>% {
  check_function(., "table")
  check_function(., "prop.table") %>% check_arg("margin") %>% check_equal()
  check_output_expr(., "prop.table(table(df$party, df$convention), margin=2)" )
  check_function(., "chisq.test")
  check_output_expr(., "chisq.test(df$party, df$convention)")
}
```

---

## Accessing Items in Data

```yaml
type: NormalExercise
key: c473e6102f
xp: 100
```

Now we will go over some data manipulations. 

In all of these examples I assume that df is your dataframe. 

**Accessing Elements**
There are several ways to access particular information in a data frame. 

- Dollar Sign: As you already know you can access particular columns in a dataframe by using the dollar sign followed by the variable name: df$party. 
- Brackets: You can also access particular rows and columns in a dataframe using `[ , ]`. For example, `df[1, 2]` will return the contents of the dataframe in first row and the second column. You can also access just a column or just a row. `df[, 4]` will return the fourth column and `df[3, ]` will return the third row 
- Brackets and Dollar Sign: Finally you can combine the two to access a particular row of a variable: `df$party[4]` will return the fourth object in the column/variable party. 

In each of these cases you can then save what is returned using <- 

For example: `party_id <- df$party`

**Updating Elements**
Along with accessing elements you can change them using `<-` to assign a new value. For example `df[1,3] <- "hello"` will replace whatever is in the first row and third column with `"hello"`. Brackets and dollar signs will both work if you want to edit what is already there, but if you want to add a new column then you have to use the dollar sign method. For example `df$ideology <- ideology` would add a new variable/column to df.

`@instructions`
1. Find out what the party is of the 32nd observation (row) in the dataframe. DO NOT SAVE IT, JUST PRINT IT OUT. 
2. There is a variable `marches` in the dataframe. Save it as a new variable: `march_number`. This new variable will be independent on the dataframe.

`@hint`


`@pre_exercise_code`
```{r}
tmp.df <- read.csv("http://assets.datacamp.com/production/repositories/3406/datasets/41ae7a219de8ed396ebf3d49e6561a03fe27541a/protest_survey.csv")

write.csv(tmp.df, "Protest_Survey.csv", row.names=F)
rm(list=ls())

```

`@sample_code`
```{r}
df <- read.csv("Protest_Survey.csv")
names(df) 
```

`@solution`
```{r}
df <- read.csv("Protest_Survey.csv")
names(df) 

df$party[32]

march_number <- df$marches
```

`@sct`
```{r}
ex() %>% {
  check_output_expr(., "df$party[32]" )
  check_object(., "march_number") %>% check_equal()
}
```

---

## Logical Statements

```yaml
type: NormalExercise
key: 6b032be1b9
xp: 100
```

We will also often want to select subsets of rows that meet some sort of criteria. For example we might want only the data for participants that are over 50 years old. In order to do this we first need to have an idea of logical statements. R allows you to compare two things using `==`, `!=`, `>`, `<`, `<=`, and `>=`

Comparisons:
- Equal `==`
- Not equal `!=` 
- Less than `<` 
- Greater than `>`
- Less than or equal to `<=`
- Greater than or equal to `>=`  

To test if two numbers are equal you would write: `1 == 1`. Or if you want to know if 10 is greater than 5: `10 > 5`. These will both return `TRUE`

We can also compare an entire vector to a value: `df$age < 45`. This will return a vector of three possible values `TRUE`, `FALSE`, or `NA`. If the value is missing it will return an `NA`. 

Finally if we want to use this to select a subset of data we use the function `subset()`. This expects a dataframe and then a logical statement that indicates which rows to pick. 

For example:
```
sub.df <- subset(df, age < 30) 
```

This will create a new dataframe called `sub.df` that includes only observations where `df$age` is less than 30 (note you do not need to repeat `df` when identifying the age variable as the subset function knows to look for age in the dataframe you provided).

`@instructions`
Use the subset function and a logical statement to create a new dataframe called sub.df that only contains individuals that identify as Democrats. Note the variable of interest is party and it takes on three values here `"Democrat"`, `"Other"`, and `"Independent"`

`@hint`


`@pre_exercise_code`
```{r}
tmp.df <- read.csv("http://assets.datacamp.com/production/repositories/3406/datasets/41ae7a219de8ed396ebf3d49e6561a03fe27541a/protest_survey.csv")

write.csv(tmp.df, "Protest_Survey.csv", row.names=F)
rm(list=ls())

```

`@sample_code`
```{r}
df <- read.csv("Protest_Survey.csv")
names(df) 

### First some examples
1 == 1
2 < 10

df$age < 30


```

`@solution`
```{r}
df <- read.csv("Protest_Survey.csv")
names(df) 

sub.df <- subset(df, party=="Democrat")
```

`@sct`
```{r}
ex() %>% {
  check_function(., "subset" ) %>% check_arg("x") %>% check_equal()
  check_object(., "sub.df") %>% check_equal()
}
```
