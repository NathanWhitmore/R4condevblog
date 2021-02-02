### Calculating time lags
The other day I was asked to calculate the time difference (median, minimum and maximum) between monitoring sessions within study sites for a large data set. 
Although this might seem daunting, it is a problem that is very easy to handle in the `tidyverse` package.

### Making the mock data
We will begin by using the packages `tidyverse` (for data wrangling)

```r
library("tidyverse")

# make a seed (so the results can be replicated)
set.seed(13)

# set the number of samples we want (e.g. 10)
my.samples <- 10
```
Rather than use the **as.Date()** function we can use the **as.POSIXct()** function which allows us to add seconds. From this we set the bounds of the dates we want to work with:

```r
# make a start and end date using as.POSIXct()
Start <- as.POSIXct("2021-01-01")
End <- as.POSIXct("2021-01-31")
```

We then turn these into a sequence:

```r
# make random number of seconds:
random.time <- runif(n = my.samples, min = 9 * 60 * 60, max = 17 * 60 * 60)
random.time <- runif(n = my.samples, min = 9 * 60 * 60, 
                     max = 17 * 60 * 60)
```   

Rather than use the **as.Date()** function we can use the **as.POSIXct()** function which allows us to add seconds. From this we set the bounds of the dates we want to work with:

```r
df <- df %>%
  group_by(village) %>%
  arrange(village, my.timestamp) %>%
  mutate(diff = difftime(my.timestamp, lag(my.timestamp), units="days"))
  mutate(diff = difftime(my.timestamp, 
                         lag(my.timestamp), units="days"))
```      
Which results in the following output. Note that the leading value for each site is always NA (because there is nothing to subtract its time from):
  
 A consequence of this when we want to get a summary we need to use **drop_na()**
  
```r
  df %>%
  group_by(village) %>%
  drop_na(diff) %>%
  summarise(median = median(diff), min = min(diff), max = max(diff))
summarise(median = median(diff), 
          min = min(diff), 
          max = max(diff))
```

Hey presto ... we get a nice summary!
