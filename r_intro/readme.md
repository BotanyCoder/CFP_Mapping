# Day 1: Getting Started with R

Based on a document created by: Jason Pienaar and Tom Miller
Edited by: CCG and OMV 2023

## Outline

### Learning objectives:
* Interacting with RStudio
* Recognizing different elements in R and their functionality
* Load  data in R from external tools


What you should have at the end of this tutorial
* A word document with answers to Tasks1-10.
* A script document with the code you produced in this tutorial.
* A basic understanding of how to run basic R code and obtain results.


### Content:
Introduction
Accessing R
Tips for coding
What does it look like and what does it mean?
Simplest tricks in R
R functions
Importing files

## Introduction

Why R?
R is an independent, open source statistical computing environment, incorporating implementations of an older commercial program called “S” by an international team of statisticians. Most other statistics packages are principally orientated towards applying standard methods of data analysis, which is great, but it is difficult to apply non-standard methods or to add to the capabilities of existing methods. This is primarily due to their limited programming capabilities. In addition, they are pretty expensive and require new licensing agreements etc. One of the great strengths of R (other than that it is free) is the relative ease with which new capabilities can be added (the R language is really easy to use). Thus, the user can easily create new functions or combine existing functions or data BUT in order to use R we need to learn the R language hence the simple tutorials.
 
The truth about R:
1- There is nothing more frustrating than when your code does not work
2- There is nothing more satisfying than when your code does work!

Anything worth doing, from losing weight to getting a degree, takes time. Learning R is no different. Especially if this is your first experience programming, you are going to experience a lot of headaches when you get started. You will run into error after error and pound your fists against the table screaming: “WHY ISN’T MY CODE WORKING?!?!? There must be something wrong with this stupid software!!!” You will spend hours trying to find a bug in your code, only to find that - frustratingly enough, you had had an extra space or missed a comma somewhere. You’ll then wonder why you ever decided to learn R when (::sigh::) Excel was so “nice and easy.”

## Accessing R 

### on Cal Poly Humboldt labs:
Although you can download in your personal computer (and you are encouraged to do so – find instructions on Canvas), we will work on a standardized set up on the school lab. 

> Add the flag to the right corner of your monitor ![](img/yellow.jpeg)

* We can open Rstudio by simply typing `Rstudio` in the search bar of the OS.
 
### Obtaining your own copy of R
Both PC and Macintosh versions of R can be downloaded from the [R home page](http://www.r-project.org/)
 
## Before you start 

### Best practices

### Tips to learn to code
* PRACTICE! A good practice consists on writing code. Do not copy and paste, do not assume that you got it by reading the tutorial: type it yourself.
* A better practice is to make sure you understand what each line of code is doing. Use # to annotate your code (see below to find out what I mean).
* Keep a notebook with these explanations: ‘File_tutorial2.R line 102: this function sums all the numbers in a vector’. 

### Some R resources
[Useful R Reference Cheatsheet by Tom Short](https://cran.r-project.org/doc/contrib/Short-refcard.pdf).  
[Basic skills by Quick R](https://www.statmethods.net).  
[Advanced skills Cheatsheet by Arianne Colton and Sean Chen](https://rstudio.com/wp-content/uploads/2016/02/advancedR.pdf).  
[Other resources from YaRrr! The pirate’s guide to R](https://bookdown.org/ndphillips/YaRrr/r-resources.html).  

## What does it look like and what does it all mean
![Fig1](img/Fig1_CIRMBioinf.jpeg) 
 
Figure 1 is a typical workspace in Rstudio, with four panels. You may have to open a new script (source) when you open the program for the first time: click on the new script icon to the top-left corner (see Fig 1, indicated with a yellow arrow)The most important panels right now are on the left, the source and the console. The source code in the top-left corner is where you will type your code and save your script. The console is where the code runs. You can write code directly to the console to find whether it works or not, or the answer to a calculation. You can also send your code from the source to the console to run. Importantly, you will be saving your source code, if you are typing in the console, you will be unable to save your progress or reuse your code. Therefore, I recommend you ALWAYS type and work on the source panel and send your code to run and see the output on the console. The panels on the right will display your data tables, plots, help and other features, but we will get to those later.
 
### Task 1
In these tutorials, you will regularly find tasks interspersed with guidelines and code. In these tasks, you have to use the code you learned to complete some challenge, then upload a document with the results from each task (see provided example). Task 1 consists of some very simple exercises in R. Answer these on a separate word document and upload to your Canvas assignment page once you are finished.  
a. Try typing ‘5’ in your source code, or any other number or letter, and then click on ‘Run’, near the top-right of the source panel. What happened?  
b. We can type comments on the source code that don’t run on the console by using ‘#’. Anything written to the right of the ‘#’ will not run. Try typing: ‘5 # is my favorite number’ in the source, and click on ‘Run’. What happened?  
c. Save your script: On the RStudio top menu, select save as, and save your script (right now you should have 2 lines, if you answered a and b above) as ‘myfirstscript.R’. Save all your progress from this tutorial and upload the script file to your Canvas assignment as well.  

### Saving your work
As with any other type of file, you need to save your scripts for later use. When working in R or Rstudio, you will be saving the Source code. This is a common misconception: you will not be saving the output in the console, so make sure you are typing your code in the Source code window and then running it, otherwise you risk losing your work and being unable to replicate it. In R studio, go to File and Save as, just like you would with other types of documents. 

When working in vlabs: Make a folder with your name in the Desktop and store all your files in there (scripts, outputs, data files). You will then need to send these files to yourself using your Google Drive, Dropbox or email. IMPORTANT: do not log out of vlab before you store your files in the cloud! The files will be deleted when you log in and you will lose your work. 
 
## The Simplest Tricks with R:

### Basic arithmetic
Open the R program by double clicking on the R icon. Under the opening message, you will find the “>” prompt, waiting for you to ask R to do something. Data analyses in R proceeds as an interactive dialogue. We type an S statement at the > prompt, press Enter, and the interpreter executes the statement, i.e. by returning a result, producing graphical output or sending output to a file or device. Try typing in the following simple arithmetic examples (just type what follows the prompt > and then <kbd>Enter<kbd>).

```
2+3
2-3
2*3
2/3
2^3
4^2-3*2
(2-3)*(2*3)
```

The usual precedence for mathematical operators is followed (multiplication and division first, then addition and subtraction). In general, R ignores spaces and so they are not necessary, but for bigger expressions spaces may improve the readability.
 
### Task 2
In these tutorials, you will regularly find tasks interspersed with guidelines and code. In these tasks, you have to use the code you learned to complete some challenge, then upload a document with the results from each task (see provided example). Task 1 consists of doing simple calculations in R. In the document you turn in, show your code and the answer for each one.
Example:

```
455*544

```

You should get:
> 455*544  
> [1] 247520
 
Run the following calculations:

```
49923*199
3333+111
322+543 * 234+23
```

And: The year you were born – today’s date (just the day) * your favorite number

> remove your flag if you are good to continue ![](img/green.jpeg)

## Variables
R can store numbers and objects as variables in its temporary memory.

> Add the flag to the right corner of your monitor ![](img/yellow.jpeg)

Run the following code
```
a = 49923*199
b = 3333+111
c = 322+543 * 234+23
```
What is different?

Now run:
```
a + b + c
```

What would you type if you wanted to save the previous calculation in a variable?

## R functions
R is a functional programming language meaning that pretty much everything we do in R is in terms of functions. R includes hundreds of built-in functions for mathematical calculations (including matrix algebra, which is extremely useful in statistics), data analyses, graphing, etc. Values passed to functions are specified within parenthesis after the function name. Here are some simple examples to try:



```
log(100)
log(100, base=10)
seq(1, 4)
seq(2, 8, by=2)
seq(0, 1, length=11)
```

To obtain help or additional info on a function, type ? before the function name or help(function name) and press Enter. (Note: the function log returns the natural log).
 
### Task 3
Explore the functions seq and rep. Use the help to find more about them. Explain what they do in your own words and show an example for each (code and output).
 
## Data types and data structures

### Variables and Vectors
Most R functions (including the simple arithmetic ones from above) can operate on more complex data structures that individual numbers. The simplest data structure (and one that we will use often) is a vector. To construct a vector use the c function

```
c(1, 2, 3, 4)
```

The “c” function combines all the numbers you provide into a vector or a list. From now on, we will give these vectors a variable name so that we can re-use them. To do this simply assign a name to the vector using “=” symbol and press Enter (some may prefer the old assignment symbol of “<-“, but “=” is simpler and more intuitive). Note that R is case sensitive so Vector1 and vector1 are not the same variable.

```
Vector1 = c(1, 2, 3, 4)
```

To see what is stored in the variable simply type the variable name and press Enter (that is, now type “Vector1”, hit return, and R will show you Vector1). The sequence operator from above (seq) also returns a vector. Functions applied to vectors operate on an element-wise basis. Thus:

```
Vector2 = log(Vector1)
```

returns the natural log values of the elements stored in Vector1 and then stores them in the variable Vector2 (note: you will probably use this at some point to log-transform a variable). The rules for naming variables in S are simple: variable names are composed of letters (a-z, A-Z), numerals (0-9) and periods (.) and can be of any length (the first character cannot be a number, and spaces are not allowed). Consequently, variables can be given descriptive names so that you don’t forget what the variable is. For example: “this.is.a.variable.containing.log.values.for.vector1” is a valid variable name. Stupid, maybe, but valid.

```
this.is.a.variable.containing.log.values.for.vector1 = log(Vector1)
```

However, remember that you will have to type it out again to recall the variable. Unlike in many programming languages, variables in S are dynamically defined and redefined so we do not need to tell the interpreter how many values, what type (real, integer etc) or whether it is numeric or a character. We can also redefine a variable simply by assigning it to a different function e.g.

```
Vector2 = rnorm(100)
```

Here the previous values in Vector2 are replaced by 100 standard-normal random numbers (the default mean = 0 and standard deviation = 1, we could easily change these defaults eg: rnorm(20, mean=25, sd=17) returns a vector containing 20 numbers drawn randomly from a set of normally distributed numbers with mean 25 and standard deviation 17). If we wish to print only one of the elements of a vector we index the element using square brackets as in the following example.

```
Vector2[21]
```

returns the 21st element of the vector.

> Remove your flag if you are good to continue ![](img/green.jpeg)

### Lists
While vectors only contain numbers, lists can contain mixed types of elements in R. You can find numbers, strings (characters), logical arguments and even lists nested within lists. 

Create a list using the following code:

> Add flag to the right corner of your monitor ![](img/yellow.jpeg)

```
list_data=list(color='red', vector=c(21,23,43), password=TRUE, Temperature=21.5)
print(list_data)
```

Notice how printing your list gives you each of the stored pieces of information in order, regardless of their class. You can inquire about the types of data you have at each position of the list:

```
list_data[3]
class(list_data[[3]])
```
Uh! Logical class. This class of elements in R indicate true or false statements. We had not run into these before. If you try the other elements in the list you will find numeric, character strings and lists. The next layer of complexity is data frames and matrices, but, importantly, by now you have probably realized that R is much more than just a calculator. It can manipulate data, conduct much of the same statistics covered in SAS, JMP, Canoco, etc., and has excellent graphic capabilities. It can also serve as a programming language. We will cover examples of these in subsequent days of this workshop.

Let's practice some of these skills before we move on.
 
### Task 4
Practice your skills with variables and vectors! For each of the following items, show your code and your output.
1. Create a variable “random.set” containing 300 elements, randomly drawn from a normal distribution of elements with a mean of 2 and standard deviation 0.5.
2. Create a second variable that contains the natural log values of the above elements
3. Use the function mean to return the means of the two variables
4. Use the function var to return the variances of the two variables
5. The function “plot” will create a separate window on your screen with a standard
labeled plot. Type plot(variable) to create a scatter plot of your variables against their indices, substituting your variable name into the brackets, and also plot(variable1, variable2) to plot your variables against each other. Paste your plot to the document you turn in.

> Remove your flag if you are good to continue ![](img/green.jpeg)

# Day 2: Importing datasets

Day 2, bonus using Dplyr, basic stats, and loops

The power of coding relies heavily on being able to analyze large datasets with few lines of code. A good package to work with table-like datasets is dplyr. We will import a dataset of plant occurrences and then calculate several statistics on those.

## Filtering data and getting some basic statistics

> Add flag to the right corner of your monitor ![](img/yellow.jpeg)

- First create a folder in your documents or desktop named `day_2`.

- Then download the following dataset and add it to the folder. The best way to do this is to access this github page from inside vlab.

[Abronia dataset](https://www.dropbox.com/s/qtjfbay88qo35fs/abronia_3_red.csv?dl=0) 

- Now that you have `abronia_3_red.csv` inside the folder `day_2` go to Rstudio and open a new script.

- Save the new script as `abronia.r` inside the folder `day_2`

### Installing our first R package

R functions like a dock in which you can install packages for analyses. A very useful package for working with dataframes is dplyr.

```
install.packages("dplyr")
```

Now that the library is installed we can load it.
```
library(dplyr)
```

Now we need to tell R about your working folder. Look for the address of one of your files inside your `day_2` folder,

```
setwd("C:/Users/ov20/Desktop/day_2")
```

We can always check our working directory by typing:
```
getwd()
dir()
```

Now we can import our dataset
```
data = read.csv("abronia_3_red.csv")
```

> Remove your flag if you are good to continue ![](img/green.jpeg)

This dataframe is pretty large so there is no point in printing it all to the screen. To check that our dataset has been successfully imported, we can print to screen only the “head” of the dataset.

> Add flag to the right corner of your monitor ![](img/yellow.jpeg)

```
head(data)
```

Because this is a dataset with numerous columns, R will stack the rows on top of each other.

Similarly, we can print the bottom or tail of the dataset, asking R to print the 10 last rows.

```
tail(data, 10)
```

We can see here that we have more than 17 thousand records in our dataset.

We can also see the structure of the dataframe by doing:

```
str(data)
```

Our dataset contains museum and inaturalist records for all the species of sand verbenas (a charismatic plant that can be found at the dunes in Arcata). We are interested in studying the distribution of sand verbenas, and therefore we would like to know several statistics about the latitudinal range for the genus and species.

Since we are interested in describing the latitudinal range for these plants, a first calculation is to find the mean of the latitude.

```
mean(data$decimalLatitude)
```

Opps!!!!

We can see that there is something wrong with the command or the dataset, any ideas what can be wrong? Let's check the last 100 values of the Latitude:

```
tail(data$decimalLatitude, 100)
```

> Remove your flag if you are good to continue ![](img/green.jpeg)

We can see that there are multiple rows without values “NAs”.  with dplyr we can easily filter out those. Let's create a new dataframe with only data that has latitude `geodata`.

> Add flag to the right corner of your monitor ![](img/yellow.jpeg)

```
geodata = data %>% filter(!is.na(decimalLatitude))
```

Dplyr uses the following syntax:

NEWDATASET = DATASET %>% FILTER

In our case, we want to keep numeral entries; for this we are using the function `filter` and the function `is.na`. If we were to use `data %>% filter(is.na(decimalLatitude))`, we will only keep the rows with NAs, which is the opposite of what we want. The character `!` means “is not”, asking R keep in the new dataframe everything that is not NA.


Let’s check that our dataset does not have NAs in the latitude column:

```
tail(geodata$decimalLatitude, 100)
```

Now let’s try to calculate the mean latitude for all species combined.
```
mean(geodata$decimalLatitude)
```

***NOTE*** There are always numerous ways of solving a problem while coding; another way of calculating the mean while ignoring the NAs is `mean(data$decimalLatitude, na.rm = T)`. Working with a clean dataset usually helps to run code more efficiently, so we will keep working with `geodata`.

We can also calculate other statistics on the latitude.

```
median(geodata$decimalLatitude)
min(geodata$decimalLatitude)
```

## Exercise 1.
Using the filter `filter(species == "Species of interest")` (notice that this is just filter of the line necessary for the line). Find the maximum longitude for “Abronia latifolia”.

<details>
  <summary>Click to see an answer!</summary>
  
```
abla = geodata %>% filter(species == "Abronia latifolia")
max(abla$decimalLongitude)
```
  
</details>

> Remove your flag if you are good to continue ![](img/green.jpeg)

## Basic plotting and model testing
We can easily compare the latitudinal range among species using a boxplot.

> Add flag to the right corner of your monitor ![](img/yellow.jpeg)

```
boxplot(geodata$decimalLatitude ~ geodata$species, las = 2)
```

We will use the dataset of Abronia latifolia (our local sand verbena) that you subsetted in the past exercise for plotting and fitting a linear model. Slide the bar to the right to see the answer for subsetting the data.

<details>
  <summary>Click to see the code!</summary>
  
```
abla = geodata %>% filter(species == "Abronia latifolia")
```
  

</details>

We can start by plotting the latitude 
```
plot(abla$decimalLatitude)
```

We can see that when we plot only latitude our X axis represents the index number inside the vector created by calling only the latitude.

Another way of viewing our data is by creating a histogram of the latitude.

```
hist(abla$decimalLatitude)
```

In a similar fashion, we can inspect the longitude data

```
plot(abla$decimalLongitude)
hist(abla$decimalLongitude)
```

We can also plot latitude and longitude in a single plot, which in this case is pretty much a basic map.

```
plot(abla$decimalLongitude, abla$decimalLatitude)
```
What do you see?

Just for fun, we can test whether or not we can predict the latitude by the longitude. ***Note*** This test might not be appropriate because we are not sure if our data is normal. 
```
model1 = lm(abla$decimalLatitude ~ abla$decimalLongitude)
summary(model1)
```

We can see the fit of the model by plotting
```
plot(abla$decimalLongitude, abla$decimalLatitude)
abline(model1)
```

> Remove your flag if you are good to continue ![](img/green.jpeg)

## Optional Exercise: advance plotting

Ggplot can create advance figures. Install ggplot and plot the latitude of all species in a single figure color-coded by species. Adapt the following code to our data:

https://ggplot2.tidyverse.org/

<details>
  <summary>Click to see an answer!</summary>
  
```
install.packages("tidyverse")
install.packages("ggplot2")
library(ggplot2)

ggplot(geodata, aes(decimalLongitude, decimalLatitude, colour = species)) + geom_point()

```

</details>

## Creating a map for a single species
Install and load required packages
```
install.packages("tidyverse")
install.packages("ggplot2")
library(ggplot2)
```


1. Create a subdataset for a single species
```
abla <- geodata %>% filter(species == "Abronia latifolia")
head(abla)
tail(abla)
```

2. Create a name for the resulting pdf file with the map
```
pdfName <- "Abronia latifolia.pdf" 
```

3. Draw the map
```
basemap = map_data("world")
ggmap(basemap) + geom_point(data = abla, aes(x=decimalLongitude, y=decimalLatitude)) + ggtitle("Abronia latifolia")

```

4. Save the map
```
ggsave(filename = pdfName, plot=last_plot()) 
```


5. Create a name for the csv file
```
csv_name <- "Abronia_latifolia.csv"
```

6. Save the dataset just for one species, Abronia latifolia
```
write.csv(abla, csv_name)
```


Cool!

## Using a loop to draw a map for each species

In R a loop uses the floowing logic

for (item in item_list){
	perform_action_1
	perform_action_2
	etc
}

In our case our loop would look like this, notice that it contains every step used previously to create a single map
```
#0 create a list of species
Ab_sp <- geodata %>% group_by(species) %>% tally()

for (x in (Ab_sp$species)){
	print(x)
	# 1 creating a dataset for only one species
	sp_data <- geodata2 %>% filter(species == x)
	print("subset of ocurrences created")
	
	# 2 creating file name for map
	file_title <- paste(x, ".pdf", sep = "")

	# 3 creating the map
	ggmap(basemap) + geom_point(data = sp_data, aes(x=decimalLongitude, y=decimalLatitude), color ="blue") + ggtitle(x) # creating the map
	print("map created")

	# 4 saving plot in pdf						
	ggsave(filename = file_title, plot=last_plot()) 
	print(file_title)

	# 5 creating file name for dataset for every species
	dataset_title <- paste(x, ".csv", sep = "")

	# 6 saving dataset for species
	write.csv(sp_data, dataset_title)	
}

```
 
After runing the code you should be be able to see 23 pdf files in your working folder!
