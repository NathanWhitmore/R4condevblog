### When months have unique labels
The other day I was working on a project which labelled months unusually: Month 1 was October, 2 was November etc. Because the analysis involved 
rasters it was important to keep the numbering system. However, when I changed the raster into dataframes it was better for graphing in ggplot to
have the numbers replaced by the names of the months. While the `tidyverse` is great most of the time for a situation like this it is often easiest to use base R.

### Making the mock data
Let's make some mock data which emulates this situation. 

```r
# a reference which corresponds to the order
# of the months (but not their name)
reference <- 1:9

# some unimportant data 
category <- letters[1:9]

# make a data frame
df <- data.frame(reference , category)

```
Now `df` looks like:

```r
  reference category
1         1        a
2         2        b
3         3        c
4         4        d
5         5        e
6         6        f
7         7        g
8         8        h
9         9        i
```

Next I use the **month.names[]** constant to produce the sequence of numbers which corresponds to the months:

```r
ref_names <- c(month.name[10:12], month.name[1:9])
ref_names
```   
Which results in:
```r
 [1] "October"   "November"  "December"  "January"  
 [5] "February"  "March"     "April"     "May"      
 [9] "June"      "July"      "August"    "September"

```      

Now the tricky bit, there are more months in the sequence than there are actual monitoring months. To ensure they match we use the following code, 
cleverly using the **max()** function to ensure we only include matching months:

```
my.months <-ref_names[1:max(df$reference)]
my.months 
```

Which results in:
```r
[1] "October"  "November" "December" "January" 
[5] "February" "March"    "April"    "May"     
[9] "June" 
```

Next we change `reference` to a factor. We then use the **levels()** function to ensure the factor levels are those given by `my.month`.

```
df$reference <- as.factor(df$reference)
levels(df$reference) <- factor(my.months)

```
Now let's check that it worked by calling `df`:

```  
df

    reference category
1   October        a
2  November        b
3  December        c
4   January        d
5  February        e
6     March        f
7     April        g
8       May        h
9      June        i
```

Hooray...it works!

[BACK to INDEX](index.md)
