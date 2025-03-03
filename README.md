# NICAR25R1

Hello and welcome to Intro to R/R1, we'll be analyzing some data from Nebraska's Department of Health and Human services that details deaths in Nebraska between 2019 and mid-2024. Below are the code chunks that you'll copy into R Studio during the class. Parts of this document come from NICAR's wonderful team.

## Getting started with R Studio

RStudio is your dashboard for working with R: writing scripts, viewing variables, even reading documentation on the fly

When you open RStudio, you see the window is divided into sections:

On the left is the **Console**, where you can write and run code. When you start writing code in script files it will reflect the code you've run. It also tells you what version of R you are running. When you create or open script files, they will appear above the Console, pushing it to the lower-left quadrant.

The upper-right quadrant has several tabs, the first of which is **environment**: if you store any information in variables (such as a table of data), they will show up there.

The lower-right quadrant also has several tabs, the first of which is Files: it shows what files exist in your current working directory, which is usually your root directory when you first open RStudio. We will change this later.

We encourage the use of **Notebook** files (.Rmd), which enable you to both write comments (which is what I'm writing here what I'm writing here) and to write code, in what we call a **code chunk**:

```{r}
x <- "Hello, World!"
print(x)
```

In notebook files, the results of the code in each code chunk will print below the chunk, allowing you to quickly see the results of your work. That's helpful when we start asking questions of data.

You create a code chunk by typing `cmd+option+i` and you write your `code` in between the first and last lines of the code chunk. All your comments, questions, thoughts, and notes are written outside of the code chunks.

### Part Two: Project setup

Before you start creating files, let's talk organization. With a language like R, it will make your life easier if you keep all your files in a consistent structure and use R project files. Use this setup for every project you undertake with R, whether it's this class or data analysis for your next story.

**Create a folder for the project.** Name it something that makes sense, with no spaces, such as "my-project" (but be more specific than that). Put it somewhere on your computer, such as the Documents folder. It's best not to put it in your root directory, which is `/Users/username` for Macs and usually `C:\Users\username` on Windows computers. If you have OneDrive on your computer, avoid putting these files on OneDrive if at all possible; it tends not to work well.

Navigate to the project folder you created in the lower-right quadrant, then click into your folder. Set this folder as your working directory by clicking the gear icon above the files and click "set as working directory."

### Part Three: Learning terminology

Some important terminology has come up already:

-   **Notebook**: a certain type of R script that allows us to write whatever text we want, including `code` inside of code chunks
-   **code chunk**: created by typing `cmd+option+i`, type your code inside code chunks.
-   **environment**: this is basically your workspace for every R session. It's empty until you start storing information in variables.
-   **variable**: a variable is a container that holds something. For example, the following code stores the word "spaghetti" into a variable named "x", using the assignment operator `<-`, and then prints the contents of that variable to the console below the code chunk:

```{r}
x <- "spaghetti"
print(x)
```

In the example above, "spaghetti" is a string of text, or characters. You can also store numbers in variables (numbers do not require double quotes):

```{r}
y <- 3
print(y)
```

Naming variables: you can use letters, numbers, underscores and dots.

-   **data types**:
    -   `chr` (character): commonly referred to as a string; can be letters, numbers, punctuation, etc. Always enclosed in "double quotes".
    -   `int` (integer): a whole number, such as `1`, `5`, `10000`. No decimal places.
    -   `dbl` or `num` (number): a number that can have decimal places, such as `5.2` or `100.37`
    -   `POSIX` or `dttm` (datetime): dates (always formatted `yyyy-mm-dd`) and times
    -   `logi` (logical): either `TRUE` or `FALSE` (not quoted)
    -   complex: e.g. `3 + 2i` (we won't use this)
    -   raw: (won't use this either)
-   **vector**: this is a common feature of R that you will use regularly. A vector is a series of values. Vectors can only store elements of the same data type, for example all strings or all numbers, and are created with the `c()` 

```{r}
x <- c("spagetti sauce", "noodles", "parmesan")
y <- c(1,2,3)
```

-   **function**: these are the backbones of all analysis, in any program (from spreadsheets to database managers to programming languages). Every function has a particular structure and does a particular thing. The structure is: `function_name(arguments)`. For example, the `sum()` function works in R the same way it works in other programs:

```{r}
x <- 1
y <- 2
sum(x,y)
```

-   **pipe**: a pipe does what it sounds like: it pipes information from one function to the next. It is a part of the [tidyverse](https://www.tidyverse.org/) **package**, which is the primary set of tools we use for data analysis in R. You will use the pipe a lot, and it looks like this: `%>%`. A shortcut for typing it is `cmd+shift+m` (or `ctrl+shift+m`).
-   **package**: a set of features and functions that are not a part of base R (what you initially installed), but that you can add to R to increase its functionality.

# Setting up with the packages we want to use

The install.packages function pulls down the package and adds it to your RStudio. You should only need to do it once for each package. When you "library" the package, it tells RStudio that you're going to use it in this document, so you'll need to do it on each new doc.

For today's exercise, we're going to use two very common R packages: tidyverse and dplyr. You will likely want them for all of your data analysis projects, so it is smart to start your documents with the library() function.

```{r}
knitr::opts_chunk$set(echo = TRUE)
install.packages(tidyverse)
install.packages(dplyr)
library(tidyverse)
library(dplyr)
```

### Part Four: The key to understanding programming...

This might be an overstatement, but it *is* very important. It's critical to understand, when coding, how **information is passed around**. For the sake of this class, let's refer to information as **data**, although it won't always be tabular. Data can be stored in a variable or printed to the **console** (which is directly below a code chunk in R Notebooks). Data stored as a variable shows up in the environment, and can be referred to later in your script.

Additionally, data can be passed (or **piped**) through functions that filter, sort, aggregate, and/or mutate it in some way. But it will always end up either printing to the console or being stored in a variable.

### Part Five: Time to write some code

Open up a Markdown (Rmd) file by clicking file -> new file -> R Markdown... in the top bar. Enter a title (something like Intro to R) and click Okay.

You'll notice that it comes with some code already written out. There are instructions about how to run code, create code chunks, and there's even some sample code you can run. To run it click the green "play" button in the upper-right corner of a code chunk. You can also run individual lines of code in a code chunk by putting your cursor on that line and typing `cmd + return` (or `ctrl + enter` on a PC).

Here's where you'll want to start with your package code. Add the lines below into the code block that's already on the page, or feel free to create another code block below. Run the code by hitting the green arrow.

```{r}
install.packages("tidyverse")
install.packages("dplyr")
library(tidyverse)
library(dplyr)
```

# Importing the data

Now we need some data to work with. There is a file on your computer called nebraska_deaths.csv

If it's not there, or you're working off of your own computer, download the file from the GitHub repository: https://github.com/dherbers/NICAR25R1/blob/main/nebraska_deaths.csv

Place the file in the folder you created earlier. 

Now, we'll bring it into R so we can analyze it. This is called "reading" in the data. We want to use the variable function to save the data to our environment.

```{r}

nebraska_deaths <- read_csv("nebraska_deaths.csv")

```

Now nebraska_deaths should appear in your environment, if you click it, it will open in another tab and you can preview the data. It's a fairly clean and simple data set, but there's a couple things I like to do to get it ready.

The first step is setting up the column names. If your column name has a space, you will need to remove it so the computer can parse the name when you start running code.

These columns are already pretty clean, but we'll rename the date_filed column to be more descriptive using the rename() function. The new column name goes first, followed by the current name.

```{r}

nebraska_deaths <- nebraska_deaths %>%
  rename(date_certificate_filed = date_filed)

```

The second step we'll take is changing all of the county names to upper case. This is a good basic data cleaning practice to standardize the data.

To create a new column or change the values of a column, we'll use tidyverse's mutate function on our county column. Within the mutate, we'll also use the toupper function with (county) to tell the computer that's the column we want to use.

```{r}

nebraska_deaths <- nebraska_deaths %>%
  mutate(county = toupper(county))

```

Now we can ask the data some questions. First, I'd like to find the statewide autopsy rate. For that, we'll need to know how many deaths ocurred, and how many were autopsied. 

The count() function will give us the total entries for each value in the autopsy column. Then, we can do some quick calculations in the console. Add the N and Y values for the total, then divide the Y value by the total and multiply by 100. 

The autopsy rate should be 6.988%

```{r}

nebraska_deaths %>%
  count(autopsy)

```

We can now use the same calculations to find the autopsy rate of individual counties using the group_by() and summarize() functions.

Summarize, like mutate, will create a new column with the values from the function based on the column in the group by. For example, grouping by county and summarizing by total_deaths = n() will create a table of counties with column total_deaths counting the total entries for each county.

We'll also programmatically calculate our percentages instead of typing each calculation into the console manually, using the mean() function within the arrange() function. 

Below, mean(autopsy == "Y") will check for Y values, meaning an autopsy was done, then give the proportion of yeses. Multiplying by 100 converts the number into a percentage.

We will save our results into the environment by using the variable function from earlier, naming our variable something relevant.

```{r}

county_totals <- nebraska_deaths %>%
  group_by(county) %>%
  summarize(
    total_deaths = n(), 
    percent_autopsied = mean(autopsy == "Y") * 100)

```

Once we have these numbers, we can use the arrange() function to organize our data from lowest to highest. If you want to reverse it to be highest to lowest, use arrange(desc()).

We'll place the column name percent_autopsied into the parenthesies to arrange by percentage. You could swap it for total_deaths to see which counties have had the most or least deaths.

```{r}

county_totals %>%
  arrange(percent_autopsied)

county_totals %>%
  arrange(desc(percent_autopsied))

```

Now we can use the data to find the autopsy rate of a specific population. The central character of our story, Pete, was a 56 year old man living in Douglas County. For the story, I wanted to find out how likely Pete was to be autopsied based on those characteristics.

First, the filter() function will return only the values specified. Using the == sign, we can ask for the values that match Pete's characteristics. Between each filter value, put a comma to act as an "and" for the function. 

```{r}

deaths_pete_peers <- nebraska_deaths %>%
  filter(county == 'DOUGLAS', age_group == '50-59', sex == 'M')

```

Finally, we will calculate the autopsy rate for this specific group, using a similar strategy as before. We can group by autopsy done and count the values, then run the percentage calculation again in the console with the numbers it returns.

N + Y = total, then (Y / total) *100

```{r}

deaths_pete_peers %>%
  group_by(autopsy) %>%
  count(autopsy)

```

Based on his demographics, Pete would have had a 19% chance of being autopsied. 
With these basic code functions, we can answer other questions about when autopsies and deaths are ocurring based on gender, location and age, which can reveal trends for additional reporting.
