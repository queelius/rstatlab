Week 2: Reading Data in R
================

To do any type of analysis, we need *data* to analyze.

Typically, the data comes from some experiment, where we randomly sample
from some population of interest and measure some set of features of interest
for each unit of the sample.

In this lesson, the data has already been generated, and we consider various
ways to read the data into R for further analysis.

Matrices
--------

We often store numerical data in a rectangular format called matrices with two dimensions, rows and columns.
Last week we created matrices by binding two vectors using the `cbind()` function.
Another way is to use the `matrix()` function.

``` r
# store the vector (1, 2, 3, 4, 5, 6, 7, 8) into a 2x4 matrix named A
A <- matrix(c(1,2,3,4,5,6,7,8), nrow = 2, ncol = 4) # nrow = # of rows, ncol = # of columns
A
#      [,1] [,2] [,3] [,4]
# [1,]    1    3    5    7
# [2,]    2    4    6    8
```

This populates the matrix by column.
If we wanted to fill the matrix by row, we use `byrow = TRUE` argument.

``` r
B <- matrix(1:8, nrow = 2, ncol = 4, byrow = TRUE) # 1:8 is another way of generating  the vector
B
#      [,1] [,2] [,3] [,4]
# [1,]    1    2    3    4
# [2,]    5    6    7    8
```

So that others may understand your results, make data self-describing.
Comments is one way to do this.
We can also assign column and row names using `colnames()` and `rownames()`, respectively.

``` r
colnames(A) <- c('C1', 'C2', 'C3', 'C4')
rownames(A) <- c('R1', 'R2')
A
#    C1 C2 C3 C4
# R1  1  3  5  7
# R2  2  4  6  8
```

Accessing the element of a matrix can be done using `[row, col]` notation.

``` r
A[1, 3]   # row 1, col 3 entry
# [1] 5
A[1, 1:3] # row 1, col 1 to 3
# C1 C2 C3 
#  1  3  5
A[1, ]    # row 1
# C1 C2 C3 C4 
#  1  3  5  7
A[ , 3]   # col 3
# R1 R2 
#  5  6
```

Data Frames
-----------

Each element of a matrix in R must be of the same type, e.g., Boolean.
However, in an experiment, we often measure two or more features that are of
different types, e.g., for persons, we may simultaneously measure the `sex` of the person, which is categorical variable whose outcome is either `male` or `female`,
and the `height` of the person, which is a numerical variable whose outcome is
any positive number.

To accomodate such data, we use data frames. Data frames allow each column of
data to be different, e.g., a `sex` column that consists of `male` or `female`
and a height column that consists of numbers.

Data frames are like Excel spreadsheets where each column represents a variable or measurement and each row represents an observational unit.
We can create a data frame using the function `data.frame()`.

We can check the structure of the data frame using the function `str()`. The output includes data information such as

-   dimension (number of observations and variables),
-   variable names
-   data type
    -   `logi`: logical, `TRUE` or `FALSE`
    -   `int`: integer
    -   `num`: numerical
    -   `chr`: character
    -   `Factor`: Factors are how R keeps track of categorical variables,
                  like `male` or `female`.

``` r
stark.kids <- data.frame(
  Name = c("Jon", "Sansa", "Arya", "Bran"),
  Age = c(24, 20, 18, 17)
)
str(stark.kids)
# 'data.frame': 4 obs. of  2 variables:
#  $ Name: chr "Jon", "Sansa", "Arya", "Bran"
#  $ Age : num  24 20 18 17
stark.kids  
#    Name Age
# 1   Jon  24
# 2 Sansa  20
# 3  Arya  18
# 4  Bran  17
```

We can also use the names of variables in a data frame to access the variable using the notation `data$name`.

``` r
stark.kids$Name
# [1] Jon   Sansa Arya  Bran 
# Levels: Arya Bran Jon Sansa
mean(stark.kids$Age) # average age
# [1] 19.75
```

Note that in the posted lesson, the `Name` column is shown as a factor with
4 levels. This is typically not want you want, so R was changed to, by default,
model a vector of strings as strings.
To turn them into factors, you may use the `factor` command, e.g.:

``` r
stark.kids$Name <- factor(stark.kids$Name)
str(stark.kids)
```


Working with data frames included in R packages
-----------------------------------------------

There are a lot of pre-built data frames that come with R, such as the data
`chickwts`. The data was the result of an experiment conducted to compare the effectiveness of various feed supplements on the growth rate of chickens.

``` r
str(chickwts) # check data structure
# 'data.frame': 71 obs. of  2 variables:
#  $ weight: num  179 160 136 227 217 168 108 124 143 140 ...
#  $ feed  : Factor w/ 6 levels "casein","horsebean",..: 2 2 2 2 2 2 2 2 2 2 ...
```

We can display selected rows using `head()` and `tail()` commands; or selected cell entries.

``` r
head(chickwts, 4) # display first 4 rows
#   weight      feed
# 1    179 horsebean
# 2    160 horsebean
# 3    136 horsebean
# 4    227 horsebean
tail(chickwts, 2) # display last 2 rows
#    weight   feed
# 70    283 casein
# 71    332 casein
```

Reading CSV data in R
---------------------

Often, you'll be getting data from a data file.
A popular file format for data are comma separated files (.csv).
Saving data files in .csv format is standard in practice.
We use the command `read.csv` to read .csv data in R.

    read.csv(file, header = TRUE)

The arguments of the `read.csv` function includes (among others)

-   `file`: name or URL (Universal Resource Locator) of the data set

-   `header = TRUE`: if the file contains the names of the variables as its first line.

### A) Loading a data set from a webpage using its URL.

The article *Going Wireless (AARP Bulletin, June 2009)* reported the estimated percentage of households with only wireless phone service (no land line). In the accompanying data table, each state was also classified into one of three geographical regions—West (W), Middle states (M), and East (E).

The URL of the data set `Going Wireless` that we need to read the data is (<https://goo.gl/72BKSf>).

``` r
wireless.data <- read.csv("https://goo.gl/72BKSf", header = TRUE)

# we'd like to convert the Region column into factors
wireless.data$Region <- factor(wireless.data$Region)
```

We can examine the structure of the data frame `wireless.data` and
the first 2 elements:
``` r
str(wireless.data) # check structure
# `data.frame':	51 obs. of  3 variables:
# Wireless: num  13.9 11.7 18.9 22.6 9 16.7 5.6 5.7 20 16.8 ...
# Region  : chr  "M" "W" "W" "M" ...
# State   : chr  "AL" "AK" "AZ" "AR" ...
> 
head(wireless.data, n=2) # display first 2 rows using the named argument n
```

### B) Loading a data set from a local file in the working directory.

R is always pointed at a directory/folder on your machine where it looks for data sets and source files. To check your current working directory, you can run the command `getwd()` in the RStudio console.

There are a number of ways to change the current working directory:

-   Use the [setwd](https://stat.ethz.ch/R-manual/R-devel/library/base/html/getwd.html) R function

-   Click `Session > Set Working Dir > Choose Directory` menu . This will also change directory location of the Files pane.

-   From within the `Files` panel (lower right), click `More > Set As Working Directory` menu. (Navigation within the Files pane alone will not change the working directory.)

> As good practice, save your `.Rmd` exercise file and your `.csv` data file in the same folder. Since you cannot control resources on the web, to avoid possible
issues with a URL resource changing or becoming unavailable, I generally suggest
making a local copy of the resource, which facilitates reproducibility. (A lot
of current research is not reproducible; science by definition requires
experiments to be reproducible by others.)

### Data on flight delays on the tarmac

Download `flight.delay.csv` from this [link](https://goo.gl/QjCxDz). To load the data in the local file `flight.delay.csv` from a folder in your machine, you need to change the working directory to that folder or use the file's fully qualified name.

``` r
getwd() # no arguments needed
delay <- read.csv("flight.delay.csv", header = TRUE)
head(delay)
#          Airline Delays Rate.per.10K.Flights
# 1     ExpressJet     93                  4.9
# 2    Continental     72                  4.1
# 3          Delta     81                  2.8
# 4         Comair     29                  2.7
# 5 American Eagle     44                  1.6
# 6     US Airways     46                  1.6
```

Note that since `flight.delay.csv` is at the web address (URL)
`https://goo.gl/QjCxDz`, which is shortened URL for
`https://raw.githubusercontent.com/jpailden/rstatlab/master/data/flight.delay.csv`,
we may alternatively simply refer to URL in the argument to `read.csv`:

``` r
delay <- read.csv("https://goo.gl/QjCxDz", header = TRUE)
```

Or, using the unshortened URL:

``` r
delay <- read.csv(
  "https://raw.githubusercontent.com/jpailden/rstatlab/master/data/flight.delay.csv", header = TRUE)
```

But, again, I generally advice downloading the file. 

------------------------------------------------------------------------
