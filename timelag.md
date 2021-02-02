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
random.time <- runif(n = my.samples, min = 9 * 60 * 60, 
                     max = 17 * 60 * 60)
```   

Rather than use the **as.Date()** function we can use the **as.POSIXct()** function which allows us to add seconds. From this we set the bounds of the dates we want to work with:

```r
df <- df %>%
  group_by(village) %>%
  arrange(village, my.timestamp) %>%
  mutate(diff = difftime(my.timestamp, 
                         lag(my.timestamp), units="days"))
                         
df
```      
Which results in the following output. Note that the leading value for each site is always NA (because there is nothing to subtract its time from):

```
# A tibble: 10 x 3
# Groups:   village [2]
   village my.timestamp        diff          
   <chr>   <dttm>              <drtn>        
 1 Lamaris 2021-01-03 11:51:33        NA days
 2 Lamaris 2021-01-05 13:43:53  2.078015 days
 3 Lamaris 2021-01-10 15:55:25  5.091331 days
 4 Lamaris 2021-01-13 14:26:39  2.938357 days
 5 Lamaris 2021-01-24 11:54:58 10.894664 days
 6 Nugi    2021-01-04 13:13:28        NA days
 7 Nugi    2021-01-06 10:05:47  1.869662 days
 8 Nugi    2021-01-16 13:22:32 10.136627 days
 9 Nugi    2021-01-19 09:41:38  2.846603 days
10 Nugi    2021-01-22 14:25:23  3.197051 days
```
 A consequence of this when we want to get a summary we need to use **drop_na()**:
  
```r
df %>%
  group_by(village) %>%
  drop_na(diff) %>%
  summarise(median = median(diff), 
            min = min(diff), 
            max = max(diff))
```

Hey presto ... we get a nice summary!

```
# A tibble: 2 x 4
  village median        min           max          
  <chr>   <drtn>        <drtn>        <drtn>       
1 Lamaris 4.014844 days 2.078015 days 10.89466 days
2 Nugi    3.021827 days 1.869662 days 10.13663 days
```
