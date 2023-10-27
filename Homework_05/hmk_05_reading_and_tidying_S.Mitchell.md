# HMK 5: Reading and tidying data

# Reading

- [R4DS Chapters 6-9](https://r4ds.hadley.nz/data-import)

# Data import

## Q1:

- Create a directory, within your main class directory, called `data`.

  - Note: in general, you should store raw data in a directory called
    `data`.

- Download the example file for Ch 9
  [here](https://pos.it/r4ds-students-csv). Save it inside the `ddata`
  directory.

- Use `read_csv()` to read the file to an R data frame. Follow the
  instructions in Ch 9 to format it properly. Follow the directions in
  Ch 9 to make sure the following are true:

  - Column names should be *syntactic*, meaning they don’t contain
    spaces.
  - N/A values should be represented with the R value `NA`, not the
    character “N/A”.
  - Data types (character vs factor vs numeric) should be appropriate.

  First, make a directory called ‘Data’ within the MICRO_575 directory
  using the mkdir command (I did this via terminal). The data that is
  stated to be in Ch 9 is actually in 8.2. Download the students CSV
  file into the created data directory. There are only two choices to
  save this file, as Page Source or Web Archive on safari. However,
  after some fiddling, I have realized that using chrome the data can be
  saved directly as a .csv file. To read the file into R, use the
  read_csv command and make sure that the file path is to the data
  directory that I created. We can now view this tibble.

``` r
library(tidyverse)
```

    ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ✔ dplyr     1.1.2     ✔ readr     2.1.4
    ✔ forcats   1.0.0     ✔ stringr   1.5.0
    ✔ ggplot2   3.4.3     ✔ tibble    3.2.1
    ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
    ✔ purrr     1.0.2     
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()
    ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
Students <- read_csv("/Users/sarinamitchell/SP_2023_Coursework/MICRO_575/Data/students.csv") 
```

    Rows: 6 Columns: 5
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: ","
    chr (4): Full Name, favourite.food, mealPlan, AGE
    dbl (1): Student ID

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
Students
```

    # A tibble: 6 × 5
      `Student ID` `Full Name`      favourite.food     mealPlan            AGE  
             <dbl> <chr>            <chr>              <chr>               <chr>
    1            1 Sunil Huffmann   Strawberry yoghurt Lunch only          4    
    2            2 Barclay Lynn     French fries       Lunch only          5    
    3            3 Jayendra Lyne    N/A                Breakfast and lunch 7    
    4            4 Leon Rossini     Anchovies          Lunch only          <NA> 
    5            5 Chidiegwu Dunkel Pizza              Breakfast and lunch five 
    6            6 Güvenç Attila    Ice cream          Lunch only          6    

To make sure we are representing N/A values with the R value NA, unlike
the default of only recognizing empty strings. So, we specify when
reading this file to acknowledge those values. We can now see that the
N/A value is changed to NA.

``` r
Students <- read_csv("/Users/sarinamitchell/SP_2023_Coursework/MICRO_575/Data/students.csv", na = c("N/A", ""))
```

    Rows: 6 Columns: 5
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: ","
    chr (4): Full Name, favourite.food, mealPlan, AGE
    dbl (1): Student ID

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
Students
```

    # A tibble: 6 × 5
      `Student ID` `Full Name`      favourite.food     mealPlan            AGE  
             <dbl> <chr>            <chr>              <chr>               <chr>
    1            1 Sunil Huffmann   Strawberry yoghurt Lunch only          4    
    2            2 Barclay Lynn     French fries       Lunch only          5    
    3            3 Jayendra Lyne    <NA>               Breakfast and lunch 7    
    4            4 Leon Rossini     Anchovies          Lunch only          <NA> 
    5            5 Chidiegwu Dunkel Pizza              Breakfast and lunch five 
    6            6 Güvenç Attila    Ice cream          Lunch only          6    

To make sure the column names are syntactic, we have to rename the
variables with backticks surrounding them so that they no longer contain
spaces.

``` r
Students |> 
  rename(
    student_id = `Student ID`,
    full_name = `Full Name`
  )
```

    # A tibble: 6 × 5
      student_id full_name        favourite.food     mealPlan            AGE  
           <dbl> <chr>            <chr>              <chr>               <chr>
    1          1 Sunil Huffmann   Strawberry yoghurt Lunch only          4    
    2          2 Barclay Lynn     French fries       Lunch only          5    
    3          3 Jayendra Lyne    <NA>               Breakfast and lunch 7    
    4          4 Leon Rossini     Anchovies          Lunch only          <NA> 
    5          5 Chidiegwu Dunkel Pizza              Breakfast and lunch five 
    6          6 Güvenç Attila    Ice cream          Lunch only          6    

We could repeat the above code to rename all of the variable with
snakecase; however, we can do this all at once to save time. But, we
will need to install the janitor package first. After installing
janitor, we see that we can quickly change all of the variables to have
underscores instead of having random formatting.

``` r
Students <- Students |> janitor::clean_names()
Students
```

    # A tibble: 6 × 5
      student_id full_name        favourite_food     meal_plan           age  
           <dbl> <chr>            <chr>              <chr>               <chr>
    1          1 Sunil Huffmann   Strawberry yoghurt Lunch only          4    
    2          2 Barclay Lynn     French fries       Lunch only          5    
    3          3 Jayendra Lyne    <NA>               Breakfast and lunch 7    
    4          4 Leon Rossini     Anchovies          Lunch only          <NA> 
    5          5 Chidiegwu Dunkel Pizza              Breakfast and lunch five 
    6          6 Güvenç Attila    Ice cream          Lunch only          6    

I realize there is a typo in the favorite food variable name, so I will
quickly fix this.

``` r
Students <- Students |> 
  rename(
    favorite_food = 'favourite_food'
  )
print(Students)
```

    # A tibble: 6 × 5
      student_id full_name        favorite_food      meal_plan           age  
           <dbl> <chr>            <chr>              <chr>               <chr>
    1          1 Sunil Huffmann   Strawberry yoghurt Lunch only          4    
    2          2 Barclay Lynn     French fries       Lunch only          5    
    3          3 Jayendra Lyne    <NA>               Breakfast and lunch 7    
    4          4 Leon Rossini     Anchovies          Lunch only          <NA> 
    5          5 Chidiegwu Dunkel Pizza              Breakfast and lunch five 
    6          6 Güvenç Attila    Ice cream          Lunch only          6    

Next, we need to change of a few of the variable types to make them
appropriate. We will change the meal_plan variable to a factor type, and
the age variable to a double using mutate while correcting the written
“five” in the dataset under the age column.

``` r
Students <- Students |>
  mutate(meal_plan = factor(meal_plan), 
         age = parse_number(if_else(age == "five", "5", age))) |>
  mutate(age = as.numeric(age))
print(Students)
```

    # A tibble: 6 × 5
      student_id full_name        favorite_food      meal_plan             age
           <dbl> <chr>            <chr>              <fct>               <dbl>
    1          1 Sunil Huffmann   Strawberry yoghurt Lunch only              4
    2          2 Barclay Lynn     French fries       Lunch only              5
    3          3 Jayendra Lyne    <NA>               Breakfast and lunch     7
    4          4 Leon Rossini     Anchovies          Lunch only             NA
    5          5 Chidiegwu Dunkel Pizza              Breakfast and lunch     5
    6          6 Güvenç Attila    Ice cream          Lunch only              6

## Q2

Find (or make) a data file of your own, in text format. Read it into a
well-formatted data frame.

# Tidying

Download the data set available at
<https://tiny.utk.edu/MICR_575_hmk_05>, and save it to your `data`
folder. **This is a real data set:** it is the output from the
evaluation forms for student colloquium speakers in the Microbiology
department. I have eliminated a few columns, changed some of the scores,
and edited comments, to protect student privacy, but the output is real.

First, open this .csv file with Microsoft Excel or a text reading app,
to get a sense of the structure of the document. It is weird.

Why is the file formatted so inconveniently? I have no idea, but I do
know that this is about an average level of inconvenient formatting for
real data sets you will find in the wild.

*Note: In theory, you can pass a URL to `read_csv()` and read the file
directly from the internet. In practice, that doesn’t seem to work for
this file. So you’ll want to download this file to your hard drive.*

We (me and my group member) believe the data type in the cells is based
on a JSON object (JavaScript Object Notation) and because of this it is
not meaningfully converted to a CSV file. These types of data files are
used to exchange information from web-based applications. As a result,
the file is inconveniently formatted. For example, the columns are
poorly named, there are unnecessary spacing in the rows, some columns
appear to have multiple names, and cells are empty. Particularly
problematic is that the actual data presented as numerical values
actually represent categorical data and we don’t actually know what the
true meaning of the data is.

## Q3a

Next, use `read_csv()` to read the data into a data frame. Note that
you’ll need to make use of some of the optional arguments. Use
`?read_csv` to see what they are.

*If you are struggling with this task, email me for hints.*

As we discussed in class, the correct shape depends on what you want to
do with the data. Use `pivot_longer()` to make the data frame longer, in
a way that makes sense.

``` r
library(tidyverse)
Colloquium_Assessment <- read_csv("/Users/sarinamitchell/SP_2023_Coursework/MICRO_575/Data/colloquium_assessment.csv", skip_empty_rows = TRUE)
```

    Rows: 36 Columns: 25
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: ","
    chr (25): StartDate, EndDate, Status, Progress, Duration (in seconds), Finis...

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
print(Colloquium_Assessment)
```

    # A tibble: 36 × 25
       StartDate             EndDate Status Progress Duration (in seconds…¹ Finished
       <chr>                 <chr>   <chr>  <chr>    <chr>                  <chr>   
     1  <NA>                  <NA>    <NA>   <NA>     <NA>                   <NA>   
     2 "Start Date"          "End D… "Resp… "Progre… "Duration (in seconds… "Finish…
     3  <NA>                  <NA>    <NA>   <NA>     <NA>                   <NA>   
     4 "{\"ImportId\":\"sta… "{\"Im… "{\"I… "{\"Imp… "{\"ImportId\":\"dura… "{\"Imp…
     5  <NA>                  <NA>    <NA>   <NA>     <NA>                   <NA>   
     6 "11/11/22 9:01"       "11/11… "0"    "100"    "18"                   "1"     
     7  <NA>                  <NA>    <NA>   <NA>     <NA>                   <NA>   
     8 "11/11/22 9:02"       "11/11… "0"    "100"    "31"                   "1"     
     9  <NA>                  <NA>    <NA>   <NA>     <NA>                   <NA>   
    10 "11/11/22 9:03"       "11/11… "0"    "100"    "109"                  "1"     
    # ℹ 26 more rows
    # ℹ abbreviated name: ¹​`Duration (in seconds)`
    # ℹ 19 more variables: RecordedDate <chr>, ResponseId <chr>,
    #   RecipientLastName <chr>, RecipientFirstName <chr>, RecipientEmail <chr>,
    #   ExternalReference <chr>, LocationLatitude <chr>, LocationLongitude <chr>,
    #   DistributionChannel <chr>, UserLanguage <chr>, Q4 <chr>, Q5 <chr>,
    #   Q6 <chr>, Q7 <chr>, Q8 <chr>, Q9 <chr>, Q10 <chr>, Q11 <chr>, Q12 <chr>

First, I’m going to go ahead and clean-up the names of the columns.

``` r
Colloquium_Assessment_clean <- Colloquium_Assessment |> janitor::clean_names()
Colloquium_Assessment
```

    # A tibble: 36 × 25
       StartDate             EndDate Status Progress Duration (in seconds…¹ Finished
       <chr>                 <chr>   <chr>  <chr>    <chr>                  <chr>   
     1  <NA>                  <NA>    <NA>   <NA>     <NA>                   <NA>   
     2 "Start Date"          "End D… "Resp… "Progre… "Duration (in seconds… "Finish…
     3  <NA>                  <NA>    <NA>   <NA>     <NA>                   <NA>   
     4 "{\"ImportId\":\"sta… "{\"Im… "{\"I… "{\"Imp… "{\"ImportId\":\"dura… "{\"Imp…
     5  <NA>                  <NA>    <NA>   <NA>     <NA>                   <NA>   
     6 "11/11/22 9:01"       "11/11… "0"    "100"    "18"                   "1"     
     7  <NA>                  <NA>    <NA>   <NA>     <NA>                   <NA>   
     8 "11/11/22 9:02"       "11/11… "0"    "100"    "31"                   "1"     
     9  <NA>                  <NA>    <NA>   <NA>     <NA>                   <NA>   
    10 "11/11/22 9:03"       "11/11… "0"    "100"    "109"                  "1"     
    # ℹ 26 more rows
    # ℹ abbreviated name: ¹​`Duration (in seconds)`
    # ℹ 19 more variables: RecordedDate <chr>, ResponseId <chr>,
    #   RecipientLastName <chr>, RecipientFirstName <chr>, RecipientEmail <chr>,
    #   ExternalReference <chr>, LocationLatitude <chr>, LocationLongitude <chr>,
    #   DistributionChannel <chr>, UserLanguage <chr>, Q4 <chr>, Q5 <chr>,
    #   Q6 <chr>, Q7 <chr>, Q8 <chr>, Q9 <chr>, Q10 <chr>, Q11 <chr>, Q12 <chr>

Next, I’m going to remove the first four rows of the columns as they are
uninformative and redundant.

``` r
Colloquium_Assessment_start_R4 <- Colloquium_Assessment_clean |>
  filter(!row_number() %in% c(1:4)) 
glimpse(Colloquium_Assessment_start_R4)
```

    Rows: 32
    Columns: 25
    $ start_date           <chr> NA, "11/11/22 9:01", NA, "11/11/22 9:02", NA, "11…
    $ end_date             <chr> NA, "11/11/22 9:02", NA, "11/11/22 9:02", NA, "11…
    $ status               <chr> NA, "0", NA, "0", NA, "0", NA, "0", NA, "0", NA, …
    $ progress             <chr> NA, "100", NA, "100", NA, "100", NA, "100", NA, "…
    $ duration_in_seconds  <chr> NA, "18", NA, "31", NA, "109", NA, "40", NA, "243…
    $ finished             <chr> NA, "1", NA, "1", NA, "1", NA, "1", NA, "1", NA, …
    $ recorded_date        <chr> NA, "11/11/22 9:02", NA, "11/11/22 9:02", NA, "11…
    $ response_id          <chr> NA, "R_2b14qToYhcmCVFD", NA, "R_2eVcizz1gWXK4gd",…
    $ recipient_last_name  <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
    $ recipient_first_name <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
    $ recipient_email      <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
    $ external_reference   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
    $ location_latitude    <chr> NA, "35.9539", NA, "35.8974", NA, "35.9452", NA, …
    $ location_longitude   <chr> NA, "-83.9357", NA, "-83.9425", NA, "-83.9435", N…
    $ distribution_channel <chr> NA, "anonymous", NA, "anonymous", NA, "anonymous"…
    $ user_language        <chr> NA, "EN", NA, "EN", NA, "EN", NA, "EN", NA, "EN",…
    $ q4                   <chr> NA, "1", NA, "1", NA, "1", NA, "1", NA, "1", NA, …
    $ q5                   <chr> NA, "2", NA, "2", NA, "2", NA, "2", NA, "2", NA, …
    $ q6                   <chr> NA, "4", NA, "3", NA, "2", NA, "2", NA, "2", NA, …
    $ q7                   <chr> NA, "5", NA, "5", NA, "4", NA, "4", NA, "5", NA, …
    $ q8                   <chr> NA, "5", NA, "5", NA, "3", NA, "5", NA, "5", NA, …
    $ q9                   <chr> NA, "4", NA, "5", NA, "3", NA, "4", NA, "4", NA, …
    $ q10                  <chr> NA, "4", NA, "5", NA, "3", NA, "4", NA, "5", NA, …
    $ q11                  <chr> NA, "4", NA, "5", NA, "5", NA, "5", NA, "5", NA, …
    $ q12                  <chr> NA, NA, NA, "Great presentation", NA, "Be very ca…

Select the odd rows to remove the empty rows and select the empty
columns to remove them.

``` r
Colloquium_Assessment_start_R4_odd <- seq_len(nrow(Colloquium_Assessment_start_R4)) %% 2
Colloquium_Assessment_start_R4_no_odd <- Colloquium_Assessment_start_R4[Colloquium_Assessment_start_R4_odd == 0, ] 
Colloquium_Assessment_start_R4_no_odd_no_empty <- subset(Colloquium_Assessment_start_R4_no_odd, select = -c(recipient_last_name:external_reference))
glimpse(Colloquium_Assessment_start_R4_no_odd_no_empty)
```

    Rows: 16
    Columns: 21
    $ start_date           <chr> "11/11/22 9:01", "11/11/22 9:02", "11/11/22 9:03"…
    $ end_date             <chr> "11/11/22 9:02", "11/11/22 9:02", "11/11/22 9:04"…
    $ status               <chr> "0", "0", "0", "0", "0", "8", "0", "0", "0", "0",…
    $ progress             <chr> "100", "100", "100", "100", "100", "100", "100", …
    $ duration_in_seconds  <chr> "18", "31", "109", "40", "243", "28", "15", "33",…
    $ finished             <chr> "1", "1", "1", "1", "1", "1", "1", "1", "1", "1",…
    $ recorded_date        <chr> "11/11/22 9:02", "11/11/22 9:02", "11/11/22 9:04"…
    $ response_id          <chr> "R_2b14qToYhcmCVFD", "R_2eVcizz1gWXK4gd", "R_2an9…
    $ location_latitude    <chr> "35.9539", "35.8974", "35.9452", "35.9452", "35.8…
    $ location_longitude   <chr> "-83.9357", "-83.9425", "-83.9435", "-83.9435", "…
    $ distribution_channel <chr> "anonymous", "anonymous", "anonymous", "anonymous…
    $ user_language        <chr> "EN", "EN", "EN", "EN", "EN", "EN", "EN", "EN", "…
    $ q4                   <chr> "1", "1", "1", "1", "1", "1", "1", "1", "2", "1",…
    $ q5                   <chr> "2", "2", "2", "2", "2", "1", "2", "2", "1", "2",…
    $ q6                   <chr> "4", "3", "2", "2", "2", "2", "2", "2", "2", "3",…
    $ q7                   <chr> "5", "5", "4", "4", "5", "4", "5", "2", "5", "5",…
    $ q8                   <chr> "5", "5", "3", "5", "5", "5", "5", "3", "4", "5",…
    $ q9                   <chr> "4", "5", "3", "4", "4", "5", "4", "2", "5", "5",…
    $ q10                  <chr> "4", "5", "3", "4", "5", "5", "5", "5", "3", "5",…
    $ q11                  <chr> "4", "5", "5", "5", "5", "5", "5", "4", "4", "5",…
    $ q12                  <chr> NA, "Great presentation", "Be very careful when m…

Finally, we can pivot the data.

``` r
Colloquium_Assessment_tidy <- Colloquium_Assessment_start_R4_no_odd_no_empty |>
  pivot_longer(cols = starts_with("q"),
               names_to = "question",
               values_to = "answer")
glimpse(Colloquium_Assessment_tidy)
```

    Rows: 144
    Columns: 14
    $ start_date           <chr> "11/11/22 9:01", "11/11/22 9:01", "11/11/22 9:01"…
    $ end_date             <chr> "11/11/22 9:02", "11/11/22 9:02", "11/11/22 9:02"…
    $ status               <chr> "0", "0", "0", "0", "0", "0", "0", "0", "0", "0",…
    $ progress             <chr> "100", "100", "100", "100", "100", "100", "100", …
    $ duration_in_seconds  <chr> "18", "18", "18", "18", "18", "18", "18", "18", "…
    $ finished             <chr> "1", "1", "1", "1", "1", "1", "1", "1", "1", "1",…
    $ recorded_date        <chr> "11/11/22 9:02", "11/11/22 9:02", "11/11/22 9:02"…
    $ response_id          <chr> "R_2b14qToYhcmCVFD", "R_2b14qToYhcmCVFD", "R_2b14…
    $ location_latitude    <chr> "35.9539", "35.9539", "35.9539", "35.9539", "35.9…
    $ location_longitude   <chr> "-83.9357", "-83.9357", "-83.9357", "-83.9357", "…
    $ distribution_channel <chr> "anonymous", "anonymous", "anonymous", "anonymous…
    $ user_language        <chr> "EN", "EN", "EN", "EN", "EN", "EN", "EN", "EN", "…
    $ question             <chr> "q4", "q5", "q6", "q7", "q8", "q9", "q10", "q11",…
    $ answer               <chr> "1", "2", "4", "5", "5", "4", "4", "4", NA, "1", …

## Q3b

Finally, calculate this student’s average score for each of questions
7-10.

First, we select the subset of questions 7-10 from our dataframe and
change the answers to numeric values so they can be used in
calculations.

``` r
questions_7_8_9_10 <- Colloquium_Assessment_tidy |>
  filter(question == "q7" | question == "q8" | question == "q9" | question == "q10") |>
  arrange(question)
questions_7_8_9_10_num <- transform(questions_7_8_9_10, answer = as.numeric(questions_7_8_9_10$answer))
```

Now we can find the average.

``` r
average_questions_7_8_9_10 <- summarize(questions_7_8_9_10_num, 
                                        average_score = mean(answer),
                                        .by = question)
print(average_questions_7_8_9_10)
```

      question average_score
    1      q10        4.4375
    2       q7        4.5000
    3       q8        4.6250
    4       q9        4.3125

## Important note about file paths in Quarto documents

When you render a Quarto document, RStudio spins up a new instance of R,
which is separate from the instance of R that you cna interact with. The
working directory for this instance of R is whatever directory your
Quarto document is saved in.

If your quarto document is saved in the same directory as your RStudio
project (e.g., `MICR_475`), then there is no difference between your
interactive working directory and the working directory for your Quarto
document.

However, if your homeworks are saved in a `HMK` directory, then the
Quarto working directory will be `HMK`. To access the saved `.csv` file,
`read_csv()` will need to look *up* one directory and then go back
*down* into `HMK`. `..` means “up one directory”, so you would need to
use `read_csv("../colloquium_assessment.csv")` instead of
`read_csv("colloquium_assessment.csv")`.
