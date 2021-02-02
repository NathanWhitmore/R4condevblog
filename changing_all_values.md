### Changing all values in a data frame

Often you will inherit a data frame in which there is a systematic mistake in data entry. Often this involves how absent, NA, or zero values have to be treated. We can make large scale changes to the dataframe using the square brackets `[ ]` in base R (see page 50 of book). Let's make a small mock data frame:

```r
x <- LETTERS[1:4]
y <- c(0,1,0,1)
z <- c(0,1,1,1)

original <- data.frame(x,y,z)
```

Let's look at our orginal dataframe:

```
original

  x y z
1 A 0 0
2 B 1 1
3 C 0 1
4 D 1 1
```
WARNING: the following code will make permanent changes to your object. So as a consequence we will make copies based on our orginal data frame (i.e. v1, v2)

```r
v1 <- original
v2 <- original
```

For example we could change all 1s to 2s. To do this we use the logical operators (given on page 113). In this example all elements of v1 which equal 1 will be changed to 2:

```r
v1[v1==1]  <- 2

v1
```

Which results in:

```
  x y z
1 A 0 0
2 B 2 2
3 C 0 2
4 D 2 2
```
Using this method we could even change numbers to text:

```r
v2[v2==1]  <- "cat"

v2
```
Which results in:

```
  x   y   z
1 A   0   0
2 B cat cat
3 C   0 cat
4 D cat cat

```

Now let's change all zero values to NA. using the same procedure. In this example all elements of v2 which equal zero will be turned to NA:

```r
v2[v2==0]  <- NA

v2
```
Which results in :
```
  x  y  z
  x    y    z
1 A <NA> <NA>
2 B  cat  cat
3 C <NA>  cat
4 D  cat  cat
```

However while changing numbers or text is easy changing NAs back to some value (such as zero) requires using the **is.na()** function as unfortunatley `v2[v2==NA] <- 0` doesn't work as we might expect:

```r
v2[is.na(v2)]  <- 0 

v2
```

Which results in:

```
  x   y   z
1 A   0   0
2 B cat cat
3 C   0 cat
4 D cat cat
```


     
[BACK to INDEX](index.md)
