

### Reading in foreign file formats

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

Now we need to tell R's Survey package how our data is set up. You'll see in the ACS data that there's a field for household weights and another for people. So if we're counting household-level things, we want to use the household weight (hhwt). If you're counting people-level things, you want to use the person weight (perwt). 

Let's keep things simple. The code below will create an object that holds the survey design information. 

```R
my_data.pw <- svydesign(ids=~0, weights=~perwt, data=my_data)
```

The ids variable specifies the cluster ids. You can use ~0 or ~1 for no clusters. As you can see, we used the perwt for weights because we want to analyze people-level things. If you plan to analyze both levels, you might want to create another svydesign object. This time just change the name of the object (I used .hw for household weight). 

```R
my_data.hw <- svydesign(ids=~0, weights=~hhwt, data=my_data)
```

OK. Now let's start analyzing the data. The first thing I always like to do is take a top-level look at my data to make sure I've got the data imported with all the records. The way I do it is by getting a sum of the population. We can do this just by summing up the weights. 

```R
sum(my_data$perwt)
```

Now let's do an estimate of population by state. Let's use aggregate. 

```R
aggregate(perwt ~ statefip, my_data, sum)
```

Want to store your results? Simply assign the aggregate formula to a new object. 

```R
state_pops <- aggregate(perwt ~ statefip, my_data, sum)
```

Now double click on the new state_pops object in the Global Environment to see your new table. 

OK, so we haven't used the survey object we created earlier. The Survey package has some methods that make some calculations easier. Let's say we wanted to know the breakdown by race. We can use svytotal.

```R
svytotal(~race, design=my_data.pw)
```

Uh oh. Did you get 593043987 for the answer? 

