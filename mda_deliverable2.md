Mini Data Analysis Milestone 2
================

*To complete this milestone, you can edit [this `.rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are commented out with
`<!--- start your work here--->`. When you are done, make sure to knit
to an `.md` file by changing the output in the YAML header to
`github_document`, before submitting a tagged release on canvas.*

# Welcome to your second (and last) milestone in your mini data analysis project!

In Milestone 1, you explored your data, came up with research questions,
and obtained some results by making summary tables and graphs. This
time, we will first explore more in depth the concept of *tidy data.*
Then, you’ll be sharpening some of the results you obtained from your
previous milestone by:

-   Manipulating special data types in R: factors and/or dates and
    times.
-   Fitting a model object to your data, and extract a result.
-   Reading and writing data as separate files.

**NOTE**: The main purpose of the mini data analysis is to integrate
what you learn in class in an analysis. Although each milestone provides
a framework for you to conduct your analysis, it’s possible that you
might find the instructions too rigid for your data set. If this is the
case, you may deviate from the instructions – just make sure you’re
demonstrating a wide range of tools and techniques taught in this class.

# Instructions

**To complete this milestone**, edit [this very `.Rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are tagged with
`<!--- start your work here--->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from
`output: html_document` to `output: github_document`. Commit and push
all of your work to your mini-analysis GitHub repository, and tag a
release on GitHub. Then, submit a link to your tagged release on canvas.

**Points**: This milestone is worth 55 points (compared to the 45 points
of the Milestone 1): 45 for your analysis, and 10 for your entire
mini-analysis GitHub repository. Details follow.

**Research Questions**: In Milestone 1, you chose two research questions
to focus on. Wherever realistic, your work in this milestone should
relate to these research questions whenever we ask for justification
behind your work. In the case that some tasks in this milestone don’t
align well with one of your research questions, feel free to discuss
your results in the context of a different research question.

# Learning Objectives

By the end of this milestone, you should:

-   Understand what *tidy* data is, and how to create it using `tidyr`.
-   Generate a reproducible and clear report using R Markdown.
-   Manipulating special data types in R: factors and/or dates and
    times.
-   Fitting a model object to your data, and extract a result.
-   Reading and writing data as separate files.

# Setup

Begin by loading your data and the tidyverse package below:

``` r
library(datateachr) # <- might contain the data you picked!
library(tidyverse)
```

    ## Warning: package 'tidyverse' was built under R version 4.1.3

    ## Warning: package 'stringr' was built under R version 4.1.3

``` r
library(dplyr)
library(broom)
```

    ## Warning: package 'broom' was built under R version 4.1.3

``` r
library(here)
```

    ## Warning: package 'here' was built under R version 4.1.3

# Task 1: Tidy your data (15 points)

In this task, we will do several exercises to reshape our data. The goal
here is to understand how to do this reshaping with the `tidyr` package.

A reminder of the definition of *tidy* data:

-   Each row is an **observation**
-   Each column is a **variable**
-   Each cell is a **value**

*Tidy’ing* data is sometimes necessary because it can simplify
computation. Other times it can be nice to organize data so that it can
be easier to understand when read manually.

### 2.1 (2.5 points)

Based on the definition above, can you identify if your data is tidy or
untidy? Go through all your columns, or if you have \>8 variables, just
pick 8, and explain whether the data is untidy or tidy.

<!--------------------------- Start your work below --------------------------->

``` r
glimpse(vancouver_trees)
```

    ## Rows: 146,611
    ## Columns: 20
    ## $ tree_id            <dbl> 149556, 149563, 149579, 149590, 149604, 149616, 149~
    ## $ civic_number       <dbl> 494, 450, 4994, 858, 5032, 585, 4909, 4925, 4969, 7~
    ## $ std_street         <chr> "W 58TH AV", "W 58TH AV", "WINDSOR ST", "E 39TH AV"~
    ## $ genus_name         <chr> "ULMUS", "ZELKOVA", "STYRAX", "FRAXINUS", "ACER", "~
    ## $ species_name       <chr> "AMERICANA", "SERRATA", "JAPONICA", "AMERICANA", "C~
    ## $ cultivar_name      <chr> "BRANDON", NA, NA, "AUTUMN APPLAUSE", NA, "CHANTICL~
    ## $ common_name        <chr> "BRANDON ELM", "JAPANESE ZELKOVA", "JAPANESE SNOWBE~
    ## $ assigned           <chr> "N", "N", "N", "Y", "N", "N", "N", "N", "N", "N", "~
    ## $ root_barrier       <chr> "N", "N", "N", "N", "N", "N", "N", "N", "N", "N", "~
    ## $ plant_area         <chr> "N", "N", "4", "4", "4", "B", "6", "6", "3", "3", "~
    ## $ on_street_block    <dbl> 400, 400, 4900, 800, 5000, 500, 4900, 4900, 4900, 7~
    ## $ on_street          <chr> "W 58TH AV", "W 58TH AV", "WINDSOR ST", "E 39TH AV"~
    ## $ neighbourhood_name <chr> "MARPOLE", "MARPOLE", "KENSINGTON-CEDAR COTTAGE", "~
    ## $ street_side_name   <chr> "EVEN", "EVEN", "EVEN", "EVEN", "EVEN", "ODD", "ODD~
    ## $ height_range_id    <dbl> 2, 4, 3, 4, 2, 2, 3, 3, 2, 2, 2, 5, 3, 2, 2, 2, 2, ~
    ## $ diameter           <dbl> 10.00, 10.00, 4.00, 18.00, 9.00, 5.00, 15.00, 14.00~
    ## $ curb               <chr> "N", "N", "Y", "Y", "Y", "Y", "Y", "Y", "Y", "Y", "~
    ## $ date_planted       <date> 1999-01-13, 1996-05-31, 1993-11-22, 1996-04-29, 19~
    ## $ longitude          <dbl> -123.1161, -123.1147, -123.0846, -123.0870, -123.08~
    ## $ latitude           <dbl> 49.21776, 49.21776, 49.23938, 49.23469, 49.23894, 4~

Based on the definition above, this data is not tidy. Specifically, the
plant_area column contains two variables. The definition of this column
is: “B = behind sidewalk, G = in tree grate, N = no sidewalk, C =
cutout, a number indicates boulevard width in feet”. It should be split
into two columns, one with the plant area type (B = behind sidewalk, G =
in tree grate, N = no sidewalk, C = cutout, and adding in O = on
boulevard), and one with boulevard width.
<!----------------------------------------------------------------------------->

### 2.2 (5 points)

Now, if your data is tidy, untidy it! Then, tidy it back to it’s
original state.

If your data is untidy, then tidy it! Then, untidy it back to it’s
original state.

Be sure to explain your reasoning for this task. Show us the “before”
and “after”.

<!--------------------------- Start your work below --------------------------->

``` r
#BEFORE 
(vancouver_trees)
```

    ## # A tibble: 146,611 x 20
    ##    tree_id civic_number std_st~1 genus~2 speci~3 culti~4 commo~5 assig~6 root_~7
    ##      <dbl>        <dbl> <chr>    <chr>   <chr>   <chr>   <chr>   <chr>   <chr>  
    ##  1  149556          494 W 58TH ~ ULMUS   AMERIC~ BRANDON BRANDO~ N       N      
    ##  2  149563          450 W 58TH ~ ZELKOVA SERRATA <NA>    JAPANE~ N       N      
    ##  3  149579         4994 WINDSOR~ STYRAX  JAPONI~ <NA>    JAPANE~ N       N      
    ##  4  149590          858 E 39TH ~ FRAXIN~ AMERIC~ AUTUMN~ AUTUMN~ Y       N      
    ##  5  149604         5032 WINDSOR~ ACER    CAMPES~ <NA>    HEDGE ~ N       N      
    ##  6  149616          585 W 61ST ~ PYRUS   CALLER~ CHANTI~ CHANTI~ N       N      
    ##  7  149617         4909 SHERBRO~ ACER    PLATAN~ COLUMN~ COLUMN~ N       N      
    ##  8  149618         4925 SHERBRO~ ACER    PLATAN~ COLUMN~ COLUMN~ N       N      
    ##  9  149619         4969 SHERBRO~ ACER    PLATAN~ COLUMN~ COLUMN~ N       N      
    ## 10  149625          720 E 39TH ~ FRAXIN~ AMERIC~ AUTUMN~ AUTUMN~ N       N      
    ## # ... with 146,601 more rows, 11 more variables: plant_area <chr>,
    ## #   on_street_block <dbl>, on_street <chr>, neighbourhood_name <chr>,
    ## #   street_side_name <chr>, height_range_id <dbl>, diameter <dbl>, curb <chr>,
    ## #   date_planted <date>, longitude <dbl>, latitude <dbl>, and abbreviated
    ## #   variable names 1: std_street, 2: genus_name, 3: species_name,
    ## #   4: cultivar_name, 5: common_name, 6: assigned, 7: root_barrier

``` r
#Adding column for boulevard width
vancouver_trees_tidy <- vancouver_trees %>%
  mutate(boulevard_width = if_else((plant_area %in%   
                                      c(1:(max(as.numeric(vancouver_trees$plant_area), na.rm=TRUE)))), 
                                   plant_area, 
                                   ""))
```

    ## Warning in plant_area %in% c(1:(max(as.numeric(vancouver_trees$plant_area), :
    ## NAs introduced by coercion

``` r
#Changing numbers in plant_area to "O"
vancouver_trees_tidy <- vancouver_trees_tidy %>%
  mutate(plant_area = if_else((plant_area %in%   
                                      c(1:(max(as.numeric(vancouver_trees$plant_area), na.rm=TRUE)))), 
                              "O", 
                              plant_area))
```

    ## Warning in plant_area %in% c(1:(max(as.numeric(vancouver_trees$plant_area), :
    ## NAs introduced by coercion

``` r
#Checking what the entry types there are for plant_area (should only be B = behind sidewalk, G = in tree grate, N = no sidewalk, C = cutout, and O = on boulevard)
unique(vancouver_trees_tidy$plant_area)
```

    ##  [1] "N" "O" "B" NA  "C" "G" "b" "c" "L" "P" "M" "n" "0" "g" "y"

``` r
#not what we want... need to change lowercase b g n and c to uppercase and remove all other variables

vancouver_trees_tidy <-
  vancouver_trees_tidy %>%
  mutate(plant_area = toupper(plant_area)) %>% #making everything uppercase
  mutate(plant_area = factor(plant_area, levels = c("B", "G", "N", "C", "O")))

vancouver_trees_tidy <- vancouver_trees_tidy[!is.na(vancouver_trees_tidy$plant_area), ]
#making it a factor with only the 5 levels we are interested in

unique(vancouver_trees_tidy$plant_area) #That looks better!
```

    ## [1] N O B C G
    ## Levels: B G N C O

``` r
#Reordering so boulevard_width is after plant_area
(vancouver_trees_tidy <- vancouver_trees_tidy %>%
  select(tree_id:plant_area, boulevard_width, everything()))
```

    ## # A tibble: 144,434 x 21
    ##    tree_id civic_number std_st~1 genus~2 speci~3 culti~4 commo~5 assig~6 root_~7
    ##      <dbl>        <dbl> <chr>    <chr>   <chr>   <chr>   <chr>   <chr>   <chr>  
    ##  1  149556          494 W 58TH ~ ULMUS   AMERIC~ BRANDON BRANDO~ N       N      
    ##  2  149563          450 W 58TH ~ ZELKOVA SERRATA <NA>    JAPANE~ N       N      
    ##  3  149579         4994 WINDSOR~ STYRAX  JAPONI~ <NA>    JAPANE~ N       N      
    ##  4  149590          858 E 39TH ~ FRAXIN~ AMERIC~ AUTUMN~ AUTUMN~ Y       N      
    ##  5  149604         5032 WINDSOR~ ACER    CAMPES~ <NA>    HEDGE ~ N       N      
    ##  6  149616          585 W 61ST ~ PYRUS   CALLER~ CHANTI~ CHANTI~ N       N      
    ##  7  149617         4909 SHERBRO~ ACER    PLATAN~ COLUMN~ COLUMN~ N       N      
    ##  8  149618         4925 SHERBRO~ ACER    PLATAN~ COLUMN~ COLUMN~ N       N      
    ##  9  149619         4969 SHERBRO~ ACER    PLATAN~ COLUMN~ COLUMN~ N       N      
    ## 10  149625          720 E 39TH ~ FRAXIN~ AMERIC~ AUTUMN~ AUTUMN~ N       N      
    ## # ... with 144,424 more rows, 12 more variables: plant_area <fct>,
    ## #   boulevard_width <chr>, on_street_block <dbl>, on_street <chr>,
    ## #   neighbourhood_name <chr>, street_side_name <chr>, height_range_id <dbl>,
    ## #   diameter <dbl>, curb <chr>, date_planted <date>, longitude <dbl>,
    ## #   latitude <dbl>, and abbreviated variable names 1: std_street,
    ## #   2: genus_name, 3: species_name, 4: cultivar_name, 5: common_name,
    ## #   6: assigned, 7: root_barrier

<!----------------------------------------------------------------------------->

### 2.3 (7.5 points)

Now, you should be more familiar with your data, and also have made
progress in answering your research questions. Based on your interest,
and your analyses, pick 2 of the 4 research questions to continue your
analysis in the next four tasks:

<!-------------------------- Start your work below ---------------------------->

1.  *Which neighbourhoods have the most Elm and Maple trees, in total
    count and in proportion to the neighbourhood’s size?*
2.  *How does the planting environment (Root barrier presence, planting
    area type) impact tree size?*

<!----------------------------------------------------------------------------->

Explain your decision for choosing the above two research questions.

<!--------------------------- Start your work below --------------------------->

I chose these research questions because they are the ones that I found
the most interesting results for in milestone 1, and I laid out future
steps to further analyze these questions in milestone 1. I think the
other two questions that I had asked are too simplistic to continue
analyzing.
<!----------------------------------------------------------------------------->

Now, try to choose a version of your data that you think will be
appropriate to answer these 2 questions. Use between 4 and 8 functions
that we’ve covered so far (i.e. by filtering, cleaning, tidy’ing,
dropping irrelevant columns, etc.).

<!--------------------------- Start your work below --------------------------->

``` r
#Adding a scientific name column by concatenating genus and species name
vancouver_trees_tidy <-
vancouver_trees_tidy %>%
  mutate(scientific_name = paste(genus_name, species_name)) %>%
  select(tree_id:cultivar_name, scientific_name, everything()) #reordering so that it comes after the other naming variables

#Dropping columns that won't be used to make the dataset easier to work with
vancouver_trees_tidy <- 
  vancouver_trees_tidy %>%
  subset(select = 
           -c(civic_number, 
              std_street,
              assigned,
              on_street_block,
              on_street,
              street_side_name))

#Reordering columns to make more logical sense
vancouver_trees_tidy <-
  vancouver_trees_tidy %>%
  select(tree_id, date_planted, genus_name:common_name, #identifying characteristics of the tree
         neighbourhood_name, latitude, longitude, #location information
         root_barrier:boulevard_width, curb,  #physical location characteristics
         height_range_id:diameter) #tree size traits

#Making an elm and maple only subset of the tidied data
elm_maple_tidy <-
  vancouver_trees_tidy %>%
  filter(genus_name %in% c("ACER", "ULMUS"))
```

<!----------------------------------------------------------------------------->

# Task 2: Special Data Types (10)

For this exercise, you’ll be choosing two of the three tasks below –
both tasks that you choose are worth 5 points each.

But first, tasks 1 and 2 below ask you to modify a plot you made in a
previous milestone. The plot you choose should involve plotting across
at least three groups (whether by facetting, or using an aesthetic like
colour). Place this plot below (you’re allowed to modify the plot if
you’d like). If you don’t have such a plot, you’ll need to make one.
Place the code for your plot below.

<!-------------------------- Start your work below ---------------------------->

``` r
(height_plantarea <-
vancouver_trees_tidy %>%
  ggplot(aes(x=plant_area, y=height_range_id, group=plant_area)) +
   geom_boxplot(aes(fill=plant_area))+
   ylab("Height range") +
   xlab("Planting area"))
```

![](mda_deliverable2_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

<!----------------------------------------------------------------------------->

Now, choose two of the following tasks.

1.  Produce a new plot that reorders a factor in your original plot,
    using the `forcats` package (3 points). Then, in a sentence or two,
    briefly explain why you chose this ordering (1 point here for
    demonstrating understanding of the reordering, and 1 point for
    demonstrating some justification for the reordering, which could be
    subtle or speculative.)

2.  Produce a new plot that groups some factor levels together into an
    “other” category (or something similar), using the `forcats` package
    (3 points). Then, in a sentence or two, briefly explain why you
    chose this grouping (1 point here for demonstrating understanding of
    the grouping, and 1 point for demonstrating some justification for
    the grouping, which could be subtle or speculative.)

3.  If your data has some sort of time-based column like a date (but
    something more granular than just a year):

    1.  Make a new column that uses a function from the `lubridate` or
        `tsibble` package to modify your original time-based column. (3
        points)

        -   Note that you might first have to *make* a time-based column
            using a function like `ymd()`, but this doesn’t count.
        -   Examples of something you might do here: extract the day of
            the year from a date, or extract the weekday, or let 24
            hours elapse on your dates.

    2.  Then, in a sentence or two, explain how your new column might be
        useful in exploring a research question. (1 point for
        demonstrating understanding of the function you used, and 1
        point for your justification, which could be subtle or
        speculative).

        -   For example, you could say something like “Investigating the
            day of the week might be insightful because penguins don’t
            work on weekends, and so may respond differently”.

<!-------------------------- Start your work below ---------------------------->

**Task Number**: 1

``` r
(height_plantarea_reordered <-
vancouver_trees_tidy %>%
  mutate(plant_area = fct_reorder(plant_area, height_range_id)) %>%
  ggplot(aes(x=plant_area, 
             y=height_range_id,
             group=plant_area)) +
  geom_boxplot(aes(fill=plant_area)) +
  ylab("Height range") +
  xlab("Planting area"))
```

![](mda_deliverable2_files/figure-gfm/unnamed-chunk-6-1.png)<!-- --> I
chose to reorder the planting area groups by height range. I thought
that this could be useful because it would give a sense of which
planting areas can support larger trees. The reordering makes it easier
to see which groups have higher averages than the others, which is
especially useful here because the averages appear to be pretty close
together.

<!----------------------------------------------------------------------------->
<!-------------------------- Start your work below ---------------------------->

**Task Number**: 2

``` r
vancouver_trees_tidy$elm_maple_other <- fct_other(vancouver_trees_tidy$genus_name,
                                  keep = c("ULMUS", "ACER"))
  
(height_plantarea <-
vancouver_trees_tidy %>%
  ggplot(aes(x=elm_maple_other, y=height_range_id, group=elm_maple_other)) +
   geom_boxplot(aes(fill=elm_maple_other))+
   ylab("Height range") +
   xlab("Genus"))
```

![](mda_deliverable2_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

Here I grouped all genuses other than Acer and Ulmus into an “Other”
category, because Acer nad Ulmus are most likely to be affected by the
Japanese Beetle (an invasive species in Vancouver) and I wanted to see
if there are any notable differences in tree height between these tree
types and other genuses.
<!----------------------------------------------------------------------------->

# Task 3: Modelling

## 2.0 (no points)

Pick a research question, and pick a variable of interest (we’ll call it
“Y”) that’s relevant to the research question. Indicate these.

<!-------------------------- Start your work below ---------------------------->

**Research Question**: How does the planting environment (Root barrier
presence, planting area type) impact tree size?

**Variable of interest**: boulevard_width

<!----------------------------------------------------------------------------->

## 2.1 (5 points)

Fit a model or run a hypothesis test that provides insight on this
variable with respect to the research question. Store the model object
as a variable, and print its output to screen. We’ll omit having to
justify your choice, because we don’t expect you to know about model
specifics in STAT 545.

-   **Note**: It’s OK if you don’t know how these models/tests work.
    Here are some examples of things you can do here, but the sky’s the
    limit.

    -   You could fit a model that makes predictions on Y using another
        variable, by using the `lm()` function.
    -   You could test whether the mean of Y equals 0 using `t.test()`,
        or maybe the mean across two groups are different using
        `t.test()`, or maybe the mean across multiple groups are
        different using `anova()` (you may have to pivot your data for
        the latter two).
    -   You could use `lm()` to test for significance of regression.

<!-------------------------- Start your work below ---------------------------->

``` r
vancouver_trees_tidy$boulevard_width <-
  as.numeric(vancouver_trees_tidy$boulevard_width)

lm_height_boulwidth <-
  lm(height_range_id ~ boulevard_width, vancouver_trees_tidy)
```

<!----------------------------------------------------------------------------->

## 2.2 (5 points)

Produce something relevant from your fitted model: either predictions on
Y, or a single value like a regression coefficient or a p-value.

-   Be sure to indicate in writing what you chose to produce.
-   Your code should either output a tibble (in which case you should
    indicate the column that contains the thing you’re looking for), or
    the thing you’re looking for itself.
-   Obtain your results using the `broom` package if possible. If your
    model is not compatible with the broom function you’re needing, then
    you can obtain your results by some other means, but first indicate
    which broom function is not compatible.

<!-------------------------- Start your work below ---------------------------->

I’m choosing to produce the tidy results of the linear regression model.
I am specifically seeking the p-value for the regression.

``` r
tidy(lm_height_boulwidth)
```

    ## # A tibble: 2 x 5
    ##   term            estimate std.error statistic  p.value
    ##   <chr>              <dbl>     <dbl>     <dbl>    <dbl>
    ## 1 (Intercept)      2.58     0.00936      276.  0       
    ## 2 boulevard_width  0.00982  0.000972      10.1 5.38e-24

<!----------------------------------------------------------------------------->

# Task 4: Reading and writing data

Get set up for this exercise by making a folder called `output` in the
top level of your project folder / repository. You’ll be saving things
there.

## 3.1 (5 points)

Take a summary table that you made from Milestone 1 (Task 4.2), and
write it as a csv file in your `output` folder. Use the `here::here()`
function.

-   **Robustness criteria**: You should be able to move your Mini
    Project repository / project folder to some other location on your
    computer, or move this very Rmd file to another location within your
    project repository / folder, and your code should still work.
-   **Reproducibility criteria**: You should be able to delete the csv
    file, and remake it simply by knitting this Rmd file.

<!-------------------------- Start your work below ---------------------------->

``` r
rootbarrier_counts <-
    vancouver_trees %>%
      group_by(root_barrier) %>%
      summarize(count=n())

write_csv(rootbarrier_counts, here::here("output", "rootbarrier_counts.csv")) 
```

<!----------------------------------------------------------------------------->

## 3.2 (5 points)

Write your model object from Task 3 to an R binary file (an RDS), and
load it again. Be sure to save the binary file in your `output` folder.
Use the functions `saveRDS()` and `readRDS()`.

-   The same robustness and reproducibility criteria as in 3.1 apply
    here.

<!-------------------------- Start your work below ---------------------------->

``` r
saveRDS(lm_height_boulwidth, file=here("output", "lm_height_boulwidth.RDS"))
lm_height_boulwidth <- readRDS(here("output", "lm_height_boulwidth.RDS"))
```

<!----------------------------------------------------------------------------->

# Tidy Repository

Now that this is your last milestone, your entire project repository
should be organized. Here are the criteria we’re looking for.

## Main README (3 points)

There should be a file named `README.md` at the top level of your
repository. Its contents should automatically appear when you visit the
repository on GitHub.

Minimum contents of the README file:

-   In a sentence or two, explains what this repository is, so that
    future-you or someone else stumbling on your repository can be
    oriented to the repository.
-   In a sentence or two (or more??), briefly explains how to engage
    with the repository. You can assume the person reading knows the
    material from STAT 545A. Basically, if a visitor to your repository
    wants to explore your project, what should they know?

Once you get in the habit of making README files, and seeing more README
files in other projects, you’ll wonder how you ever got by without them!
They are tremendously helpful.

## File and Folder structure (3 points)

You should have at least four folders in the top level of your
repository: one for each milestone, and one output folder. If there are
any other folders, these are explained in the main README.

Each milestone document is contained in its respective folder, and
nowhere else.

Every level-1 folder (that is, the ones stored in the top level, like
“Milestone1” and “output”) has a `README` file, explaining in a sentence
or two what is in the folder, in plain language (it’s enough to say
something like “This folder contains the source for Milestone 1”).

## Output (2 points)

All output is recent and relevant:

-   All Rmd files have been `knit`ted to their output, and all data
    files saved from Task 4 above appear in the `output` folder.
-   All of these output files are up-to-date – that is, they haven’t
    fallen behind after the source (Rmd) files have been updated.
-   There should be no relic output files. For example, if you were
    knitting an Rmd to html, but then changed the output to be only a
    markdown file, then the html file is a relic and should be deleted.

Our recommendation: delete all output files, and re-knit each
milestone’s Rmd file, so that everything is up to date and relevant.

PS: there’s a way where you can run all project code using a single
command, instead of clicking “knit” three times. More on this in STAT
545B!

## Error-free code (1 point)

This Milestone 1 document knits error-free, and the Milestone 2 document
knits error-free.

## Tagged release (1 point)

You’ve tagged a release for Milestone 1, and you’ve tagged a release for
Milestone 2.

### Attribution

Thanks to Victor Yuan for mostly putting this together.
