# Calculating time lags

The other day I was asked to calculate the time difference between monitoring sessions within study sites for a large data set. Although this might seem daunting, it is a problem that is very easy to handle in the tidyverse.
 
## Making the mock data

We will begin by using the packages `tidyverse` (for data wrangling) and `lubridate` (for handling dates).

    library(tidyverse)
    library(lubridate)

    # make a seed (so the results can be replicated)
    set.seed(13)

    # set the number of samples we want (e.g. 10)
    my.samples <- 10

Rather than use the **as.Date()** function we can use the **as.POSIXct()** function which allows us to add seconds. From this we set the bounds of the dates we want to work with:

    # make a start and end date using as.POSIXct()
    Start <- as.POSIXct("2021-01-01")
    End <- as.POSIXct("2021-01-31")

We then turn these into a sequence:

    # use these to make a sequence of days
    dates <- seq(from = Start, to = End, by = "day")

We can then randomly sample those dates:

    # randomly sample from those dates
    random.date <- sample(dates, my.samples)

Okay, now that is sorted we need to add some hour, minutes and seconds. We can do this using the **runif()** function to generate a random sample within an interval. 
If we imagine the monitoring took place between 0900 and 1700 hours the numbers of seconds would be:

    # make random number of seconds:
    random.time <- runif(n = my.samples, min = 9 * 60 * 60, max = 17 * 60 * 60)
    
    We then are able to make a timestamp:

    # add the rand.time to the  random.date to make a (random) timestamp
    my.timestamp <- random.date + random.time

We then make a variable containing study site information:

    # combine to form a simple data frame
    village <- c(rep("Lamaris", 5), rep("Nugi", 5))

And make a data frame from these:

    # combine to form a simple data frame
    df <- data.frame(village, my.timestamp)


    df <- df %>%
      group_by(village) %>%
      arrange(village,my.timestamp) %>%
      mutate(diff = difftime(my.timestamp, lag(my.timestamp), units="days"))


    my.summary <- df %>%
      group_by(village) %>%
      drop_na(diff) %>%
      summarise(median = median(diff), min = min(diff), max = max(diff))
