# Geog4/6300: Lab 1


## Loading data into R, data transformation, and summary statistics

**Your name:** Wil Gamble **Overview and lab criteria:**

This lab is intended to assess your ability to use R to load data and to
generate basic descriptive statistics. For this lab to be marked
complete, the following criteria must be met:

4.  Identify and apply appropriate data filtering and cleaning
    strategies to prepare datasets for analysis. (Task 2)
5.  Effectively interpret the code you create, explaining in plain
    language what each step does to the data. (Task 6)
6.  Identify and use appropriate external documentation — including
    package references, help files, and peer resources — to learn and
    apply unfamiliar functions or methods. (Task 7)
7.  Filter, aggregate, and transform datasets using grouping and summary
    operations to answer specific analytical questions. (Tasks 1 & 5)
8.  Reshape data between wide and long formats to meet the requirements
    of different analytical or visualization tasks. (Task 4)
9.  Create effective data visualizations across multiple chart types
    (line, scatter, histogram, Q-Q plot), applying appropriate aesthetic
    choices such as color, grouping, and labeling. (Task 3 & 5)

**Data:**

You’ll be using monthly weather data from the Daymet climate database
(http://daymet.ornl.gov) for all counties in the United States over an
12-year period (2010-2021). These data are available on the GitHub repo
for our course. The following variables are provided:

- `cty_txt`: Code for joining to census data
- `year`: Year of observation (with an initial “Y” to make it a
  character)
- `month`: Month of observation (1 = Jan, 2 = Feb, etc.)
- `median_tmax`: Median maximum recorded temperature (Celsius)
- `median_tmin`: Median minimum recorded temperature (Celsius)
- `sum_prcp`: Total recorded precipitation for the month (mm)
- `cty_name`: Name of the county
- `state`: State of the county
- `region`: Census region (map:
  https://www2.census.gov/geo/pdfs/maps-data/maps/reference/us_regdiv.pdf)
- `division`: Census division
- `X`: Longitude of the county centroid
- `Y`: Latitude of the county centroid

These labs are meant to be done collaboratively, but your final
submission should demonstrate your own original thought (don’t just copy
your classmate’s work or turn in identical assignments). Your answers to
the lab questions should be typed in this Quarto template. You’ll then
render the document to a GitHub markdown document and upload it to your
class GitHub repo.

**Procedure:**

Load the tidyverse package and import the data:

``` r
library(tidyverse)

daymet_data <- read_csv("data/daymet_monthly_median_2010-2021.csv")
```

We can look at the first few rows of the dataset using the *head()*
function. We also use *kable* to format the output as a readable table..

``` r
kable(head(daymet_data))
```

| cty_txt | year | month | median_tmax | median_tmin | sum_prcp | cty_name | state | region | division | x | y |
|:---|:---|---:|---:|---:|---:|:---|:---|:---|:---|---:|---:|
| G02060 | Y2010 | 1 | -4.27 | -10.83 | 10.04 | Bristol Bay | Alaska | West Region | Pacific Division | -156.7011 | 58.74213 |
| G02185 | Y2010 | 1 | -20.73 | -28.20 | 0.00 | North Slope | Alaska | West Region | Pacific Division | -153.4411 | 69.30696 |
| G02180 | Y2010 | 1 | -16.50 | -23.72 | 5.75 | Nome | Alaska | West Region | Pacific Division | -163.9703 | 64.89492 |
| G02050 | Y2010 | 1 | -11.20 | -18.90 | 24.55 | Bethel | Alaska | West Region | Pacific Division | -159.7678 | 60.92187 |
| G02261 | Y2010 | 1 | -13.93 | -20.03 | 15.84 | Valdez-Cordova | Alaska | West Region | Pacific Division | -144.4573 | 61.57080 |
| G02170 | Y2010 | 1 | -5.10 | -12.42 | 35.84 | Matanuska-Susitna | Alaska | West Region | Pacific Division | -149.5702 | 62.31653 |

There are a lot of observations here, 452,448 to be exact. To get a
better grasp on the data, we can use `group_by()` and `summarise()` from
the tidyverse package. This will allow us to identify the mean value for
each year by county across the study period.

## Task 1

*Use `group_by()` and `summarise()` to calculate the mean minimum
temperature for each year by county across all months, also including
State and Region as grouping variables. Your resulting dataset should
show the value of tmin for each county in each year. Use the `kable()`
and `head()` functions as shown above to call the resulting table.*

``` r
Min_Temp_Daymet<-daymet_data%>%
  group_by(state,region,cty_name,year)%>%
  summarise(median_of_tmin=mean(median_tmin))
```

    `summarise()` has regrouped the output.
    ℹ Summaries were computed grouped by state, region, cty_name, and year.
    ℹ Output is grouped by state, region, and cty_name.
    ℹ Use `summarise(.groups = "drop_last")` to silence this message.
    ℹ Use `summarise(.by = c(state, region, cty_name, year))` for per-operation
      grouping (`?dplyr::dplyr_by`) instead.

``` r
kable(head(Min_Temp_Daymet))
```

| state   | region       | cty_name | year  | median_of_tmin |
|:--------|:-------------|:---------|:------|---------------:|
| Alabama | South Region | Autauga  | Y2010 |       10.66375 |
| Alabama | South Region | Autauga  | Y2011 |       10.81333 |
| Alabama | South Region | Autauga  | Y2012 |       12.19917 |
| Alabama | South Region | Autauga  | Y2013 |       11.21500 |
| Alabama | South Region | Autauga  | Y2014 |       10.76167 |
| Alabama | South Region | Autauga  | Y2015 |       13.09750 |

``` r
# Your code goes here
```

## Task 2

*Let’s shift to the state level, focusing on those in the South Region.
Filter the original data frame (`daymet_data`) to just include counties
in this region. Then calculate the mean minimum temperature by year for
each state. For an optional extra challenge, use the `round()` function
to include only 1 decimal point. Use `kable()` and `head()` to call the
first few lines of the resulting table.*

``` r
South_Region_tmin_mean<-daymet_data%>%
  filter(region=="South Region")%>%
  group_by(state,region,year)%>%
  summarise(median_of_tmin=mean(median_tmin))
```

    `summarise()` has regrouped the output.
    ℹ Summaries were computed grouped by state, region, and year.
    ℹ Output is grouped by state and region.
    ℹ Use `summarise(.groups = "drop_last")` to silence this message.
    ℹ Use `summarise(.by = c(state, region, year))` for per-operation grouping
      (`?dplyr::dplyr_by`) instead.

``` r
kable(head(South_Region_tmin_mean))
```

| state   | region       | year  | median_of_tmin |
|:--------|:-------------|:------|---------------:|
| Alabama | South Region | Y2010 |       10.24551 |
| Alabama | South Region | Y2011 |       10.73490 |
| Alabama | South Region | Y2012 |       11.83369 |
| Alabama | South Region | Y2013 |       10.79009 |
| Alabama | South Region | Y2014 |       10.18975 |
| Alabama | South Region | Y2015 |       12.54363 |

``` r
# Your code goes here
```

## Task 3

*To visualize the trends, we could use ggplot to visualize change in
mean temperature over time. Create a line plot (`geom_line`) showing the
state means you calculated in task 2. Use the `color` parameter to show
separate colors for each state. You may also need to define the state as
a group in the aesthetic parameter.*

``` r
ggplot(South_Region_tmin_mean,
       aes(x=median_of_tmin,
           y=state,
           color=state))+
  geom_line()
```

![](Lab1_Loading_data_and_basic_stats_files/figure-commonmark/task3-1.png)

``` r
# Your code goes here
```

## Task 4

*If you wanted to look at these data as a table, you’d need to have it
in wide format. Use the `pivot_wider()` function to create a wide-format
version of the data frame you created in task 2. In this case, the rows
should be states, the columns should be the years, and the data in those
columns should be mean minimum temperatures. Then call the whole table
using `kable()`.*

``` r
South_Region_tmin_mean_Wide<-South_Region_tmin_mean%>%
  pivot_wider(names_from=state,
              values_from=median_of_tmin)
  

kable(South_Region_tmin_mean_Wide)
```

| region | year | Alabama | Arkansas | Delaware | District of Columbia | Florida | Georgia | Kentucky | Louisiana | Maryland | Mississippi | North Carolina | Oklahoma | South Carolina | Tennessee | Texas | Virginia | West Virginia |
|:---|:---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| South Region | Y2010 | 10.24551 | 10.079350 | 8.710000 | 9.231250 | 14.19376 | 10.34788 | 7.313726 | 13.24872 | 8.334948 | 10.98403 | 8.675038 | 9.568831 | 10.16305 | 8.279781 | 11.47886 | 7.409715 | 4.910909 |
| South Region | Y2011 | 10.73490 | 10.169006 | 8.837778 | 9.299584 | 15.75246 | 11.17051 | 7.892549 | 13.88003 | 8.623281 | 11.43144 | 9.117442 | 9.515498 | 10.95912 | 8.572684 | 12.15796 | 7.880649 | 5.756598 |
| South Region | Y2012 | 11.83369 | 11.119139 | 9.178194 | 9.466667 | 16.39962 | 12.35534 | 8.214351 | 14.68066 | 8.734462 | 12.16839 | 9.727054 | 10.350790 | 11.81840 | 9.359118 | 12.81577 | 8.217077 | 5.662424 |
| South Region | Y2013 | 10.79009 | 9.174078 | 8.275417 | 8.518750 | 16.24793 | 11.20243 | 6.749837 | 13.20297 | 7.787986 | 10.96740 | 8.707479 | 8.527890 | 10.61620 | 7.862009 | 11.47434 | 7.327907 | 4.829826 |
| South Region | Y2014 | 10.18975 | 9.112750 | 7.555417 | 7.978333 | 15.44876 | 10.81830 | 6.659774 | 12.69992 | 7.008941 | 10.53040 | 8.566662 | 8.813268 | 10.42804 | 7.575233 | 11.53815 | 6.839179 | 4.189849 |
| South Region | Y2015 | 12.54363 | 10.780661 | 8.657917 | 8.896667 | 17.51590 | 12.78961 | 8.019806 | 14.84642 | 8.107795 | 12.83943 | 10.239342 | 9.778474 | 12.29996 | 9.361088 | 12.42113 | 8.209383 | 5.642818 |
| South Region | Y2016 | 11.86675 | 10.927544 | 8.832361 | 9.101250 | 16.68010 | 12.08799 | 8.387410 | 14.96477 | 8.392500 | 12.40992 | 9.841467 | 10.167760 | 11.79163 | 9.147855 | 12.88090 | 8.169474 | 5.866697 |
| South Region | Y2017 | 12.35919 | 11.046489 | 9.269167 | 9.960834 | 17.16158 | 12.65043 | 8.477073 | 15.40160 | 8.864080 | 12.91975 | 10.016312 | 9.960471 | 12.10562 | 9.333417 | 12.88292 | 8.376147 | 6.098197 |
| South Region | Y2018 | 11.99053 | 10.279783 | 8.847917 | 9.126250 | 16.68835 | 12.22129 | 8.075319 | 14.52474 | 8.396684 | 12.20355 | 9.853292 | 9.157338 | 11.75577 | 9.175232 | 12.07293 | 8.159727 | 5.799614 |
| South Region | Y2019 | 12.34111 | 10.576967 | 9.268194 | 9.791250 | 17.00906 | 12.72917 | 8.359674 | 14.40792 | 8.914427 | 12.50770 | 10.471858 | 9.294513 | 12.22753 | 9.573070 | 12.00986 | 8.686776 | 6.241008 |
| South Region | Y2020 | 12.28553 | 10.509972 | 9.571944 | 9.969583 | 17.24558 | 12.68696 | 8.207757 | 14.83411 | 9.219514 | 12.59737 | 10.136233 | 9.413074 | 12.21150 | 9.262618 | 12.42005 | 8.600852 | 6.157992 |
| South Region | Y2021 | 11.80322 | 10.749478 | 8.871806 | 9.590000 | 16.62846 | 11.99530 | 8.075701 | 14.70991 | 8.681406 | 12.44660 | 9.622996 | 9.877798 | 11.45334 | 9.062390 | 12.47404 | 8.050179 | 5.836871 |

``` r
# Your code goes here
```

## Task 5

*Let’s assess the relationship of heat and precipitation by region.
Returning to the original dataset, create a data frame that shows the
mean maximum temperature and mean precipitation for all states in 2015,
also including region as a subgroup in your `group_by`. Then use ggplot
to create a scatterplot (`geom_point`) for these two variables, coloring
the points using the region variable.*

``` r
Mean_Temp_Precip<-daymet_data%>%
  filter(year=="Y2015")%>%
   group_by(state,region)%>%
  summarise(mean_of_tmax=mean(median_tmax), 
            mean_prcp=mean(sum_prcp))
```

    `summarise()` has regrouped the output.
    ℹ Summaries were computed grouped by state and region.
    ℹ Output is grouped by state.
    ℹ Use `summarise(.groups = "drop_last")` to silence this message.
    ℹ Use `summarise(.by = c(state, region))` for per-operation grouping
      (`?dplyr::dplyr_by`) instead.

``` r
ggplot(Mean_Temp_Precip,
       aes(x=mean_of_tmax,
           y=mean_prcp,
         color=region))+
  geom_point()
```

![](Lab1_Loading_data_and_basic_stats_files/figure-commonmark/task5-1.png)

``` r
# Your code goes here
```

## Task 6

*In the space below, explain what each function in your code for task 5
does to the dataset in plain English.*

The first function of the code is creating a new object that will
transfer alterations to a new name. That is
Mean_Temp_Precip\<-daymet_data. The %\<% is a “pipe”. I understand it as
a separation for a new function to be run. It gives the code a sequence
of events. filter() is used to denote what from a column we want to keep
and be used in our data set. You start with the column header then equal
it to the column variable you want. group_by() takes a whole column and
transfers it to the new object. summarize() allows you to create a new
column/data frame that at the same time can perform a function like
average, mean, etc. summarise(median_of_tmax=mean(median_tmax) creates a
new column called “median_of_tmax” based on mean data from the existing
column “median_tmax”.

ggplot() is used to create/alter different graphs. You start with the
newly (or any) data set created. The aes function can then be used to
tell ggplot what columns are going to control the graph. You can then
decide the x/y axis using x=, y=. The color= function will make the data
of a certain column different colors. Ones that share a name will be the
same color. geom_point() is what visualizes all that is above into a
scatter plot. There are multiple geom_blanks() that provide different
graphs.

*Your response here.*

## Task 7

*The `dplyr` package also includes `across` function. Use `?across` on
the R command line to open the documentation for this function. In the
space below, explain what it does in your own words. Then interpret the
way the across function is used below, going line by line within the
function.*

``` r
state_2015 <- daymet_data %>%
  filter(year == "Y2015") %>%
  group_by(region, state) %>%
  summarise(
    across(
      c(median_tmax, sum_prcp),
      mean,
      na.rm = TRUE,
      .names = "mean_{.col}"
    )
  )
```

    Warning: There was 1 warning in `summarise()`.
    ℹ In argument: `across(c(median_tmax, sum_prcp), mean, na.rm = TRUE, .names =
      "mean_{.col}")`.
    ℹ In group 1: `region = "Midwest Region"`, `state = "Illinois"`.
    Caused by warning:
    ! The `...` argument of `across()` is deprecated as of dplyr 1.1.0.
    Supply arguments directly to `.fns` through an anonymous function instead.

      # Previously
      across(a:b, mean, na.rm = TRUE)

      # Now
      across(a:b, \(x) mean(x, na.rm = TRUE))

    `summarise()` has regrouped the output.
    ℹ Summaries were computed grouped by region and state.
    ℹ Output is grouped by region.
    ℹ Use `summarise(.groups = "drop_last")` to silence this message.
    ℹ Use `summarise(.by = c(region, state))` for per-operation grouping
      (`?dplyr::dplyr_by`) instead.

{Your response here}

The across function appears to remove the need to type out the new
individual names and mean function each time. The across( c(median_tmax,
sum_prcp), mean is making the code take the mean of the existing columns
named in c(). na.rm = TRUE prevents na values from preventing the
formation of columns properly. It will tell R to ignore NA values. This
allows the mean calculation to be performed without interference. .names
= “mean\_{.col} adds mean\_ to the start of median_tmax, and sum_prcp.
For just 2 columns, it is not too useful here. If I needed to deal with
50 columns, I could see the value.

## Challenge Question

In class, we covered ways of working with the Daymet API. Create a
script below that uses the **daymetr** package to download data from
Daymet for a place (or places) of your choosing. Then visualize the
temporal pattern for a variable of your choosing in this place, similar
to what you did in question 4. Use a dplyr function (`mutate()`,
`summarise()`, `filter()`, etc.) to do any needed data wrangling and
create a visual using ggplot.

In addition to this code, write a short summary of a pattern that’s
evident in the data you visualized.

``` r
library(daymetr)

terrell_daymet<-download_daymet(site="terrell County, Ga",lat=31.77,lon=-84.43,start=2010,end=2020,internal=TRUE)
```

    Downloading DAYMET data for: terrell County, Ga at 31.77/-84.43 latitude/longitude !

    Done !

``` r
Terrell_data<-terrell_daymet$data


Terrell_Mean_Mtemp_precip_2016<-Terrell_data%>%
  filter(year=="2016")%>%
   select(yday,tmax..deg.c.,prcp..mm.day.)

ggplot(Terrell_Mean_Mtemp_precip_2016,
       aes(x=tmax..deg.c.,
           y=prcp..mm.day.,
         color=tmax..deg.c.,
         ))+
  geom_line()
```

![](Lab1_Loading_data_and_basic_stats_files/figure-commonmark/challenge-1.png)

``` r
# Your code goes here
```

*Explanation goes here.*

For my graph, I wanted to see if there was a pattern between hotter
temperatures and more precipitation in 2016. Really it ended up showing
that 1. Terrell is either boom or bust for precipitation and 2. It rains
more in the fall/spring than the summer/winter. Most of the rainfall is
concentrated in the middle of the graph which is where temperatures are
at their least extreme. The highest rains are around 66-80 degrees which
makes sense. That is the temp range for May. It gets pretty wet then
from my memory and a quick Google search corroborated that.

http://weather.uga.edu/mindex.php?variable=AV&site=DAWSON
(Dawson=terrell county)

## Final Submission Stuff

### Disclosure of Assistance

Besides class materials, what other sources of assistance did you use
while completing this lab? These can include input from classmates,
relevant material identified through web searches (e.g., Stack
Overflow), or assistance from ChatGPT or other AI tools. How did these
sources support your own learning in completing this lab?

This lab I mainly used ChatGPT, when I popped an error code. My most
common mistake was finding a similar chunk in a script but forgetting to
place a %\>% in my code. It would say for example “object
‘median_of_tmin’ not found”. I scrambled trying to figure it out, but it
was just the lack of a pipe. You helped me quite a bit, when I had all
the na data in task 4. I just did not follow task 2 correctly which led
to a wrong answer down the line. I tried group_by() for the challenge
question but it gave me too many variables. CHATgpt told me to use
select instead. I now understand group_by() is for keeping summarize
together with certain columns. Select just lets you pick out certain
columns. *Response here.*

### Lab Reflection

How do you feel about the work you did on this lab? Was it easy,
moderate, or hard? What are the biggest things you learned by completing
it?

This lab was easier than the first to me. I mainly focused on using
script 10 and 8 to understand how the different code chunks needed to be
formatted. I have better notes now too which helps. I did not have to
use much generative A.I this time. If I have a template, I can normally
figure out what to do. Some errors would have been a headache to solve
without it though. I would give this one a moderate compared to lab
one’s hard. Focusing on the scripts helped.

The challenge question was not to bad once I found what I needed in
script 3. It was mostly just taking earlier code after that.

*Your response here.*
