---
layout: default
weight: 1
---

# R FOR HERITAGE: TRAINING WORKSHOP

## 15 May 2019, University of Stirling

#### Sponsored by the Scottish Graduate School for Arts and Humanities

#### Authors: Marta Krzyzanska (marta.krzyzanska@stir.ac.uk) and Dr Chiara Bonacchi (chiara.bonacchi@stir.ac.uk)

#### Part 1: Introduction to R

Welcome! In this part of the training you will be introduced to R. R is a kind of Free and Open Source statistical software that is extensively used in data science and can facilitate a broad range of analyses. In this session we will cover its basic functionality, so that you can get comfortable with the software, before we proceed with the analysis of a collection of tweets containing the hashtag #hadrianswall. We extracted this collection at two different points in time: in 2017 and in 2018.

## Introduction

To open R, go to the Launcher bar at the bottom of the screen and find R Studio (the icon looks like a blue circle with an 'R' in the middle). From now on we will refer to R Studio as just R.

R executes commands through the Script and Console windows. This means that you need to tell the software what to do by writing commands directly into the Script window of your R studio screen and then by pressing *run*. The Script window is located in the top left quadrant of your R studio screen. The bottom left quadrant is the Console window, where your outputs will appear. In the top right quadrant, you will find the Environment window, where your work is saved in R. We will leave the fourth quadrant aside for now and return to it later.

To recap: you will input code into the Script window; outputs will appear in the Console window. Now, let's learn how to execute some simple commands.

At the very basic level, R can be used as a calculator, so try to input the following command into the Script window and press *run*:


```R
2+2
```


4


This should output 4.  
Now try to input the same command *2+2* preceded by # and press run:


```R
#2+2
```

As you can see, this time nothing happened. This is because # is used to 'comment out' some parts of the input, which means that they are displayed, but not executed by the program. # is often used to include annotations just before or next to a line of code, in order to explain how the code is used and for what purposes. Annotating is a very good practice in programming! It helps you to remember why you did what you did and makes your work more accessible to yourself, later on, and to the others.

## Variables

In programming, information is stored in **variables**. You can store values and data in **variables**. To assign a specific value to a variable, use *<-*:


```R
a<-2 # This assigns the value of 2 to the variable *a*
```

It may seem like nothing happened, because the console did not return anything, but this command assigned the value of 2 to the variable *a*. In most cases, if the R console does not return anything, it is a good sign and it means that everything went well. If something goes wrong, R will show an error. To print (which means to show) the value stored in the variable *a*, simply input *a* and press run:


```R
a
```


2


This should print the value of *a*, which is 2.

In programming, these 'atomic' elements that can only hold one value at a time are called **scalars**. They are the smallest building blocks from which it is possible to create more complex data. To find out the kind of value held by a scalar, use the command class(). For example, let's check the mode of the scalar *2*:


```R
class(2)
```


'numeric'


##### Q1. Which part of the line of code below is an annotation?   

a<-2 # This assigns the value of 2 to the variable *a*

##### Q2. Now assign the value of 3 to the object *b*.  

After you have done that, subtract *a* from *b*:


```R
b-a
```


1


We can store the result of the subtraction in *c*:


```R
c<-b-a
```

##### Q3. What is the value of *c*?

##### Q4. Now assign the value of the sum of *b* and *c* to the variable *d*. What is the value of *d*?


The output of the commands other than mathematical operations can also be stored in variables. For example, let's store the information about the type of value held by *c* in a variable called *dt_c*:


```R
dt_c <- class(c)
```

Now you can check that the type of value stored in *c* is numeric:


```R
dt_c
```


'numeric'


But what is the type of value stored in the variable *dt_c*? Let's check:


```R
class(dt_c)
```


'character'


'Character' is another common type of data, which represents either a single character or multiple characters. In programming, character values are also commonly called **strings**. Strings can be used to store textual data, such as the text of the tweets. To assign a string value to a variable, you need to put the variable in the double quotation marks. For example, create a variable called *tweet* and assign the text "this is my tweet" to it:


```R
tweet <- "this is my tweet"
```

##### Q5. Now try to print the variable *tweet*.

R supports different types of data. Remember that only certain operations can be done with certain types of data. In the next few sections, we will look at few more common data types.


## Vectors and lists

A series of atomic elements of the same type - for example numeric or characters - can be combined into a **vector**. If the sequence is composed of elements of different type, then it is called a **list**. Let's put all our variables of type numeric (*a*,*b*,*c*,*d*) in the vector *numbers*:


```R
numbers <- c(a,b,c,d)
```

Now type:


```R
numbers
```


<ol class="list-inline">
	<li>2</li>
	<li>3</li>
	<li>1</li>
	<li>4</li>
</ol>



You can also add elements to a vector at a specific point of the vector. For example, to add 5 as the 5th element of the vector *numbers*, type:


```R
numbers[5]<-5
```

##### Q6. Now try to print *numbers* again.

As you can see above, vectors are created by using the *c()* function and placing the elements of the vector in brackets, separated by commas.   

**Functions** are methods. They are collections of statements that allow you to carry out a task. You can use them to transform and/or query data in a predefined way. Functions take **arguments** that are put in brackets and, if there is more than one, these arguments are always separated by commas.
&nbsp;


R has a series of built-in functions that allow undertaking useful operations. For example, to add all the numbers stored in the vector *numbers* use:


```R
sum(numbers)
```


15


To check how many elements the vector *numbers* contains, use the *length()* function:


```R
length(numbers)
```


5


To know what a function does and how it is used, just type *?* before the name of the function and the information will be provided to you:


```R
?sum()
```

##### Q7. What is the argument in the function sum(numbers)?

Now, let's make another vector that contains strings instead of numeric elements. For example, make a vector with some types of crowdsourcing applications that are available on the MicroPasts crowdsourcing website for archaeology, history and heritage (crowdsourced.micropasts.org):


```R
applications <- c("transcription","georeferencing","photomasking","photo-tagging","video-tagging")
```

##### Q8. Print *applications*.

Now, let's create a vector that contains the names of the developers of those crowdsourcing applications:


```R
developers <- c("Daniel","Dan","Andy","Adi","Chiara")
```

##### Q9. Print *developers*.

## Matrices and data frames

Two or more vectors can be combined into a **matrix** with the functions *cbind()* (to combine them by column) or *rbind()* (to combine them by row). Execute both functions to see how the results look like:


```R
cbind(applications,developers)
```


<table>
<thead><tr><th scope="col">applications</th><th scope="col">developers</th></tr></thead>
<tbody>
	<tr><td>transcription </td><td>Daniel        </td></tr>
	<tr><td>georeferencing</td><td>Dan           </td></tr>
	<tr><td>photomasking  </td><td>Andy          </td></tr>
	<tr><td>photo-tagging </td><td>Adi           </td></tr>
	<tr><td>video-tagging </td><td>Chiara        </td></tr>
</tbody>
</table>




```R
rbind(applications,developers)
```


<table>
<tbody>
	<tr><th scope=row>applications</th><td>transcription </td><td>georeferencing</td><td>photomasking  </td><td>photo-tagging </td><td>video-tagging </td></tr>
	<tr><th scope=row>developers</th><td>Daniel        </td><td>Dan           </td><td>Andy          </td><td>Adi           </td><td>Chiara        </td></tr>
</tbody>
</table>



As you can see, a **matrix** is a collection of elements of the same type - character, numeric, or logic - arranged in a two-dimensional rectangular layout. The matrix created with *cbind()* has 5 rows and 2 columns, while the matrix created with *rbind()* has 2 rows and 5 columns.

Let's create a matrix by combining our two vectors by column: *applications* and *developers*, and call it *matrix*:


```R
matrix <-cbind(applications,developers)
```

A matrix can be further transformed into a data frame, which is a data type commonly used to store tables:


```R
table <-as.data.frame(matrix)
```

Differently from matrices, data frames can contain elements of different types. For example, they can contain both characters and numeric values.

Another way to create a data frame is by using the *data.frame()* function and including the names of the vectors we want to use as columns inside the brackets. For example, let's create a data frame called *mytable* using the vectors *numbers* and *applications*, where *applications* is the first column and *numbers* is the second column:


```R
mytable <- data.frame(applications, numbers)
```

If you have a data frame and do not know the names of the columns, you can find out what these are by typing:


```R
colnames(mytable)
```


<ol class="list-inline">
	<li>'applications'</li>
	<li>'numbers'</li>
</ol>



To access a specific column of which you know the name, simply write the name of the data frame that stores it, followed by *$* and the name of the column:


```R
mytable$applications
```


<ol class="list-inline">
	<li>transcription</li>
	<li>georeferencing</li>
	<li>photomasking</li>
	<li>photo-tagging</li>
	<li>video-tagging</li>
</ol>



There is also another way to access the entire content of a column or a row. For example, to access the entire first raw, type:


```R
mytable[1,]
```


<table>
<thead><tr><th scope="col">applications</th><th scope="col">numbers</th></tr></thead>
<tbody>
	<tr><td>transcription</td><td>2            </td></tr>
</tbody>
</table>



And to access the entire second column, type:


```R
mytable[,2]
```


<ol class="list-inline">
	<li>2</li>
	<li>3</li>
	<li>1</li>
	<li>4</li>
	<li>5</li>
</ol>



You can also access data stored in a specific part of a data frame, by putting the relevant number of the row and of the column in square brackets. For example, to access the data stored in the 1st row and the second column of the table, type:


```R
mytable[1,2]
```


2


Finally, when the dataset contained in a data frame is large and you wish to quickly inspect its structure, it may be helpful to have a look at the content of the first few rows. To do this, use the head() command:


```R
head(mytable)
```


<table>
<thead><tr><th scope="col">applications</th><th scope="col">numbers</th></tr></thead>
<tbody>
	<tr><td>transcription </td><td>2             </td></tr>
	<tr><td>georeferencing</td><td>3             </td></tr>
	<tr><td>photomasking  </td><td>1             </td></tr>
	<tr><td>photo-tagging </td><td>4             </td></tr>
	<tr><td>video-tagging </td><td>5             </td></tr>
</tbody>
</table>



## Saving data

Now let's save some of the data we created.

First, create a folder called *Rheritage* inside the *Users* folder that you can access from your Desktop. Second, tell R that the *Rheritage* folder you have just created will be your working directory, which is the space in your computer where you will be working and, in this case, saving data. You can do this by copy pasting the path to the *Rheritage* folder into the *setwd()* function. Make sure that you use / in the path and that the path is in quotation marks:


```R
setwd("/Users/yourusername/Documents/Rheritage")
```

Having done this, we can start saving data. A common format for storing tables is .csv, which stands for comma-separated-values. A .csv file consists of rows of textual data separated by commas. Such files can be easily imported into R as data frames with the *read.csv()* function, but can also be opened with programs such as Excel or even notepad. To save the table you created before as a .csv file named "mytable", type:


```R
write.csv(mytable, file="mytable.csv")
```

R also allows you to save the whole workspace with all its contents:


```R
save.image("my_workspace.Rdata")
```

Alternatively, you can also save any value you assigned, data or function you created as an R object that can be directly read into R (but not with any other program):


```R
save(mytable,file="mytable.R")
```

You should now have 3 new files in the folder defined as your working directory. To load R objects or workspaces into R, simply use the *load()* command and input the name of the object or directory in quotation marks (make sure your workspace is set to the same directory in which the files are located):


```R
load("mytable.R")
load("my_workspace.Rdata")
```

And to import a .csv file:


```R
mytable<-read.csv("mytable.csv")
```

Well done, you have reached the end of the section!

## Answers:  

##### Q1: Which part of the line of code below is an annotation?   

a<-2 # This assigns the value of 2 to the variable *a*


```R
# This assigns the value of 2 to the variable a
```

##### Q2. Now assign the value of 3 to the variable *b*.  


```R
b<-3
```

##### Q3. What is the value of *c*?

1

##### Q4. Now assign the value of the sum of *b* and *c* to the object *d*. What is the value of *d*?


```R
d<-b+c
d
```


4


##### Q5. Now try to print the variable tweet.


```R
tweet
```

    [1] "this is my tweet"


##### Q6. Now try to print numbers again.


```R
numbers
```

    [1] 2 3 1 4 5


##### Q7. What is the argument in the function sum(numbers)?

numbers

##### Q8. Print applications.


```R
applications
```


<ol class="list-inline">
	<li>'transcription'</li>
	<li>'georeferencing'</li>
	<li>'photomasking'</li>
	<li>'photo-tagging'</li>
	<li>'video-tagging'</li>
</ol>



##### Q9. Print developers


```R
developers
```


<ol class="list-inline">
	<li>'Daniel'</li>
	<li>'Dan'</li>
	<li>'Andy'</li>
	<li>'Adi'</li>
	<li>'Chiara'</li>
</ol>
