

###Reading in foreign file formats

We downloaded the iPUMS data in Stata format. Stata, like SAS or SPSS, are costly statistical packages. R can't natively import these file formats, so we need to use a package to read them. I use [Haven](https://cran.r-project.org/web/packages/haven/haven.pdf). 

Let's install Haven. 

```R
install.packages("haven")
```

Now let's set our working directory 

```R
setwd("D:/newdata2/survey")
```

And before you can actually use a package like Haven, you have to tell R to load it. Let's do that. 

```R
library(haven)
```

OK, now we're ready to load our data. We're going to load the data and assign it to the my_data object. 

```R
my_data <- read_dta("usa_00022.dta")
```

The most important thing about dealing with survey data like the ACS or CPS is the use of a weight variable. Each record is weighted. For example, your first record might have a weight of 1.76. That means it is not counted one time, but rather 1.76 times. Doing these calculations in SQL could be a pain, but it's quite a bit easier with a statistical package like R. 

But to do this, let's install another R package: [Survey](https://cran.r-project.org/web/packages/survey/survey.pdf).

```R
install.packages("survey")
```

And just like with Haven, we need to tell R to use it. 

```R
library("survey")
```





