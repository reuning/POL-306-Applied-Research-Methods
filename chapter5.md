---
title: T-Tests
description: ""
---

## T-Tests

```yaml
type: NormalExercise
key: ea755615df
xp: 100
```

t-tests in R use the function `t.test()`. They use formulas to identify what the interval variable is and what the group variable is. You will be using formulas a lot later on when doing regressions. The important part of formulas is ~ This is called a tilde and is on the top left of your keyboard (near the number 1). 

A formula for `t.test()` will look something like: interval_variable~group_variable. 

`t.test()` expects the first argument to be a formula and you can include the dataframe as the second argument. If you include the dataframe as the second argument you don't have to worry about doing df$ for each variable. For example if you have a dataset, `df` with two variables `X` (your grouping variable) and `Y` (your interval variable) you can call `t.test()` like this: 

```
t.test(Y~X, df) 

```

You could also do it like this:

```
t.test(df$Y~df$X)

```

Both will give you the same results.   


`t.test()` can take a third argument called alternative that sets the alternative hypothesis. This can be "two.sided" (the default), "greater", or "less". This one you'll like want to call by name:

```
t.test(Y~X, df, alternative="two.sided")

```

There are also several other arguments for `t.test()` and if you want you can check them by calling `?t.test`

`@instructions`
We are going to keep using the protest dataset. You will do a t.test on if the people that protested at the DNC are younger than protesters at the RNC. I want you to call `t.test()` three times once with each different alternative choice. Look at how the p-value and the confidence intervals change.

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

t.test(age~convention, data=df, alternative="greater")
t.test(age~convention, data=df, alternative="two.sided")
t.test(age~convention, data=df, alternative="less")
```

`@sct`
```{r}
ex() %>% {
  check_function(., "t.test" ) %>% check_arg("alternative")
}
```

---

## Dropping Values

```yaml
type: NormalExercise
key: feaeb1297c
xp: 100
```

t-tests can only compare two groups. If you have more than two groups you have to decide what to do. You can do either ignore other groups or include observations that are in other groups into one of the two groups of interest. 

Changing the values of a variable in your dataset can be dangerous so it is always good practice to start by saving it as a new variable. Remember we did this before by calling `df$new_var <- df$old_var`. 

Once you create the new variable you can start changing values. The easiest way to do this is to use the dollar sign & bracket mentioned before: `df$new_var[ ]`. Previously we just put a number in there but we can also put a logical statement and then assign something new to the data we select. This looks like:

```
df$new_var[logical statement] <- new_value 
```

As a more concrete example lets reassign everyone who said they went on more than 4 marches:

```
df$marches[df$marches > 4] <- 4
```

Finally if you assign NA then you are making those values that are matched into missing values which makes them easier to drop in commands.

```
df$marches[df$marches > 4] <- NA
```

`@instructions`
You will test to see if there is a difference between how many marches Democrats and non-Democrats participated in at the protests. Currently party has three attributes: "Democrat", "Independent" and "Other". 

1. Start by creating a new variable in the dataset named `party_new` based on the current variable `party`. 
2. After you create the variable `party_new` change all the instances where it is `"Independent"` to `"Other"`
3. Finally do a t-test on the the difference in marches (not the march_recode) between Democrats and non-Democrats.

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

df$march_recode <- df$marches
table(df$march_recode)

df$march_recode[df$march_recode > 4] <- 4
table(df$march_recode)


```

`@solution`
```{r}
df <- read.csv("Protest_Survey.csv")
names(df) 

df$march_recode <- df$marches
table(df$march_recode)

df$march_recode[df$march_recode > 4] <- 4
table(df$march_recode)

df$party_new <- df$party
df$party_new[df$party_new=="Independent"] <- "Other"
t.test(marches~party_new, data=df)
```

`@sct`
```{r}
ex() %>% {
  check_function(., "t.test" )
  check_object(., "df") %>% check_column("party_new") %>% check_equal()
  check_output_expr(., "t.test(marches~party_new, data=df)")
}
```
