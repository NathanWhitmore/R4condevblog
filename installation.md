### How to install the "condev" package 

**R for Conservation and Development Projects** uses example data set which are stored on GitHub. These can be easily be directly from R using the following steps:

### How to download
First install the "remotes" package via:
```
install.packages("remotes")
```

in Program R then run:
```r
library("remotes") 
install_github("NathanWhitmore/condev")
```

and then load the package via:
```r
library("condev")
```

### How to access the data
The data available for use can be viewed by the function:
```r
data()
```
To bring a data set (e.g. 'demo1') into the R environment use:
```r
data(demo1)
```
[BACK to INDEX](index.md)
